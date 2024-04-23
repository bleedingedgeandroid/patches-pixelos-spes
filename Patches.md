# Patches for using this repo

## Initial Patches
```shell
# Add makefiles to bengal HAL
cp hardware/qcom-caf/common/os_pickup.mk hardware/qcom-caf/bengal/Android.mk
cp hardware/qcom-caf/common/os_pickup_qssi.bp hardware/qcom-caf/bengal/Android.bp 

cd hardware/qcom-caf/common
# Split bengal and kona SOC families
curl -s https://raw.githubusercontent.com/bleedingedgeandroid/patches-pixelos-spes/fourteen-pixelos/hardware/qcom-caf/common/0001-Split-bengal-and-kona-SoC-families.patch -s | git am
cd ../../..

cd hardware/qcom-caf/bengal/gps
# Split bengal and kona SOC families
curl -s https://raw.githubusercontent.com/bleedingedgeandroid/patches-pixelos-spes/fourteen-pixelos/hardware/qcom-caf/bengal/gps/0001-bengal-comment-out-unused-header.patch -s | git am
cd ../../../..

cd vendor/qcom/opensource/interfaces
# Introduce FM HAL
curl -s https://raw.githubusercontent.com/bleedingedgeandroid/patches-pixelos-spes/fourteen-pixelos/vendor/qcom/opensource/interfaces/0001-interfaces-Introduce-the-QTI-FM-HAL.patch | git am
cd ../../../..

cd external/wpa_supplicant_8
curl -s https://raw.githubusercontent.com/bleedingedgeandroid/patches-pixelos-spes/fourteen-pixelos/external/wpa_supplicant_8/0001-Convert-wpa_supplicant-to-soong-for-cuttlefish.patch | git am
cd ../..

cd vendor/qcom/common/system/gps
curl -s https://raw.githubusercontent.com/bleedingedgeandroid/patches-pixelos-spes/fourteen-pixelos/vendor/qcom/common/system/gps/0001-gps-drop-com.qualcomm.location.patch | git am
cd ../../../../..

rm -rf vendor/qcom/opensource/commonsys/fm device/qcom/vendor-common/memtrack/Android.bp vendor/qcom/opensource/core-utils/fwk-detect/Android.bp # We already have another thing providing these(fm from device/qcom/vendor-common/commonsys/fm)

cd vendor/aosp

# These patches are only needed when building unofficial. This requires you to add a valid pif fingerprint in overlay/rro_overlay/CertifiedPropsOverlay
git revert cba30d055a5dffdf57217c5f59ada565a78edd18 #this commit changes every time
cd ../..
```