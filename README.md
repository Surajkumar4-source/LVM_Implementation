# LVM_Implementation_Steps















Here’s the formatted version for clarity and structure:

---

# Detailed Guide for LVM with EXTENDED Operations  

Logical Volume Manager (LVM) allows flexible disk management in Linux. Below is a step-by-step guide covering key LVM operations.

---

## 1. Update and Install Required Tools  

### Update the Package List:  
```bash
sudo apt update  
```  
This updates the package list to get information about the latest versions of packages available in the repositories.  

### Install LVM Tools:  
```bash
sudo apt install lvm2  
```  
Installs the `lvm2` package, which provides tools to manage disk volumes.  

---

## 2. List Block Devices  

### Check Available Disks:  
```bash
lsblk  
```  
Lists block devices, showing all hard drives, partitions, and storage devices.  

---

## 3. Create Physical Volumes  

### Create a Physical Volume on `/dev/sdb`:  
```bash
sudo pvcreate /dev/sdb  
```  
Prepares `/dev/sdb` for inclusion in LVM by marking it as a physical volume (PV).  

### Display Physical Volumes:  
```bash
sudo pvdisplay  
```  
Shows detailed information about physical volumes on the system.  

### Create a Physical Volume on `/dev/sdc`:  
```bash
sudo pvcreate /dev/sdc  
```  
Marks `/dev/sdc` as a physical volume for use in LVM.  

---

## 4. Create a Volume Group  

### Create a Volume Group:  
```bash
sudo vgcreate suraj_volume_group /dev/sdb /dev/sdc  
```  
Combines `/dev/sdb` and `/dev/sdc` into a volume group (VG) named `suraj_volume_group`.  

### Display Volume Groups:  
```bash
sudo vgdisplay  
```  
Displays information about the volume group(s) on your system.  

---

## 5. Create a Logical Volume  

### Create a Logical Volume (LV):  
```bash
sudo lvcreate -L 10G -n suraj_logical_volume suraj_volume_group  
```  
Creates a logical volume named `suraj_logical_volume` with a size of 10GB within `suraj_volume_group`.  

### Display Logical Volumes:  
```bash
sudo lvdisplay  
```  
Shows details about the logical volume(s) in the volume group.  

---

## 6. Format and Mount the Logical Volume  

### Format the Logical Volume:  
```bash
sudo mkfs.ext4 /dev/suraj_volume_group/suraj_logical_volume  
```  
Formats the logical volume in the ext4 filesystem.  

### Check Filesystem Information:  
```bash
sudo blkid /dev/suraj_volume_group/suraj_logical_volume  
```  
Displays the UUID and filesystem type of the logical volume.  

### Create a Mount Point:  
```bash
sudo mkdir /mnt/suraj_lvm  
```  
Creates a directory to mount the logical volume.  

### Mount the Logical Volume:  
```bash
sudo mount /dev/suraj_volume_group/suraj_logical_volume /mnt/suraj_lvm  
```  
Mounts the logical volume at `/mnt/suraj_lvm`.  

### Verify Mount:  
```bash
df -h  
```  
Shows disk space usage and confirms the logical volume is mounted.  

Alternatively, you can use:  
```bash
mount | grep /mnt/suraj_lvm  
```  

---

## 7. Make the Mount Persistent  

### Edit the fstab File:  
```bash
sudo nano /etc/fstab  
```  
Add the following line to ensure the logical volume is automatically mounted after reboot:  
```
/dev/suraj_volume_group/suraj_logical_volume  /mnt/suraj_lvm  ext4  defaults  0  0  
```  

### Verify fstab Entry:  
```bash
cat /etc/fstab  
```  
Ensures the logical volume is listed in the `fstab` file.  

---

## 8. Extend the Logical Volume  

### Extend Logical Volume to 20GB:  
```bash
sudo lvextend -L 20G /dev/suraj_volume_group/suraj_logical_volume  
```  
Increases the size of `suraj_logical_volume` to 20GB.  

### Reduce Logical Volume to 15GB:  
```bash
sudo lvextend -L 15G /dev/suraj_volume_group/suraj_logical_volume  
```  
Adjusts the logical volume size to 15GB.  

### Resize the Filesystem:  
```bash
sudo resize2fs /dev/suraj_volume_group/suraj_logical_volume  
```  
Resizes the filesystem to match the new logical volume size.  

---

## 9. Verify Updates  

### Display Logical Volume Details:  
```bash
sudo lvdisplay /dev/suraj_volume_group/suraj_logical_volume  
```  
Shows updated information for the resized logical volume.  

### Check Disk Space:  
```bash
df -h  
```  
Confirms the updated disk space usage after resizing.  

---

This completes the detailed LVM setup and management process.
