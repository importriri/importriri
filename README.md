### Ciao, I'm Sid 👋

OT/industrial-automation technician moving into IT/OT security.
8+ years hands-on with electromechanical and control systems — now building
the software and security side to match, on my own hardware, from scratch.

I learn by building things that have to actually work, and I document the
bugs as much as the wins. Verify, don't trust.

**What I'm building right now**

- 🧰 **[arch-bootstrap](https://github.com/importriri/arch-bootstrap)** — a
  test-driven Arch Linux installer in bash (LUKS2, Btrfs, Secure Boot,
  `linux-hardened`). Every destructive step is guarded and tested against
  loop devices before it ever touches a disk.
- 🖥️ **[arch-hypervisor-lab](https://github.com/importriri/arch-hypervisor-lab)** —
  a four-domain GPU-passthrough hypervisor on a single laptop: clean gaming,
  dirty gaming, dev, and an air-gapped malware lab. Includes a laptop-specific
  VFIO freeze bug that took ~2 months to solve.

These two are one pipeline: the installer builds the base, Ansible roles
configure it, the lab is where the security work happens.

**Where this is going**

Offensive and defensive security for industrial control systems (PLCs) —
the kind of work that needs a legal, isolated place to run. That's what the
lab is for. NIS2 is making the OT + IT security profile scarce; I'm building
to fill exactly that gap.

**Toolbox:** Bash · Linux (Arch) · Ansible · KVM/VFIO · Btrfs/LUKS2 ·
nftables · OT/ICS (Modbus, Purdue model, IEC 62443) · Java

---

*Currently: evening Perito Informatico diploma + building the pipeline above,
one verified phase at a time.*
