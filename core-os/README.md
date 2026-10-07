# Fedora CoreOS Butane configs

Butane configuration scripts for Fedora CoreOS

## homenucleus.bu

Config for CoreOS to host UniFi OS Server

* Do not forget to modify Butane script before install:
  * Replace xxx with username and password
  * Adjust `start_mib: ` for swap partition based on disk size
  * Check timezone

* Generate password for the user

```bash
podman run -ti --rm quay.io/coreos/mkpasswd --method=yescrypt
```

* Generate Ignition config

```bash
podman run --interactive --rm quay.io/coreos/butane:release --pretty --strict < homenucleus.bu > homenucleus.ign
```
* Host Ignition config

```bash
podman run -p 8080:80 -v .:/usr/share/nginx/html:ro --rm docker.io/library/nginx:stable-alpine
```
* Commads to test install on VirtualBox

```bash
VBoxManage controlvm "CoreOS" keyboardputstring sudo coreos-installer install /dev/sda --insecure-ignition --ignition-url http://192.168.x.x:8080/homenucleus.ign
VBoxManage controlvm "CoreOS" keyboardputstring curl -O https://fw-download.ubnt.com/data/unifi-os-server/5172-linux-x64-5.1.42-12e9e3cf-8f8b-4e54-928c-76b80a10c8a4.42-x64
```
