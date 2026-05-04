# Autoinstall Ubuntu Server

## Summary

- Automatically picks the biggest disk to install to
- Creates a user for me and adds gh ssh keys to log in
- Updates and upgrades apt packages


## Upstream Documentation

- [Canonical Subiquity Autoinstall](https://canonical-subiquity.readthedocs-hosted.com/en/latest/intro-to-autoinstall.html)
- [Cloud-Init Documentation](https://cloudinit.readthedocs.io/en/latest/index.html)
- [Nocloud Datasource](https://cloudinit.readthedocs.io/en/latest/reference/datasources/nocloud.html)


## Usage

### Provide Files

Host this folder in a webserver in order to serve the files **user-data**, **meta-data**, and **vendor-data**.
```
cd <this_folder>
python3 -m http.server 3003
```

At the time of writing, I'm hosting a public instance at '[https://syndr.dev/cloud-init/ubuntu-server/](https://syndr.dev/cloud-init/ubuntu-server/)'

### Boot Ubuntu Live Server

Obtain one of Canonical's [Ubuntu Server images](https://ubuntu.com/download/server), attach this disk as well
as the disk to install to to your machine, and boot to the install disk.

### Setup Webserver Discovery

Add this line to the kernel command line or smbios:
```
ds=nocloud;s=http(s)://<address_of_webserver>/
```

_Note: if using GRUB, escape the semicolon:_
```
ds=nocloud\;s=http(s)://<address_of_webserver>/
```

Yours may look like:
```
ds=nocloud\;s=http://192.168.96.132:3003/
```

If a zero-touch installation is desired, an additional parameter `autoinstall` can be specified.
Otherwise, you will be prompted before installation commences.

_(See the Arch Wiki [Kernel Parameters](https://wiki.archlinux.org/title/Kernel_parameters) Page)_

### Finalization

If all goes well, the install should commence without the installer gui appearing. You will
be able to log in to the user specified in the user-data file once cloud-init is finished.
