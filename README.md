### Ciao, I'm Sid 👋

OT/industrial-automation technician moving into **IT/OT security**.
8+ years hands-on with electromechanical and control systems — now building
the software and security side to match, on my own hardware, from scratch.

I learn by building things that have to *actually work*, and I document the
bugs as much as the wins. **Verify, don't trust.**

**What I'm building right now**

- 🖥️ **[arch-hypervisor-lab](https://github.com/importriri/arch-hypervisor-lab)**
  — a five-domain GPU-passthrough hypervisor pipeline for two target
  laptops: clean gaming, dirty gaming, dev, an isolated malware lab, and a
  services domain. Includes a machine-readable hardware compatibility matrix,
  sanitized evidence reports, and a
  laptop-specific VFIO freeze bug that took ~2 months to solve — and the
  Looking Glass setup with **no dummy plug** (the only port wired to the
  dGPU is broken), three writeups of failure included.
- 🧰 **[arch-bootstrap](https://github.com/importriri/arch-bootstrap)** — the
  test-driven Arch installer that lays the encrypted foundation the lab
  stands on: LUKS2, Btrfs, Secure Boot, `linux-hardened`. Every destructive
  step is guarded and tested against loop devices before it touches a disk.
- 🧱 **[privatestack-ansible](https://github.com/importriri/privatestack-ansible)**
  — the Ansible bricks that assemble the lab on the bootstrapped host: one
  brick per job, auto-detected Nitro RTX 3060 / Predator RTX 3070 profiles,
  five reconciled libvirt networks, discovery-driven CI, and invariant tests
  that pin every bug already paid for. Published stage by stage.

Three repos, one tested pipeline: **arch-bootstrap** installs the encrypted base,
**privatestack-ansible** is the Ansible bricks that build the
lab on top of it — the core bricks plus optional ones to bolt on more
services — and **arch-hypervisor-lab** is the writeup that ties it together:
how to drive Ansible to build it, the real hardware problems, the setup,
and the evidence required before a laptop is called compatible.

**Also shipped — two zero-dependency Java/Swing tools for the shop floor I
work on:**

- **[AutoFillSuite](https://github.com/importriri/AutoFillSuite)** — drives an
  API-less supplier portal with `java.awt.Robot` and verifies every run
  against the portal's own CSV export. In daily production use.
- **[etichette-custom](https://github.com/importriri/etichette-custom)** —
  designs, serializes and prints QR labels on a Datamax thermal printer.
  The QR encoder is written from scratch and proven with 576/576 round-trips
  against two independent decoders.

**Where this is going**

Offensive and defensive security for industrial control systems — work that
needs a legal, isolated place to run, which is exactly what the lab is for.
NIS2 is making the OT + IT security profile scarce; I'm building to fill
that gap.

**Toolbox:** Bash · Linux (Arch) · Ansible · KVM/VFIO · Btrfs/LUKS2 ·
nftables · OT/ICS (Modbus, Purdue model, IEC 62443) · Java

---

*Evening Perito Informatico diploma, building the pipeline above one
verified phase at a time — component success is recorded separately from a
full clean-install pipeline pass.*

📫 [LinkedIn](https://www.linkedin.com/in/importriri/)
