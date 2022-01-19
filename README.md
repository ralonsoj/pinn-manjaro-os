# pinn-manjaro-os

## Building the boot.tar.xz and root.tar.xz

$ sudo mkdir /run/media/rafa/boot   
$ sudo mkdir /run/media/rafa/root   
$ sudo mount /dev/sda1 /run/media/rafa/boot   
$ sudo mount /dev/sda2 /run/media/rafa/root 

$ cd /run/media/rafa/boot
$ sudo bsdtar --numeric-owner --format gnutar -cpvf /home/rafa/Desktop/boot.tar .

$ cd /run/media/rafa/root
$ sudo find . -type s -exec rm {} \;
$ sudo getfacl -s -R . >acl_permissions.pinn
$ sudo bsdtar --numeric-owner --format gnutar --one-file-system -cpf /home/rafa/Desktop/root.tar .

$ cd /home/rafa/Desktop/   
$ xz -9 -e boot.tar
$ xz -9 -e root.tar

## Creating boot.size and root.size

$ sudo du -BK -s /run/media/rafa/boot | cut -d"K" -f1 >boot.size  
$ sudo du -BK -s /run/media/rafa/root | cut -d"K" -f1 >root.size  


uncompressed_tarball_size boot partition    -> expr `cat boot.size` / 1024 + 1
partition_size_nominal boot partition       -> uncompressed_tarball_size + 100

uncompressed_tarball_size root partition    -> expr `cat root.size` / 1024 + 1
partition_size_nominal root partition       -> uncompressed_tarball_size + 500
