# Extend ubuntu disk
source: https://forum.proxmox.com/threads/resize-ubuntu-vm-disk.117810/post-510089

For when you underestimated how much storage a VM needs.

* Increase disk size from pve gui
  * Make sure host is turned off
  * Host -> Hardware -> Disk Action
* Boot freshly extended host
* Check free space
  * ```sudo fdisk -l```
* Extend physical drive partition
  * ```sudo growpart /dev/sda 3```
* Show physical drive
  * ```sudo pvdisplay```
* Instruct LVM that disk size has changed
  * ```sudo pvresize /dev/sda3```
* Check if physical drive changed
  * ```sudo pvdisplay```
* Extend Logical Volume
* View starting LV
  * ```sudo lvdisplay```
* Resize LV
  * ```sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv```
* View changed LV
  * ```sudo lvdisplay```
* Resize Filesystem
  * ```sudo resize2fs /dev/ubuntu-vg/ubuntu-lv```
* Confirm resize
  * ```sudo fdisk -l```
