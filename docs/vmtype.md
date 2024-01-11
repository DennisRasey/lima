# vmType

Lima supports two ways of running guest machines:
- [qemu](#qemu)
- [vz](#vz)

## QEMU
"qemu" option makes use of QEMU to run guest operating system. 
This option is used by default if "vmType" is not set.

## VZ
> **Warning**
> "vz" mode is experimental

| :zap: Requirement | Lima >= 0.14, macOS >= 13.0 |
|-------------------|-----------------------------|

"vz" option makes use of native virtualization support provided by macOS Virtualization.Framework.

An example configuration:
```yaml
# Example to run ubuntu using vmType: vz instead of qemu (Default)
vmType: "vz"
images:
- location: "https://cloud-images.ubuntu.com/releases/22.04/release/ubuntu-22.04-server-cloudimg-amd64.img"
  arch: "x86_64"
- location: "https://cloud-images.ubuntu.com/releases/22.04/release/ubuntu-22.04-server-cloudimg-arm64.img"
  arch: "aarch64"
mounts:
  - location: "~"
mountType: "virtiofs"
```

### Caveats
- "vz" option is only supported on macOS 13 or above
- Virtualization.framework doesn't support running "intel guest on arm" and vice versa

### Known Issues
- "vz" doesn't support `legacyBoot: true` option, so guest machine like centos-stream, archlinux, oraclelinux will not work
- Host to guest networking (`networks` section in lima yaml) is not supported
- When running lima using "vz", `${LIMA_HOME}/<INSTANCE>/serial.log` will not contain kernel boot logs
