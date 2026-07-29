# Engineering portfolio

This is a technical brief, not a duplicate résumé. Each case study starts with
a real constraint, explains the engineering choice and points to evidence. The
linked repositories remain the source of truth.

## Profile

I am an industrial-automation technician with more than eight years of
hands-on experience around electromechanical systems, production equipment and
control environments. My current direction is the intersection of that OT
background with Linux infrastructure, automation and IT/OT security.

The projects below are built around constraints I meet in practice:

- old or locked-down production PCs;
- software with no API;
- hardware whose behavior differs from documentation;
- changes that can erase data or stop a workstation from booting;
- public documentation that must not expose production or personal data.

The common engineering pattern is: establish identity, perform the smallest
controlled change, observe the resulting state and keep enough evidence to
repeat or reject the claim.

## Case study 1 — reproducible laptop hypervisor pipeline

### Goal

Build a repeatable Arch Linux hypervisor for two Acer laptops, with encrypted
storage, Secure Boot, a hardened kernel, separated network domains and one
NVIDIA dGPU assigned to one reviewed guest at a time.

The project is divided across three repositories because the responsibilities
have different failure and release boundaries:

```text
arch-bootstrap
    physical disk layout, LUKS2, Btrfs, systemd-boot, linux-hardened
          ↓ verified storage contract
privatestack-ansible
    host reconciliation, hardware profiles, networks, VFIO and lab target
          ↓ sanitized acceptance evidence
arch-hypervisor-lab
    architecture, operator protocol, compatibility policy and failure reports
```

Repositories:

