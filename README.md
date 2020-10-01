# XavierHacks

A collection of tricks and hacks for the AGX Xavier Development Kit

## Install NVMe SSD

1. Insert SSD into M.2 slot on the IO-board
   - https://www.jetsonhacks.com/2018/10/18/install-nvme-ssd-on-nvidia-jetson-agx-developer-kit/
2. Create SSD partition using parted and add a file system:

   1. Get device ID:
      ```
      sudo parted -l
      ```
   2. Open parted:
      ```
      sudo parted /dev/nvme0n1
      ```
      Check if correct device is active!
   3. Set partition table:
      ```
      mklabel gpt
      ```
   4. Create partition:
      ```
      mkpart primary 0% 100%
      ```
   5. Check if everything worked:
      ```
      print
      ```
   6. Exit parted:
      ```
      q
      ```
   7. Add file system:
      ```
      sudo mkfs.ext4 /dev/nvme0n1p1
      ```

3. Move /home folder to SSD:

   1. Mount SSD:
      ```
      sudo mkdir /mnt/home
      sudo mount /dev/nvme0n1p1 /mnt/home # use your device ID
      ```
   2. Copy /home folder and remove old one:
      ```
      sudo cp -a /home/* /mnt/home/
      sudo rm -rf /home/* (make sure it’s backed up at /mnt/home)
      ```
   3. Unmount SSD and edit /etc/fstab:

      ```
      sudo umount /dev/nvme0n1p1
      sudo nano -w /etc/fstab
      ```

      In /etc/fstab, add:

      ```
      /dev/nvme0n1p1 /home ext4 defaults 0 1
      ```

      For the looks, align the entries using spaces

   4. Mount devices:
      ```
      sudo mount -a
      ```
   5. Make sure /home looks fine and reboot the system.
