## TinyUSB Xinput driver

A xinput driver I use for a few of my personal projects.

Currently TinyUSB doesn't support using custom drivers so needs to be modified like this commit: https://github.com/Ryzee119/tinyusb/commit/4a61b7ac61c7cfefb80a3abfd0891adf105c545c

Then set `CFG_TUH_XINPUT` in your `tusb_config.h` to enable.