- [`arch-bootstrap`](https://github.com/importriri/arch-bootstrap)
- [`privatestack-ansible`](https://github.com/importriri/privatestack-ansible)
- [`arch-hypervisor-lab`](https://github.com/importriri/arch-hypervisor-lab)

### Constraints

- Acer Nitro 5 with RTX 3060 Mobile and one internal disk;
- Acer Predator Helios 300 with RTX 3070 Mobile and two internal disks;
- host graphics must remain on the iGPU while the dGPU belongs to VFIO;
- the only physical output wired to the dGPU is unusable, so guest capture must
  work without relying on a dummy plug;
- five trust domains need stable network identities and explicit reachability;
- VM storage must be demonstrably located on the declared encrypted filesystem;
- Windows guest setup and private images must stay outside the public
  automation and evidence bundle.

### Architecture

The final host has two intentional targets:

- **foundation** — headless KVM/VFIO, storage, networking, isolation and GPU
  policy;
- **lab** — foundation plus the local Sway cockpit and Looking Glass host
  transport.

Workload creation is kept outside host reconciliation. Creating, resetting or
starting a VM depends on image state, capacity and exclusive GPU ownership; a
routine Ansible rerun must never imply those decisions.

The public network domains are:

```text
clean       trusted accounts and workloads
dirty       higher-risk software and mods
dev         development and 3D work
lab         controlled security research, no uplink by default
services    service VMs with reviewed exposure only
```

### Storage hand-off

Stage 1 writes `/etc/privatestack/bootstrap-storage.yml` only after observing
the mounted result:

```text
single disk:     /dev/mapper/cryptroot  Btrfs fsroot /@vm
second VM disk:  /dev/mapper/cryptvm    Btrfs fsroot /
mountpoint:      /var/lib/libvirt/images
```

Stage 2 independently compares that declaration with live `findmnt`, Btrfs and
No_COW evidence before creating the Hyperlab tree. A plausible configuration
file is not accepted as proof of the mounted filesystem.

### Failures that changed the design

- A hard host freeze during GPU work was isolated on the Nitro through boot
  parameter experiments. `pcie_port_pm=off` was the effective fix; the Predator
  later reproduced the failure without it and confirmed the same correction.
- Looking Glass initially appeared successful while showing the recovery SPICE
  display rather than shared-memory capture. Client logs and kvmfr state became
  part of the acceptance boundary.
- A mount check used an ambiguous `findmnt` operand. A real disposable-VM run
  exposed the problem, and every gate now uses explicit targets.
- After that failed run, a desktop automounter mounted the test mapper below
  `/run/media`. Cleanup now proves mapper-to-loop identity, discovers mounts by
  source and refuses unrelated devices.

The failure reports are indexed under the
[`problems/`](https://github.com/importriri/arch-hypervisor-lab/tree/main/problems)
directory.

### Current verification boundary

The complete disposable two-disk transaction passed on stage-1 commit:

```text
1f017cc0e74e3462468667e7a3d3dc574f928cb3
```

The retained raw log has SHA-256:

```text
5cfc465221373e5550a26189b0f0322c9f81624a204ff0f01ebd4a27761c4f92
```

That run proved real loop, GPT, LUKS2/argon2id, device-mapper, Btrfs, dedicated
VM-store adoption, contract creation and complete teardown. It does **not**
replace the remaining physical gates:

1. Nitro single-disk clean installation, Secure Boot state and two successful
   `linux-hardened` boots;
2. frozen Ansible foundation/lab acceptance and idempotence;
3. Predator dual-disk portability using the same accepted commits.

This separation is intentional: a component success is useful evidence, but it
is not promoted into a full compatibility claim.

## Case study 2 — AutoFillSuite

### Problem

A supplier portal used on the shop floor has no API. Registering hundreds of
cable labels manually is repetitive, but blind GUI automation would create a
new trust problem: a robot can press keys without proving that the portal saved
the intended records.

### Solution

[`AutoFillSuite`](https://github.com/importriri/AutoFillSuite) is a Java 8/Swing
desktop tool built with only the JDK. `java.awt.Robot` drives the browser, while
a separate verification path downloads the portal's fresh CSV export and
compares the complete result with the attempted run.

A run turns green only when the external record agrees.

### Current operator workflows

**Range registration** derives a serial interval from one scanned label and a
quantity, then sends one controlled browser transaction per code.

**Queued scanning** accepts a label QR and a batch QR for each item. The pair is
validated before it enters the queue; the fields clear immediately so the
operator can continue scanning while a worker drains the queue. Continuous and
block release remain explicit choices.

The candidate now refuses:

- reversed label/batch scans;
- malformed or mismatched pairs;
- duplicates that would make the intended run ambiguous;
- transmission while the operator is still moving the mouse;
- a green result for queued rows that the portal export did not cover.

Verification begins only after the first genuine quiet period, not in the
middle of a scan burst. Moving the mouse remains an immediate physical stop.

### Recovery and deployment

Every attempted send is journaled before it is considered complete. An
interrupted session can reopen with pending work offered for verification
instead of silently disappearing. Daily reports, verification logs, the compact
HUD and the full cockpit share the same state model.

The Italian and English manuals are embedded in the JAR and rendered through a
tested path, so documentation cannot be omitted by copying only the executable.
The tool remains one standalone Java 8 JAR with no runtime downloads.

### Verification

The current candidate is covered by focused suites for:

- export comparison and duplicate semantics;
- fresh-download detection and retry behavior;
- worker cancellation and interrupted verification;
- scan-order, mismatch and queue guards;
- journal/report round-trips and recovery;
- table state that must never invent a green result;
- icon, glyph, text-fit and spinner behavior;
- embedded manual rendering;
- first launch and saved-settings restart under a real X display.

The public fixtures and offline mock portal preserve the interaction contract
while replacing production identifiers and records with synthetic data.

## Case study 3 — Etichette Custom

### Problem

Operators need to design and print serialized QR labels on a Datamax thermal
printer. Preview, export and print must remain identical, printer drivers must
not silently rescale the page, and exhausting the numeric part of a serial must
never wrap into a duplicate label.

### Solution

[`etichette-custom`](https://github.com/importriri/etichette-custom) is a
zero-dependency Java 8/Swing application with one shared vector layout and five
outputs:

```text
preview · PNG · SVG · PDF · Windows print queue
```

The QR encoder is implemented inside the project rather than delegated to a
runtime library.

### Current composer

The current candidate turns the original fixed form into a complete label
composer:

- a tool rail creates text, QR and serial-aware elements;
- a property inspector edits geometry, rotation, typography and content;
- reusable templates preserve reviewed layouts;
- multiline text and rotated elements remain vector shapes;
- selection handles and warnings use the same physical coordinate system as
  export and print.

`LabelLayout` remains the single source of geometry. Preview, raster export,
SVG, PDF and print are backends of the same model, not separate reimplementations.
Text becomes vector outlines before a backend sees it, so PDF and SVG need no
embedded font and printed geometry cannot drift from the preview's text model.

### Printing boundary

The Windows print path records explicit page size and orientation and includes a
calibration workflow for the real printer/driver combination. Tests cover the
same layouts at multiple DPI and UI scaling profiles so a visually acceptable
96-DPI window is not mistaken for proof at 144 DPI or 150% scale.

Serial exhaustion is evaluated before output starts. If the remaining numeric
window cannot hold the requested run, the operation stops before label one.
Logging still degrades to a visible local fallback when a network destination
is unavailable rather than blocking production or hiding the loss of the
preferred path.

The bilingual operator manual is embedded and tested with the application.

### Verification

The candidate exercises:

- QR structure, capacities, modes, masks and error-correction levels;
- independent decoder round-trips and frozen reference matrices;
- serial-window boundaries and fail-closed exhaustion;
- layout persistence, templates, rotation and multiline geometry;
- PNG physical metadata, SVG structure and byte-correct PDF offsets;
- preview-to-raster QR module agreement;
- print page geometry, orientation and calibration;
- manual rendering, logging fallback and first/saved startup;
- multiple DPI and Windows look-and-feel/scaling paths.

A font-dependent test that assumed bold text must always be wider than regular
text was replaced with the property the renderer actually needs: the two vector
outlines must differ. That keeps the test portable across valid fontconfig
installations without weakening the layout contract.

## Working style

The strongest pattern across the projects is not a specific technology. It is
how uncertainty is handled:

1. identify what can actually be observed;
2. refuse ambiguous or unsafe state;
3. reproduce the failure in the smallest useful environment;
4. turn the result into a test, invariant or explicit manual gate;
5. publish enough context to audit the claim without exposing private data.

## Current direction

The best fit is work where industrial context matters as much as software:
IT/OT security, Linux infrastructure, automation, virtualization, technical
operations and engineering support around production systems.

Useful discussion starting points:

- why host reconciliation stops before VM lifecycle decisions;
- how to prove an encrypted mount is the filesystem you intended;
- why a GUI robot must verify against an external record;
- how scan queues remain fast without trusting order or timing;
- how one vector model prevents preview-to-print drift;
- how hardware evidence should be promoted from component result to supported
  compatibility claim.

[Back to profile](README.md) ·
[LinkedIn](https://www.linkedin.com/in/importriri/)
