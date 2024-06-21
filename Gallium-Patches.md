# Patches for using this repo

## Initial Patches
```shell
cd vendor/aosp

# These patches are only needed when building unofficial. This requires you to add a valid pif fingerprint in overlay/rro_overlay/CertifiedPropsOverlay
curl -s https://raw.githubusercontent.com/bleedingedgeandroid/patches-pixelos-spes/fourteen-pixelos/gallium-patches/vendor/aosp/0001-spes-re-add-CertifiedPropsOverlay.patch | git am
cd ../..
```