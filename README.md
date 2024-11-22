# Creating and Analyzing a Forensic Image - Using Linux
USE A WRITE BLOCKER WHEN POSSIBLE & FILL OUT CHAIN OF COSTODY
* Disable auto mounting of Drives (be sure to test non evidence drive) 
```
sudo systemctl stop udisks2.service
```
* Check udisks2 status
```
systemctl status udisks2 
```
* List block devices (drives):
```
lsblk
```
* Image evidence drive:
```
sudo dd if=/dev/sde of=/dev/sdc status=progress
```
-or-
```
sudo dc3dd if=/dev/sde of=/dev/sdc/img/disk.dd hash=md5 hash=sha256 hlog=/path/to/case/imghash.txt log=/path/to/case/img.log verb=on 
```
# Watch disk I/O
iostat -p ```<disk>``` -d ```<seconds>``` 
```
iostat -p sda -d 2
```

* Hash both image file and disk (if needed) 
```
md5sum /dev/sde && md5sum /dev/sdc/disk.dd
sha256sum /dev/sde && sha256sum /dev/sdc/disk.dd
```
# Analyze a copy of the image (.dd, .img, etc) 

 
# Mounting an image file 
```
sudo losetup -f -P ./img.dd
```
# Unmounting an image file 
* list loops:
```
losetup --list
```
* Unmount loop device: 
```
sudo losetup --detach /dev/loop2
```

# Plaso Log2Timeline
* install 
```
sudo apt update && sudo apt install plaso-tools
```
* (if that didn't work) 
```
sudo add-apt-repository universe
sudo add-apt-repository ppa:gift/stable  
```
# Psteal 
```
psteal.py --source image.raw -o dynamic -w registrar.csv
```

# Log2Timeline & PSort (alternative to using psteal) 
* log2timeline (unless specified with -z, default is in UTC ) 
```
log2timeline.py --storage-file timeline.plaso image.dd
```
* PSort
```
psort.py -o dynamic -w registrar.csv timeline.plaso 
```
* Advanced PSort
```
psort.py -o l2tcsv -w timeline.csv ./timeline.plaso "date > '2024-03-14 23:59:59' AND date < '2024-03-16 13:00:00'"
```
```
psort.py -o l2tcsv -w timeline.csv ./timeline.plaso "date > '2024-03-14 23:59:59'"
```
Then use Timeline Explorer to view large CSV - https://ericzimmerman.github.io/#!index.md

# TIPS
Disable Automounting Service from starting up with system 
```
sudo systemctl mask udisks2
```
Re-enable service at startup (reboot, or manually start service) 
```
sudo systemctl unmask udisks2
```
Bitlocker drives require you to enter the "-"s for the Recovery Password in Linux. 




