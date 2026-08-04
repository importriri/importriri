# Technical portfolio

This file gives more context for the projects linked from my GitHub profile.
The repositories contain the code and current setup instructions.

## Background

I have more than eight years of experience as an industrial automation
technician around electromechanical systems, production equipment and control
panels. My current direction combines that background with Linux,
virtualization, infrastructure automation and IT/OT security.

The projects below started from practical constraints: locked-down Windows
workstations, supplier software without APIs, printer drivers that do not honor
page geometry, and laptop hardware that behaves differently from generic VFIO
guides.

## Arch/KVM/VFIO laptop lab

### Goal

Build a repeatable Arch Linux hypervisor on two Acer laptops with encrypted
storage, Secure Boot, a hardened kernel, separate network domains and one
NVIDIA dGPU assigned to a reviewed workload VM at a time.

### Repository split

```text
arch-bootstrap
    disk layout, LUKS2, Btrfs, systemd-boot and the base system
          |
          v
privatestack-ansible
    KVM, hardware profiles, networks, isolation, VFIO and the local desktop
          |
          v
arch-hypervisor-lab
    architecture, setup order, compatibility notes and failure reports
```

This split keeps disk installation, host configuration and documentation
independently testable.

### Network and GPU model

```text
clean       trusted accounts and games
dirty       mods and higher-risk software
dev         development and 3D work
lab         isolated defensive research
services    private service VMs
```

Only the four workload domains may receive the dGPU. The services domain does
not take part in GPU rotation. The host uses the iGPU for the required Sway
desktop, and the Looking Glass host transport is reconciled after that desktop.
The headless foundation remains a recovery stage, not the normal laptop target.

### Problems that changed the implementation

A passthrough guest could freeze the host before useful logs were written. The
problem was first isolated on the Nitro and later reproduced on the Predator.
`pcie_port_pm=off` was the effective fix on both machines.

Looking Glass also produced a convincing false success by showing the SPICE
recovery display. The acceptance check now includes the client log and kvmfr
state instead of relying only on the visible window.

A disposable storage run exposed an ambiguous `findmnt` check and a desktop
automounter later mounted the test mapper under `/run/media`. Cleanup now binds
the mapper to the expected loop device and discovers mounts by source before
teardown.

### Current public state

The public `main` branches contain the installer, complete laptop host target,
image factory, explicit VM lifecycle and service registration and configuration
roles. Windows guest preparation remains a documented manual step. The Nitro
bootstrap-to-stage-2 host path has reproduced immediate `changed=0`; full
pipeline compatibility remains pending until the frozen clean-install,
guest-cycle and shared-memory capture evidence is complete.

## AutoFillSuite

### Problem

A supplier portal used on the shop floor has no API. Registering a large batch
of labels by hand is slow, but blind GUI automation can type into the wrong
place or continue after the portal fails to save a record.

### Implementation

AutoFillSuite is a Java 8/Swing desktop application. `java.awt.Robot` performs
the browser interaction, while a separate verification path requests a fresh
CSV export and compares the portal's records with the attempted run.

The application supports serial ranges and queued label/lot QR pairs. It checks
scan order, malformed pairs and duplicates before queueing. Moving the mouse
stops the robot. A journal records attempted sends so an interrupted session can
return for verification after restart.

### Practical decisions

The application remains one standalone JAR because the production PC is easier
to support without an installer or dependency manager. The Italian and English
manuals are embedded in the package. An offline test page reproduces the field
order, save behavior and append-only CSV export without using portal assets or
production data.

## Etichette Custom

### Problem

Operators need serialized QR labels whose preview, exported files and physical
print agree. The printer driver may replace the requested label page with A4,
and a serial counter must never wrap into duplicate labels.

### Implementation

Etichette Custom is a Java 8/Swing editor. Text and QR elements are stored in a
single label model. Preview, PNG, SVG, PDF and the Java print queue all use the
same layout code.

The QR encoder is part of the project. Text is converted to vector outlines
before export, and the print window exposes page size, orientation, offsets and
scale. A calibration sheet helps separate page-format problems from mechanical
registration errors.

Before printing, the serial-number window checks whether the complete run fits.
If it does not, printing stops before the first label. Logs fall back to a local
directory when a configured network path is unavailable and show that fallback
to the operator.

### Testing

The test programs cover QR structure and frozen matrices, serial limits,
layout persistence, exporters, print geometry, manual packaging, startup and
multiple display scaling profiles. Optional Python scripts compare matrices
with Segno and decode generated QR images with zxing-cpp.

## Direction

I am looking for work around Linux infrastructure, systems administration,
virtualization, technical operations or IT/OT security where production-floor
experience and careful troubleshooting are useful.

[Back to profile](README.md) ·
[LinkedIn](https://www.linkedin.com/in/importriri/)
