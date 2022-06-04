## TinyUSB Xinput driver

A xinput driver I use for a few of my personal projects.

Currently TinyUSB doesn't support using custom drivers so needs to be modified like this commit: https://github.com/Ryzee119/tinyusb/commit/4a61b7ac61c7cfefb80a3abfd0891adf105c545c

Then set `CFG_TUH_XINPUT` in your `tusb_config.h` to enable.

Implement these functions in your application code. Example code provided:
```
void tuh_xinput_report_received_cb(uint8_t dev_addr, uint8_t instance, uint8_t const *report, uint16_t len)
{
    xinputh_interface_t *xid_itf = (xinputh_interface_t *)report;
    TU_LOG1("XINPUT Got data on dev %02x, len %02x:\n", dev_addr, len);
    TU_LOG1_MEM(report, len, 4);
    //Automatically received the next report
    tuh_xinput_receive_report(dev_addr, instance);
}

void tuh_xinput_mount_cb(uint8_t dev_addr, uint8_t instance, const xinputh_interface_t *xinput_itf)
{
    TU_LOG1("XINPUT MOUNTED %02x %d\n", dev_addr, instance);
    // If this is a Xbox 360 Wireless controller, dont register yet. Need to wait for a connection packet
    // on the in pipe.
    if (xinput_itf->type == XBOX360_WIRELESS && xinput_itf->connected == false)
    {
        tuh_xinput_receive_report(dev_addr, instance);
        return;
    }
    tuh_xinput_set_led(dev_addr, instance, 0, true);
    tuh_xinput_set_led(dev_addr, instance, 1, true);
    tuh_xinput_set_rumble(dev_addr, instance, 0, 0, true);
    tuh_xinput_receive_report(dev_addr, instance);
}

void tuh_xinput_umount_cb(uint8_t dev_addr, uint8_t instance)
{
    TU_LOG1("XINPUT UNMOUNTED %02x %d\n", dev_addr, instance);
}
```
