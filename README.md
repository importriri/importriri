# Sid Ahmed Riri

Industrial automation technician moving toward Linux infrastructure and IT/OT
security, with more than eight years of hands-on work around production
equipment, electrical systems and controls.

I build small tools for real shop-floor problems and a reproducible Arch/KVM
lab for virtualization and security work. I am interested in Linux system
administration, infrastructure automation and technical roles where industrial
experience is useful.

[Technical portfolio](PORTFOLIO.md) ·
[LinkedIn](https://www.linkedin.com/in/importriri/)

## Selected projects

### Arch/KVM/VFIO laptop lab

```text
arch-bootstrap  ->  privatestack-ansible  ->  arch-hypervisor-lab
base install        host configuration        documentation and test records
```

- [`arch-bootstrap`](https://github.com/importriri/arch-bootstrap) installs an
  encrypted Arch base with Btrfs, systemd-boot, Secure Boot and
  `linux-hardened`.
- [`privatestack-ansible`](https://github.com/importriri/privatestack-ansible)
  configures KVM, five network domains, nftables isolation, VFIO and the
  optional Sway/Looking Glass desktop.
- [`arch-hypervisor-lab`](https://github.com/importriri/arch-hypervisor-lab)
  contains the setup notes, hardware matrix and failure investigations.

The public repositories contain the tested foundation. Later VM lifecycle and
service work stays local until the remaining Nitro and Predator checks pass.

### AutoFillSuite

[`AutoFillSuite`](https://github.com/importriri/AutoFillSuite) is a Java/Swing
tool used to register cable labels on a supplier portal with no API. It drives
the browser with `java.awt.Robot`, then checks the result against a fresh CSV
export from the portal. It supports ranges, queued QR scans, recovery after an
interrupted run and an embedded bilingual manual.

### Etichette Custom

[`etichette-custom`](https://github.com/importriri/etichette-custom) is a
Java/Swing label editor with its own QR encoder. Preview, PNG, SVG, PDF and
printing share one layout model. It includes serial-number limits, printer
calibration, vector text and bilingual manuals.

## Working approach

- Check the resulting state instead of relying only on a successful command.
- Turn reproduced failures into tests or explicit refusal paths.
- Keep production data and private machine identifiers out of public fixtures.
- Keep deployment simple on locked-down shop-floor PCs.
- Separate hardware-dependent claims from tests that can run in CI.

## Technical focus

Linux · Bash · Ansible · KVM/libvirt/VFIO · LUKS2/Btrfs/Secure Boot · network
isolation · Java/Swing · industrial automation · IT/OT security
