# How to apply:

```
git clone https://github.com/mendesrr/bc250-acpi-fix-updated-8c.git

cd bc250-acpi-fix-updated-8c

mkdir -p /tmp/acpi_tables/kernel/firmware/acpi

cp *.aml /tmp/acpi_tables/kernel/firmware/acpi/.

cd /tmp/acpi_tables

find kernel | cpio -H newc --create > SSDT_ACPI.cpio

sudo cp SSDT_ACPI.cpio /boot/.
```

## For Grub:
```
echo 'GRUB_EARLY_INITRD_LINUX_CUSTOM="../../SSDT_ACPI.cpio"' | sudo tee -a /etc/default/grub

ujust regenerate-grub

systemctl reboot
```
## For Limine:
```
sudo sed -i 's|KERNEL_CMDLINE\[default\]+="|KERNEL_CMDLINE[default]+="initrd=/SSDT_ACPI.cpio |' /etc/default/limine

sudo limine-mkinitcpio

systemctl reboot
```


### To check if it's working:
```
cpupower idle-info

cpupower frequency-info
```

### Additionally you can set the scaling governor to performance mode:
```
sudo cpupower frequency-set -g performance
```
Check current mode with:
```
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```
...................................................................................................................................................................................................
## Special thanks to:
NexGen-3D and shinf1x
