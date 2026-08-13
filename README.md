# Reproducible TWRP recovery build for Xiaomi Pad 6S Pro (sheng)

This repository builds a source-level replacement for the Xiaomi Pad 6S
Pro TWRP recovery whose Boot Control HAL reports three slots and fails on
the cross-UFS-LUN `multiimgoem` / `multiimgqti` layout. It also preserves
the higher-priority `ABS_MT_TRACKING_ID=-1` touch-release state when the
sheng touchscreen reports zero pressure and touch-major later in the same
input frame.

The workflow deliberately builds `recoveryimage`, not `bootimage`.
On the target dual-boot tablet, `boot_a` is Android and `boot_b` is
Linux; neither boot partition is an acceptable recovery build target.

## Pinned inputs

- TWRP manifest commit:
  `6dc117d9cbd08430daa16db2013560e1c4017fa8`
- patched TWRP recovery core:
  `NoobMaster233/android_bootable_recovery@610555eb3535fa2d9df2e6091c0ee9f93dd169df`
- sheng device tree:
  `map220v/android_device_xiaomi_sheng@a9a2994997fa3574b6e718561b5d1c884661b89e`
- patched Boot Control:
  `NoobMaster233/android_hardware_qcom_bootctrl@a359fdddf152642e6d23834058859ed4ecaabcf9`

The local manifest replaces both the TWRP 12.1 recovery core and Boot
Control dependency with pinned patched commits.

## Output policy

The first build is uploaded only as a GitHub Actions artifact together
with its checksum, resolved manifest, AVB metadata, provenance and build
log. Before upload, the workflow checks the exact 100 MiB recovery
partition size, parses the AVB algorithm and recovery hash descriptor,
and verifies the signature against the pinned AOSP RSA-4096 test key.
Failure diagnostics deliberately exclude `recovery.img` and are named
`NOT-FOR-FLASHING`. A successful image is not automatically published as
a Release and must not be flashed before offline inspection and a
separately authorized, single-recovery-slot validation.
