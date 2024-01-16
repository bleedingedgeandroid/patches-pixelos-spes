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
git clone https://gitlab.pixelexperience.org/android/vendor-blobs/vendor_xiaomi_spes.git vendor/xiaomi/spes  -b fourteen
git clone https://gitlab.pixelexperience.org/android/vendor-blobs/vendor_xiaomi_sm6225-common.git vendor/xiaomi/sm6225-common  -b fourteen
git clone https://github.com/PixelOS-AOSP/hardware_xiaomi hardware/xiaomi -b fourteen
git clone https://github.com/TheMatheusDev/external_wpa_supplicant_8 external/wpa_supplicant_8 -b uvite
git clone https://github.com/PixelExperience-Staging/hardware_st_nfc hardware/st/nfc -b fourteen
git clone https://github.com/PixelExperience-Staging/vendor_qcom_opensource_usb vendor/qcom/opensource/usb -b fourteen
git clone https://github.com/bleedingedgeandroid/device_xiaomi_sepolicy device/xiaomi/sepolicy -b  fourteen-pixelos

```
## Initial Patches (replace the patch files with the ones in this repo)
```shell
# Add makefiles to bengal HAL
cp hardware/qcom-caf/common/os_pickup.mk hardware/qcom-caf/bengal/Android.mk
cp hardware/qcom-caf/common/os_pickup_qssi.bp hardware/qcom-caf/bengal/Android.bp

cd hardware/qcom-caf/common
# Split bengal and kona SOC families
git apply ../../../device/xiaomi/spes/patches/0001-split-kona-and-bengal-family.patch

# Use specific FM
git apply ../../../device/xiaomi/spes/patches/0001-Use-specific-FM-HAL.patch
cd ../../..

cd vendor/qcom/opensource/interfaces
# Introduce FM HAL
git apply ../../../../device/xiaomi/spes/patches/0a1e8499b11c9c80a58510faa7f63e2d85ab831d.patch
cd ../../../..

rm -rf device/qcom/vendor-common/commonsys/fm device/qcom/vendor-common/memtrack/Android.bp # We already have another thing providing these

# These patches are only needed when building unofficial. This requires you to add a valid pif fingerprint in overlay/rro_overlay/CertifiedPropsOverlay
cd vendor/aosp
git revert 716e7e378e5f31f634893865c10bdaa17092883e
git revert b4a22409743f39f5bd381f62484ea7e5498f0bee
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