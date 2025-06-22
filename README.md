## Building and Testing Per-Interrupt Logger on Linux kernel 6.15.1


#### Clone/Download Linux kernel version 6.15.1 source code:
```
git clone git@github.com:awadyn/ixgbe.git
wget https://www.kernel.org/pub/linux/kernel/v6.x/linux-6.15.1.tar.gz
tar -xzf linux-6.15.1.tar.gz
```

#### Copy per-interrupt logger modifications to Linux 6.15.1:
```
cp -r ixgbe/ linux-6.15.1/drivers/net/ethernet/intel/
```

#### Configure Linux kernel 6.15.1:
```
sudo apt install -y fakeroot dwarves flex bison libssl-dev libelf-dev
```
If the above fails, then you probably need to update your package index and upgrade installed packages to current versions via `apt update` and `apt upgrade` commands.
```
cd linux-6.15.1/
cp -v /boot/config-$(uname -r) .config 
yes "" | make localmodconfig
scripts/config --disable SYSTEM_TRUSTED_KEYS
scripts/config --disable SYSTEM_REVOCATION_KEYS
scripts/config --set-str CONFIG_SYSTEM_TRUSTED_KEYS ""
scripts/config --set-str CONFIG_SYSTEM_REVOCATION_KEYS ""
```

#### Build and Install Linux kernel 6.15.1:
```
fakeroot make -j10
sudo make modules_install
sudo make install
sudo reboot
```
