```sh
curl -o preseed.cfg https://www.debian.org/releases/bullseye/example-preseed.txt
cat <<EOF >> preseed.cfg
d-i grub-installer/bootdev  string default
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
  --disk size=100 \
  --initrd-inject preseed.cfg \
  --location http://ftp.debian.org/debian/dists/bullseye/main/installer-amd64 \
  --memory 2048 \
  --name debian11 \
  --network=bridge:virbr0 \
  --os-variant debian11 \
  --vcpus 2
```
