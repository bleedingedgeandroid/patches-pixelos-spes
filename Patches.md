# Patches for using this repo

## Initial clone:
``` shell
# We replace these repos to get working nfc, wifi and mtp
rm -rf hardware/st/nfc external/wpa_supplicant_8 vendor/qcom/opensource/usb

git clone https://github.com/PixelExperience-Devices/device_qcom_common.git device/qcom/common -b fourteen
git clone https://github.com/PixelExperience-Devices/device_qcom_common-sepolicy.git device/qcom/common-sepolicy -b fourteen
git clone https://github.com/PixelExperience-Devices/device_qcom_qssi.git device/qcom/qssi -b fourteen
git clone https://github.com/PixelExperience-Devices/device_qcom_vendor-common.git device/qcom/vendor-common -b fourteen
git clone https://github.com/PixelExperience-Devices/device_qcom_wlan.git device/qcom/wlan -b fourteen
git clone https://gitlab.pixelexperience.org/android/vendor-blobs/vendor_qcom_common.git vendor/qcom/common -b fourteen
git clone https://github.com/PixelExperience-Staging/vendor_qcom_opensource_commonsys-intf_bluetooth.git vendor/qcom/opensource/commonsys-intf/bluetooth -b fourteen
git clone https://github.com/PixelExperience-Staging/vendor_qcom_opensource_core-utils.git vendor/qcom/opensource/core-utils -b fourteen
git clone https://github.com/PixelExperience-Staging/vendor_qcom_opensource_commonsys_dpm.git vendor/qcom/opensource/commonsys/dpm -b fourteen
git clone https://github.com/PixelExperience-Staging/hardware_qcom-caf_bengal_audio.git hardware/qcom-caf/bengal/audio -b fourteen
git clone https://github.com/PixelExperience-Staging/hardware_qcom-caf_bengal_display.git hardware/qcom-caf/bengal/display -b fourteen
git clone https://github.com/PixelExperience-Staging/hardware_qcom-caf_bengal_gps.git hardware/qcom-caf/bengal/gps -b fourteen
git clone https://github.com/PixelExperience-Staging/hardware_qcom-caf_bengal_media.git hardware/qcom-caf/bengal/media -b fourteen
git clone https://github.com/bleedingedgeandroid/device_xiaomi_sm6225-common.git device/xiaomi/sm6225-common -b fourteen-pixelos
git clone https://github.com/muralivijay/kernel_xiaomi_sm6225.git kernel/xiaomi/sm6225  -b android-14
git clone https://github.com/bleedingedgeandroid/device_xiaomi_spes-chris.git device/xiaomi/spes -b fourteen-pixelos
git clone https://github.com/bleedingedgeandroid/vendor_xiaomi_spes.git vendor/xiaomi/spes  -b fourteen-pixelos
git clone https://gitlab.pixelexperience.org/android/vendor-blobs/vendor_xiaomi_sm6225-common.git vendor/xiaomi/sm6225-common  -b fourteen
git clone https://github.com/PixelOS-AOSP/hardware_xiaomi hardware/xiaomi -b fourteen
git clone https://github.com/TheMatheusDev/external_wpa_supplicant_8 external/wpa_supplicant_8 -b uvite
git clone https://github.com/PixelExperience-Staging/hardware_st_nfc hardware/st/nfc -b fourteen
git clone https://github.com/PixelExperience-Staging/vendor_qcom_opensource_usb vendor/qcom/opensource/usb -b fourteen
git clone https://github.com/bleedingedgeandroid/device_xiaomi_sepolicy device/xiaomi/sepolicy -b  fourteen-pixelos

```
## Initial Patches
```shell
# Add makefiles to bengal HAL
cp hardware/qcom-caf/common/os_pickup.mk hardware/qcom-caf/bengal/Android.mk
cp hardware/qcom-caf/common/os_pickup_qssi.bp hardware/qcom-caf/bengal/Android.bp

cd hardware/qcom-caf/common
# Split bengal and kona SOC families
curl -s https://raw.githubusercontent.com/bleedingedgeandroid/patches-pixelos-spes/fourteen-pixelos/hardware/qcom-caf/common/0001-Split-bengal-and-kona-SoC-families.patch -s | git am
cd ../../..

cd vendor/qcom/opensource/interfaces
# Introduce FM HAL
curl -s https://github.com/PixelExperience/vendor_qcom_opensource_interfaces/commit/0a1e8499b11c9c80a58510faa7f63e2d85ab831d.patch | git am
cd ../../../..

rm -rf vendor/qcom/opensource/commonsys/fm device/qcom/vendor-common/memtrack/Android.bp # We already have another thing providing these(fm from device/qcom/vendor-common/commonsys/fm)

# These patches are only needed when building unofficial. This requires you to add a valid pif fingerprint in overlay/rro_overlay/CertifiedPropsOverlay
cd vendor/aosp
git revert aa62a23b3d0dc06deb8ea332d0f3107e9261e970
git revert cba30d055a5dffdf57217c5f59ada565a78edd18
cd ../..
```



## Update all repos 
```shell
cd device/qcom/common && git pull && cd ../../..
cd device/qcom/common-sepolicy && git pull && cd ../../..
cd device/qcom/qssi && git pull && cd ../../..
cd device/qcom/vendor-common && git pull && cd ../../..
cd device/qcom/wlan && git pull && cd ../../..
cd vendor/qcom/common && git pull && cd ../../..
cd vendor/qcom/opensource/commonsys-intf/bluetooth && git pull && cd ../../../../..
cd vendor/qcom/opensource/core-utils && git pull && cd ../../../..
cd vendor/qcom/opensource/commonsys/dpm && git pull && cd ../../../../..
cd hardware/qcom-caf/bengal/audio && git pull && cd ../../../..
cd hardware/qcom-caf/bengal/display && git pull && cd ../../../..
cd hardware/qcom-caf/bengal/gps && git pull && cd ../../../..
cd hardware/qcom-caf/bengal/media && git pull && cd ../../../..
cd device/xiaomi/sm6225-common && git pull && cd ../../..
cd kernel/xiaomi/sm6225  && git pull && cd ../../..
cd vendor/xiaomi/spes  && git pull && cd ../../..
cd vendor/xiaomi/sm6225-common  && git pull && cd ../../..
cd hardware/xiaomi && git pull && cd ../..
cd hardware/st/nfc && git pull && cd ../../../
cd external/wpa_supplicant_8 && git pull && cd ../..
cd vendor/qcom/opensource/usb && git pull && cd ../../../..
cd device/xiaomi/sepolicy && git pull && cd ../../..
```
