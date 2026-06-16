# Installing Proxmox VE

## Introduction

A while ago I bought an old desktop PC to learn more about computer hardware, upgrades, networking, and homelab projects. After upgrading a few components, I decided to repurpose it as a home server. A colleague of mine (an IT manager) recommended using Proxmox for my home server setup.

Since I already had the hardware, building my own server made more sense.

This guide covers the installation of Proxmox VE.

---

## Download Proxmox

Download the latest Proxmox VE ISO:

https://www.proxmox.com/en/downloads

---

## Create a Bootable USB

Create a bootable USB drive using one of the following tools:

* Ventoy
* Rufus
* Balena Etcher

Once the USB drive is ready:

1. Insert it into the target machine.
2. Reboot the computer.
3. Open the BIOS / Boot Menu.
4. Select the USB drive as the boot device.

---

## Install Proxmox VE

When the installer starts, select:

```text
Install Proxmox VE (Graphical)
```

Select the drive where Proxmox will be installed.

> **Recommended:** Use an SSD if available.

> **Warning:** The installer will erase all data on the selected drive. Make sure you have backups before continuing.

Continue through the installer:

* Select Country
* Select Timezone
* Select Keyboard Layout

Configure credentials:

```text
Password
```

This is important because you'll use it regularly.

```text
Email
```

Less important for a homelab environment, but still recommended.

---

## Configure Networking

Select the network adapter connected to your network.

Hostname example:

```text
pve.local
```

I used `pve.local` because Proxmox prefers having a proper hostname (FQDN).

IP Address example:

```text
192.168.1.111
```

Review the settings and start the installation.

Once installation completes, reboot the machine.

---

## Accessing the Web Interface

After the reboot, Proxmox will display the management URL on the local console.

From another computer on the same network, open:

```text
https://<IP_ADDRESS>:8006
```

Example:

```text
https://192.168.1.111:8006
```

You'll likely receive a certificate warning. This is normal for a fresh installation.

Proceed to the login page and enter:

```text
Username: root
Password: <your password>
```

The username is always:

```text
root
```

After logging in, you should see the Proxmox dashboard and are ready to create your first virtual machine.

---

## Fixing the "No Valid Subscription" Repository Error

By default, Proxmox enables the Enterprise repository, which requires a paid subscription.

If you are running a homelab or personal server, disable the Enterprise repository and enable the free No-Subscription repository instead.



After logging into Proxmox:

1. Select your node (e.g. `pve`)
2. Open `Updates`
3. Open `Repositories`
4. Disable `Enterprise Repository`
5. Click `Add`
6. Select `No-Subscription Repository`
7. Click `Refresh`

You should now be able to install updates without a Proxmox subscription.

---

## Fixing the "No Valid Subscription" Repository Error CLI Alternative

```bash

/etc/apt/sources.list.d/pve-enterprise.list

```

Comment out:
```bash
deb https://enterprise.proxmox.com/debian/pve bookworm pve-enterprise
```
Add to:
```bash
/etc/apt/sources.list

deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
```
Then run:
```bash
apt update
apt full-upgrade -y
```
