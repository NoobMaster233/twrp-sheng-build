# Reproducible TWRP recovery build for Xiaomi Pad 6S Pro (sheng)

This repository builds a source-level replacement for the Xiaomi Pad 6S
Pro TWRP recovery whose Boot Control HAL reports three slots and fails on
the cross-UFS-LUN `multiimgoem` / `multiimgqti` layout.

The workflow deliberately builds `recoveryimage`, not `bootimage`.
On the target dual-boot tablet, `boot_a` is Android and `boot_b` is
Linux; neither boot partition is an acceptable recovery build target.

## Pinned inputs

- TWRP manifest commit:
  `6dc117d9cbd08430daa16db2013560e1c4017fa8`
- sheng device tree:
  `map220v/android_device_xiaomi_sheng@a9a2994997fa3574b6e718561b5d1c884661b89e`
- patched Boot Control:
  `NoobMaster233/android_hardware_qcom_bootctrl@a359fdddf152642e6d23834058859ed4ecaabcf9`

The local manifest replaces the TWRP 12.1 Boot Control dependency at
`hardware/qcom-caf/bootctrl` with the pinned patched commit.

## Output policy

The first build is uploaded only as a GitHub Actions artifact together
with its checksum, resolved manifest, AVB metadata, provenance and build
log. It is not automatically published as a Release and must not be
flashed before offline inspection and a separately authorized,
single-recovery-slot validation.
