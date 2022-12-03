
# Proxmox Errors
These are the error that I have found within Proxmox.

## VM Errors 

### Windows ISO Fail to boot

!!! warning "Windows ISO Fail to boot"

    This error seems to have something to do with, not having QEMU Guest Agent installed/enabled from the Proxmox VM wizard.

!!! success "*Resolved by*"

    Install/enable QEMU Guest Agent in Proxmox VM wizard before booting the VM for the first time

----------------------------
### Windows 10 Template Blue Screen
!!! warning "Windows 10 Template Blue Screen"

    This error has been “created” by making a VM, that runs an completely fresh installation of windows 10, without any updates or software installed. 
    
    Then powering the VM down and making it to a Proxmox template. When it has been made into a template then remove the CD/DVD drive. 

!!! success "*Resolved by*"

    If you don’t care about the CD/DVD drive, just leave it be. The missing drive is what is causing Windows to blue screen. If you have already remove the drive, just re-added it again but make sure it is the right ISO you pick.

----------------------------
### Proxmox Virtual Environment 7.2-11 Live Migration
!!! warning "Proxmox Virtual Environment 7.2-11 Live Migration"

    This problem have been covered by Proxmox's own forum.

    Xeon(R) Bronze 3106 <<--->> Xeon(R) CPU E5-2650 v2 kernel 5.15

    ----------------------------
    When migrating Xeon(R) CPU E5-2650 v2 -->> Xeon(R) Bronze 3106 

    --> *Everything is ok, VM is working*

    ----------------------------
    When migrating Xeon(R) Bronze 3106 -->> Xeon(R) CPU E5-2650 v2 

    --> *VM freezes, requires full VM shutdown and power on*

!!! success "*Resolved by*"
    
    Installing pve-kernel-5.19 repository
    ```
    apt install pve-kernel-5.19
    ```

    After restarting both PVE servers they are running on pve-kernel-5.19.7-1-pve and now

    -----------------------------
    When migrating Xeon(R) CPU E5-2650 v2 -->> Xeon(R) Bronze 3106 

    --> *Everything is ok, VM works and does not freeze*

    -----------------------------
    When migrating Xeon(R) Bronze 3106 -->> Xeon(R) CPU E5-2650 v2 

    --> *Everything is ok, VM is running and not freezing*

    -------------

## VM Internet Access

### Tailscale VPN On Proxmox LXC Container
!!! warning "Tailscale VPN On a Proxmox LXC Container"

    This issue has been found when trying to use tailscale on a Proxmox LXC Container. 

    Tailscale can be installed without any problems, but when you try to run the `Sudo tailscale up` command it will return and error saying something along the lines of tailscale is not running try to.... and no matter what you try on the VM it will not be able to start tailscale.

!!! success "*Resolved by*"
    
    This solution will fix any VPN you want to run such as Tailscale and OpenVPN
    ```
    cd /etc/pve/lxc
    ```

    Find the vmid of the LXC Container and then copy the lines below into the LXC Containers config.
    (Just put it below the lines already there)

    ```
    lxc.cgroup.devices.allow: c 10:200 rwm 
    lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
    ```

    **AND DON'T FORGET TO REBOOT THE LXC CONTAINER AFTER...**

---------------------------

