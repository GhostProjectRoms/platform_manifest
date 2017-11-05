PornAOSP
===========
Blazing fast, Rock-Hard stability. 

Getting Started
---------------
To get started with the Porn sources, you'll need to get
familiar with [Git and Repo](http://source.android.com/source/version-control.html).


Create the Directories
----------------------

You will need to set up some directories in your build environment.

To create them run:

    mkdir -p ~/bin
    mkdir -p ~/paosp


Install the Repository
----------------------

Enter the following to download the "repo" binary and make it executable:

curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo && chmod a+x ~/bin/repo

You may need to reboot for these changes to take effect. 
Now enter the following to initialize the repository:

    cd ~/paosp


Initializing the PornAOSP Source:
---------------

For initializing repo use:

    repo init -u https://github.com/PornAOSP/platform_manifest.git -b o

Syncing repo:

    repo sync -j2 | -j4 |-j8 | -j32 (# of CPUs x2)


Compiling PornAOSP
---------------

Set up environment:

    . build/envsetup.sh
    
If your device is officially supported by PornAOSP, you can do now:

    breakfast <device_codename>
    
...to automatically pull all missing repositories. Then:

    brunch <device_codename>
    
...to start compilation process.


Adding support for new device
================

If you want to be official maintainer and add PornAOSP support for a new device you have to create these two files in device tree:

paosp.mk sample
----------

    # Inherit from those products. Most specific first.
    $(call inherit-product, $(SRC_TARGET_DIR)/product/core_64_bit.mk) -- only for 64bit phones
    $(call inherit-product, $(SRC_TARGET_DIR)/product/full_base_telephony.mk)

    # Inherit from device
    $(call inherit-product, device/<path>/device.mk) -- path to main device makefile

    # Inherit common product files.
    $(call inherit-product, vendor/paosp/config/common_full_phone.mk)

    # Set those variables here to overwrite the inherited values.
    BOARD_VENDOR := 
    PRODUCT_BRAND := 
    PRODUCT_DEVICE := 
    PRODUCT_NAME := paosp_device
    PRODUCT_MANUFACTURER := 
    PRODUCT_MODEL := 
    TARGET_VENDOR := 

    # Use the latest approved GMS identifiers unless running a signed build
    ifneq ($(SIGN_BUILD),true)
    PRODUCT_BUILD_PROP_OVERRIDES += \
        BUILD_FINGERPRINT=specific for your device \
        PRIVATE_BUILD_DESC="specific for your device"
    endif

paosp.dependencies sample
----------

    [
      {
        "repository": "android_kernel_<name>",
        "target_path": "kernel/path"
      },
      {
        "repository": "proprietary_vendor_<name>",
        "target_path": "vendor/path"
      },
      {
        "repository": "android_device_<name>-common"
        "target_path": "device/path"
      }
    ]
