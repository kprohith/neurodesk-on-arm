# Setting up QEMU on ARM
## Create a 4 core 24 GB ARM Ampere A1 instance

sudo apt-get update
sudo apt install qemu-utils genisoimage qemu

wget https://cloud-images.ubuntu.com/xenial/current/xenial-server-cloudimg-amd64-disk1.img

qemu-img info xenial-server-cloudimg-amd64-disk1.img

sudo mv xenial-server-cloudimg-amd64-disk1.img ubuntu-16.04.qcow2

sudo qemu-img create -f qcow2 -F qcow2 -o backing_file=ubuntu-16.04.qcow2 instance-1.qcow2

sudo qemu-img info instance-1.qcow2
sudo qemu-img resize instance-1.qcow2 5G

ssh-keygen
export PUB_KEY=$(cat ~/.ssh/id_rsa.pub)

cat >meta-data <<EOF
local-hostname: instance-1
EOF

cat >user-data <<EOF
#cloud-config
users:
  - name: ubuntu
    ssh-authorized-keys:
      - $PUB_KEY
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    groups: sudo
    shell: /bin/bash
runcmd:
  - echo "AllowUsers ubuntu" >> /etc/ssh/sshd_config
  - restart ssh
EOF

sudo genisoimage  -output instance-1-cidata.iso -volid cidata -joliet -rock user-data meta-data

sudo qemu-system-x86_64  \
  -m 12G -smp $(nproc) -cpu EPYC \
  -nographic \
  -device virtio-net-pci,netdev=net0 \
  -netdev user,id=net0,hostfwd=tcp::2222-:22 \
  -drive if=virtio,format=qcow2,file=instance-1.qcow2 \
  -drive if=virtio,file=instance-1-cidata.iso

  ## Open another terminal
  ssh -o "StrictHostKeyChecking no" -p 2222 ubuntu@0.0.0.0


# For Ubuntu 22.04

wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img

sudo mv jammy-server-cloudimg-amd64.img ubuntu-22.04.qcow2
  
sudo qemu-img create -f qcow2 -F qcow2 -o backing_file=ubuntu-22.04.qcow2 instance-2.qcow2

sudo qemu-img info instance-2.qcow2
sudo qemu-img resize instance-2.qcow2 80G

ssh-keygen
export PUB_KEY=$(cat ~/.ssh/id_rsa.pub)

cat >meta-data-2 <<EOF
local-hostname: instance-2
EOF

cat >user-data <<EOF
#cloud-config
users:
  - name: ubuntu
    ssh-authorized-keys:
      - $PUB_KEY
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    groups: sudo
    shell: /bin/bash
runcmd:
  - echo "AllowUsers ubuntu" >> /etc/ssh/sshd_config
  - restart ssh
EOF

sudo genisoimage  -output instance-2-cidata.iso -volid cidata -joliet -rock user-data meta-data-2

sudo qemu-system-x86_64  \
  -m 12G \
  -nographic \
  -device virtio-net-pci,netdev=net0 \
  -netdev user,id=net0,hostfwd=tcp::2222-:22 \
  -drive if=virtio,format=qcow2,file=instance-2.qcow2 \
  -drive if=virtio,file=instance-2-cidata.iso

## Open another terminal
ssh -o "StrictHostKeyChecking no" -p 2222 ubuntu@0.0.0.0
