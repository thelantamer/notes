# GNU Linux Cheat Sheet

## special permissions for ssh keys
chmod 600 keyfile.pub

## chown for user and group
sudo chown myuseracct:myuseracct filename

## preferred bash prompt
export PS1="\[\033[0;32m\]\u@\h:\[\033[36m\]\W\[\033[0m\] \$ "

## linux distro info
cat /etc/*release

## copy ssh keys for auto login
ssh destuser@desthost mkdir -p .ssh
cat ~/.ssh/id_rsa.pub | ssh destuser@desthost 'cat >> .ssh/authorized_keys'

## scp basics
scp /path/to/source username@host:/path/to/destination

REMOTE DESKTOP (Windows host)
rdesktop -u username -d domainname -g 1152x1008 -p yourpassword -a 16 -r disk:fedora=/home/username/ -r clipboard:CLIPBOARD -z -x b -k en-us hostname &
specify screen size, share local folder and clipoboard, run as background task, no window decorations

CP - RECURSIVE, DO NOT OVERWRITE, VERBOSE
cp source destination -ruv

RSYNC
rsync -av curfolder destfolder	sort by size 

LS SHORTCUTS
ls -alSh sort by size 
ls -d */ display folders

MAKE .ISO IMAGE
sudo genisoimage -o image.iso /media/CDTitle

FIND DUPLICATE FILES
fdupes . -r -n -f -S -d -N &gt; /data/fdupes.txt
recurse, noempty, omitfirst, show size, delete, noprompt

FORMAT NTFS PARTITION
sudo fdisk /dev/sdb
	[t, 87]
sudo mkntfs /dev/sdb1 -f

CHECK DISK SPACE
du -h  	disk usage from current folder, human-readable format
du -hc -d 1		disk size, show largest folder
df -h  	disk usage on all drives, human-readable

BACKGROUND TERMINALS, DISCONNECT AND RECONNECT
screen -d -R name
screen -r name
[CTRL+a, d to detach]

screen -X -S [## to kill] quit	kill the screen session

TAR (BACKUP WITH GZIP COMPRESSION)
tar czfv source dest.tgz

SECURE COPY (COPY OVER SSH)
scp -r user@remotehost:files localdestination
scp localfiles user@remotehost:remotedestination

SHUTDOWN IN 5 MINS
sudo shutdown +5

DISK IMAGE (BLOCK)
sudo dd if=/dev/sda1 of=/media/rn1ds4/2012-image.dd conv=sync,noerror

DISK IMAGE (BLOCK - WITH PROGRESS BAR)
sudo dd if=/dev/sdc bs=4096 | pv -s 2G | sudo dd bs=4096 of=~/USB_BLACK_BACKUP.IMG

ZERO OUT DRIVE
sudo dd if=/dev/zero of=/dev/sdX bs=1M

LOGON TO WINDOWS DOMAIN
smbclient //hostname/sharename –U username –W domainname
recurse [enables automatic recursion]
prompt [disables automatic prompting]

MOUNT WINDOWS SHARE
sudo mount.cifs //resource/sharename -o user=domainname/username /mnt/localmountpoint

SERIAL PORT TERMINAL
sudo minicom -D /dev/ttyS0
[in screen, use CTRL+A,A to send CTRL+A]

MOUNT ISO IMAGE
sudo mount -o loop -t iso9660 /tmp/Fedora-7.iso /mnt/iso

FIND AND COPY
find . -iname "*.mp3" -exec cp {} /data/mp3/ \;

FIND AND RENAME FILE TYPES
for i in *.txt; do mv $i $RANDOM.txt; done

MOUNT DISK IMAGE
parted hda.img
mount -o loop,ro,offset=  hda.img /mnt/sda1

FILE AND PHOTO RECOVERY
sudo photorec /log
sudo testdisk /log

FILE HASHES
sha1sum filename &gt; filename.txt.sha1
md5sum filename &gt; md5sum.txt.md5

FILE ENCRYPTION
gpg -c filename

MEDIA PLAYER FROM BASH, REPEAT 10 TIMES
mplayer *.mp3 -loop 10

LOGICAL VOLUME RECOVERY (FEDORA)
sudo su
lvm
vgscan
vgchange -ay VolGroup00
lvs
mkdir /mnt/fcroot
mount /dev/VolGroup00/LogVol00 /mnt/fcroot -o ro,user

INSTALL NESSUS (VULNERABILITY SCANNING)
[first obtain new registration code from website]
sudo /opt/nessus/bin/nessus-fetch --register CODE-CODE
sudo /sbin/service nessusd stop
sudo /sbin/service nessusd start
https://localhost:8834

SECURE SHELL OUTSIDE PUTTY, WITH LOGGING
ssh user@remote.server.com | tee /path/of/log/file

SPLIT LARGE FILE
split -b 10m currentfile.img newfile.img_

COMBINE FILES
cat newfile.img_1 newfile.img_2 &gt; newfile.img

SHUTDOWN FIREWALL (IPTABLES)
sudo iptables -P INPUT ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -F
sudo iptables -X

SOUND CARD CONTROLS
alsamixer

REMOTE ACCESS VIA SSH
Xnest -geometry 800x600 :2 &
export DISPLAY=:2
ssh -X 192.168.1.20
gnome-session

COMBINE PDF FILES
gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -sOutputFile=finished.pdf file1.pdf file2.pdf

SED - SEARCH AND REPLACE
cat phones.csv | sed -e 's/334-222-2222/3342222222/' -e 's/334-222-2222/3342222222/' &gt; phones1.csv

MD5 HASH FROM TEXT
echo -n “texthere” | md5

LIST GROUPS
cat /etc/group

ADD EXISTING USER TO EXISTING GROUP
sudo usermod -G vboxusers -a qdd

MAKE A CIFS MOUNT PERMANENT
sudo cp /etc/fstab /etc/fstab-backup

gedit ~/.smbcredentials
username=msusername
password=mspassword

chmod 600 ~/.smbcredentials

fstab:
//servername/sharename /media/windowsshare cifs credentials=/home/ubuntuusername/.smbcredentials,iocharset=utf8,file_mode=0777,dir_mode=0777 0 0

sudo mount -a

GREP-FIND TEXT AND OUTPUT TO SEPARATE FILE
http://bit.ly/VnLrTR

SPANISH-LANGUAGE CHARACTERS
setup Compose Key (Right-ALT key) in UBUNTU, (see link below)
http://bit.ly/WFUNd8

[compose key + ‘ + e]	é
[compose key + : + u]	ü
[compose key,~,n]		ñ


## ssh agent
eval "$(ssh-agent -s)" && ssh-add -k ~/.ssh/id_rsa

## sync folders using rsync and ssh
rsync -avh -e ssh . user1@host1.mydomain.com:/home/user1/ansibletmp

## remove all files from current folder, including hidden (like .git)
find ! -name '.' ! -name '..' -delete