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
-or- 
sudo dc3dd if=/dev/sde of=/dev/sdc/img/disk.dd hash=md5 hash=sha256 hlog=/path/to/case/imghash.txt log=/path/to/case/img.log progress=on verb=on 
```
* Hash both image file and disk (if needed) 
```
md5sum /dev/sde && md5sum /dev/sdc/disk.dd | tee hash.txt
sha256sum /dev/sde && md5sum /dev/sdc/disk.dd | tee hash.txt
```
Analyze a copy of the image (.dd, .img, etc) 
ie. a copy of the copy. 



# TIPS
Disable Automounting Service from starting up with system 
```
sudo systemctl mask udisks2
```
Re-enble service at startup (reboot, or manually start service) 
```
sudo systemctl unmask udisks2
```
