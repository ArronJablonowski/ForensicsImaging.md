# Taking a Forensic Image 

* Disable Automounting of Drives (be sure to test non evidence drive) 
```
sudo systemctl stop udisks2.service
```
* Check udisks2 status
```
systemctl status udisks2 
```
* List block devices:
```
lsblk
```
* Image evidence drive:
```
sudo dd if=/dev/sde of=/dev/sdc status=progress
```
-or-
```
sudo dc3dd if=/dev/sde of=/dev/sdc/img/disk.dd hash=md5 hash=sha256 hlog=/path/to/case/imghash.txt log=/path/to/case/img.log progress=on verb=on 
```
* Hash both image file and disk (if needed) 
```
md5sum /dev/sde && md5sum /dev/sdc/disk.dd | tee hash.txt
sha256sum /dev/sde && md5sum /dev/sdc/disk.dd | tee hash.txt
```
Analyze a copy of the image (.dd, .img, etc) 
ie. a copy of the copy. 

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

# TIPS
Disable Automounting Service from starting up with system 
```
sudo systemctl mask udisks2
```
Re-enble service at startup (reboot, or manually start service) 
```
sudo systemctl unmask udisks2
```
Bitlocker drives require you to enter the "-"s for the Recovery Password. 

# Plaso Log2Timeline
* install 
```
sudo apt update && sudo apt isntall plaso-tools
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

# Log2Timeline & PSort
* log2timeline
```
log2timeline.py --storage-file timeline.plaso image.dd
```
* PSort
```
psort.py -o dynamic -w registrar.csv timeline.plaso 
```
* Advances PSort
```
psort.py -z UTC -o l2tcsv -w timeline.csv ./timeline.plaso "date > '2024-03-14 23:59:59' AND  '2024-03-16 13:00:00'"
```
```
psort.py -z UTC -o l2tcsv -w timeline.csv ./timeline.plaso "date > '2024-03-14 23:59:59'"
```
Then use Timeline Explorer to view large CSV - https://ericzimmerman.github.io/#!index.md
