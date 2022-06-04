# Debian 11
```sh
curl -o preseed.cfg https://www.debian.org/releases/bullseye/example-preseed.txt
cat <<EOF >> preseed.cfg
d-i grub-installer/bootdev string default
d-i netcfg/get_hostname string debian11-1
d-i passwd/root-login boolean false
d-i passwd/user-fullname string whs
d-i passwd/user-password password magic
d-i passwd/user-password-again password magic
d-i passwd/username string whs
d-i preseed/late_command string apt-install openssh-server
popularity-contest popularity-contest/participate boolean false
tasksel tasksel/first multiselect standard
EOF
```

```sh
virt-install \
  --disk size=20 \
  --initrd-inject preseed.cfg \
  --location https://deb.debian.org/debian/dists/bullseye/main/installer-amd64/ \
  --memory 2048 \
  --name debian11-1 \
  --network=bridge:virbr0 \
  --os-variant debian11 \
  --vcpus 2
```

## Install pip
```sh
sudo apt-get install -y python3-distutils
curl -O https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py
```

```sh
echo 'export PATH=$PATH:$HOME/.local/bin' >> ~/.bashrc
source ~/.bashrc
```

## Install ansible
```sh
pip install --upgrade ansible
```

# Fedora 36
```sh
cat <<EOF >> anaconda-ks.cfg
autopart
clearpart --initlabel
keyboard --xlayouts=us
lang en_US.UTF-8
reboot
timezone --utc Asia/Hong_Kong
user --name whs --password magic --groups wheel
EOF
```

```sh
virt-install \
  --disk size=100 \
  --extra-args="inst.ks=file:/anaconda-ks.cfg" \
  --initrd-inject anaconda-ks.cfg \
  --location https://dl.fedoraproject.org/pub/fedora/linux/releases/36/Server/x86_64/os/ \
  --memory 2048 \
  --name fedora36 \
  --network=bridge:virbr0 \
  --os-variant fedora36 \
  --vcpus 2
```
