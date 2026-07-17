### Ciao, I'm Sid 👋

OT/industrial-automation technician moving into **IT/OT security**.
8+ years hands-on with electromechanical and control systems — now building
the software and security side to match, on my own hardware, from scratch.

I learn by building things that have to *actually work*, and I document the
bugs as much as the wins. **Verify, don't trust.**

**What I'm building right now**

- 🖥️ **[arch-hypervisor-lab](https://github.com/importriri/arch-hypervisor-lab)**
  — a four-domain GPU-passthrough hypervisor on a single laptop: clean
  gaming, dirty gaming, dev, and an air-gapped malware lab. Includes a
  laptop-specific VFIO freeze bug that took ~2 months to solve.
- 🧰 **[arch-bootstrap](https://github.com/importriri/arch-bootstrap)** — the
  test-driven Arch installer that lays the encrypted foundation the lab
  stands on: LUKS2, Btrfs, Secure Boot, `linux-hardened`. Every destructive
  step is guarded and tested against loop devices before it touches a disk.
- 🧱 **[privatestack-ansible](https://github.com/importriri/privatestack-ansible)**
  — the Ansible bricks that assemble the lab on the bootstrapped host: one
  brick per job, discovery-driven CI, invariant tests that pin every bug
  already paid for. Published stage by stage.

Three repos, one system: **arch-bootstrap** installs the encrypted base,
**privatestack-ansible** is the Ansible bricks that build the
lab on top of it — the core bricks plus optional ones to bolt on more
services — and **arch-hypervisor-lab** is the writeup that ties it together:
how to drive Ansible to build it, the real hardware problems, the setup.

**Also shipped:** [AutoFillSuite](https://github.com/importriri/AutoFillSuite)
— a Java/Swing desktop tool (`java.awt.Robot`) running in production at my
workplace, automating barcode-driven web form filling.

**Where this is going**

Offensive and defensive security for industrial control systems — work that
needs a legal, isolated place to run, which is exactly what the lab is for.
NIS2 is making the OT + IT security profile scarce; I'm building to fill
that gap.

**Toolbox:** Bash · Linux (Arch) · Ansible · KVM/VFIO · Btrfs/LUKS2 ·
nftables · OT/ICS (Modbus, Purdue model, IEC 62443) · Java

---

*Evening Perito Informatico diploma, building the pipeline above one
verified phase at a time.*

📫 [LinkedIn](https://www.linkedin.com/in/importriri/)
