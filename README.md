# How to apply for Bazzite and SteamOS:

```
git clone https://github.com/mendesrr/bc250-acpi-fix-updated-8c.git

cd bc250-acpi-fix-updated-8c

mkdir -p /tmp/acpi_tables/kernel/firmware/acpi

cp *.aml /tmp/acpi_tables/kernel/firmware/acpi/.

cd /tmp/acpi_tables

find kernel | cpio -H newc --create > SSDT_ACPI.cpio

sudo cp SSDT_ACPI.cpio /boot/.
```

## For Grub-Bazzite:
```
echo 'GRUB_EARLY_INITRD_LINUX_CUSTOM="../../SSDT_ACPI.cpio"' | sudo tee -a /etc/default/grub

ujust regenerate-grub

systemctl reboot
```
## For Grub-SteamOS:
```
echo 'GRUB_EARLY_INITRD_LINUX_CUSTOM="/SSDT_ACPI.cpio"' | sudo tee -a /etc/default/grub

sudo update-grub

systemctl reboot
```
## How to apply for CachyOS-Limine:
```
sudo -i

mkdir -p /etc/initcpio/acpi_override/

cd /etc/initcpio/acpi_override/

wget -nc https://github.com/mendesrr/bc250-acpi-fix-updated-8c/raw/refs/heads/main/SSDT-CST.aml https://github.com/mendesrr/bc250-acpi-fix-updated-8c/raw/refs/heads/main/SSDT-PST.aml

sed -i '/^HOOKS=/ { /acpi_override/q; s/microcode/& acpi_override/; q }' /etc/mkinitcpio.conf

mkinitcpio -P

systemctl reboot
```


### Check if it's working:
```
cpupower idle-info

cpupower frequency-info
```

Additionally you can set temporarily the scaling governor to performance mode:
```
sudo cpupower frequency-set -g performance
```
If you want to set it permanently:
```
sudo sed -i 's|#GOVERNOR=\'ondemand\'|GOVERNOR=\'performance\'|' /etc/default/cpupower-service.conf
```
Check current mode with:
```
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```
...................................................................................................................................................................................................
## Special thanks to:
NexGen-3D, shinf1x, J0sh and popsicle
