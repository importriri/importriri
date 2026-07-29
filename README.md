# Sid Ahmed Riri

**Industrial automation technician building reliable Linux infrastructure,
controlled research environments and shop-floor software.**

I bring more than eight years of hands-on work around electromechanical and
control systems. I am extending that background into IT/OT security with
software used in production and infrastructure that must pass repeatable tests
on real hardware.

I document failures as carefully as successful outcomes. The recurring rule is
simple: **observe the result, fail closed and keep the proof.**

[Engineering portfolio](PORTFOLIO.md) ·
[LinkedIn](https://www.linkedin.com/in/importriri/)

## Selected work

### Reproducible Arch/KVM/VFIO hypervisor pipeline

A three-repository pipeline for an encrypted, Secure-Booted laptop hypervisor
with five separated trust domains and one explicitly controlled passthrough
GPU:

```text
arch-bootstrap  →  privatestack-ansible  →  arch-hypervisor-lab
base host          reconciled target        architecture and evidence
```

- [`arch-bootstrap`](https://github.com/importriri/arch-bootstrap) builds the
  LUKS2/Btrfs/`linux-hardened` base and writes a verified storage hand-off.
- [`privatestack-ansible`](https://github.com/importriri/privatestack-ansible)
  reconciles host state, libvirt networks, VFIO policy and the interactive lab.
- [`arch-hypervisor-lab`](https://github.com/importriri/arch-hypervisor-lab)
  records architecture, failure analysis, compatibility policy and sanitized
  evidence.

The current release candidate has passed its complete two-disk transaction
inside a disposable Arch VM. Nitro single-disk clean-install acceptance and
Predator dual-disk portability acceptance remain separate physical gates; the
project does not call either laptop fully verified before those gates pass.

### AutoFillSuite — browser automation that verifies its own work

[`AutoFillSuite`](https://github.com/importriri/AutoFillSuite) registers cable
labels on an API-less supplier portal using Java/Swing and `java.awt.Robot`.
The robot's clicks are never treated as proof: every run is compared with the
portal's fresh CSV export before it turns green.

The current candidate adds a queued two-QR scanning workflow alongside range
registration. It refuses inverted pairs, mismatches and unsafe duplicates,
stops when the operator moves the mouse, journals work before transmission and
can resume verification after an interrupted session. The cockpit, compact HUD
and embedded Italian/English manual ship in one standalone Java 8 JAR with no
runtime dependencies.

### Etichette Custom — one label model from preview to printer

[`etichette-custom`](https://github.com/importriri/etichette-custom) is a
zero-dependency Java/Swing composer for serialized QR labels on a Datamax
thermal printer.

Its current candidate adds a complete tool rail, property inspector, reusable
templates, rotated elements and multiline vector text. Preview, PNG, SVG, PDF
and Windows printing still share one layout model; printer calibration, page
orientation and multiple DPI/scaling profiles are exercised as part of the
release boundary. Serial exhaustion remains fail-closed before label one, and
the embedded operator manual follows the same tested delivery path as the UI.

## How I work

- **Observed state over declared state.** A command completing is not proof that
  the intended system state exists.
- **Failure becomes a contract.** Reproduced bugs become tests, refusal paths or
  documented hardware gates.
- **Disk-changing work requires explicit identity.** Devices, mappers, commits
  and evidence are bound before any write.
- **Public evidence excludes private data.** Reports preserve technical value
  without publishing serials, network identities or production records.
- **Small dependencies, clear boundaries.** Shop-floor tools remain deployable;
  infrastructure roles and transactions remain independently testable.

## Technical direction

IT/OT security · Linux and infrastructure automation · KVM/libvirt/VFIO ·
Bash and Ansible · LUKS2/Btrfs/Secure Boot · network isolation · Java/Swing ·
industrial controls and shop-floor operations

The detailed case studies, current validation boundaries and useful interview
entry points are in [`PORTFOLIO.md`](PORTFOLIO.md).
