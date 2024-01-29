<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [Introduction](#introduction)
   * [System magic](#system-magic)
      + [Passwd](#passwd)
      + [Df](#df)
      + [Free](#free)
      + [Find](#find)
      + [Du](#du)
      + [Ps](#ps)
      + [Apt-mark](#apt-mark)
      + [Echo](#echo)
      + [Systemctl](#systemctl)
   * [User management magic](#user-management-magic)
      + [Useradd](#useradd)
      + [Setfacl](#setfacl)
   * [LVM and disk management magic](#lvm-and-disk-management-magic)
      + [Fdisk](#fdisk)
      + [Pvcreate](#pvcreate)
      + [Vgcreate](#vgcreate)
      + [Lvcreate](#lvcreate)
      + [Mkfs](#mkfs)
      + [Mount](#mount)
      + [Fstab](#fstab)
      + [Umount](#umount)
   * [Network magic](#network-magic)
      + [Ip](#ip)
      + [Curl](#curl)
   * [Text and data manipulation magic](#text-and-data-manipulation-magic)
      + [Sed](#sed)
      + [Awk](#awk)
      + [Grep](#grep)
   * [Multimedia magic](#multimedia-magic)
      + [FFmpeg](#ffmpeg)
      + [Yt-dlp](#yt-dlp)
   * [Security magic](#security-magic)
      + [Nmap](#nmap)
      + [Gobuster](#gobuster)
      + [Enum4linux ](#enum4linux)
      + [Smbclient](#smbclient)
      + [Hydra](#hydra)

<!-- TOC end -->

# Introduction
Let's face it, we are mere mortals that keep forgetting everytime we learn something new. So that's why I created this simple cheatsheet. Every time I forget something important, I want to refer back in one single place instead of scouring the Internet for the same answer that I did before. Hope this will help.

## System magic
### Passwd
```sh
paste <(cat /etc/passwd | cut -d: -f1) <(cat /etc/passwd | cut -d: -f3) <(cat /etc/passwd | cut -d: -f4) <(cat /etc/passwd | cut -d: -f5) | column -s $'\t' -t
```
- Show only the username, user id, group id and comment/description from `passwd` file.

### Df
```sh
df -h | awk '/^C:\\/ {print $3 "/" $2}'
```
-  Show C: disk usage, use in WSL.

```sh
df -h -t ext4 -t xfs | awk '{print $1, $3 "/" $2}' | column -t
```
- Show disk usage for disk using ext4/xfs filesystems.

### Free
```sh
free -h | awk '/^Mem:/ {print $3 "/" $2}'
```
- Show memory used/total.

### Find
```sh
find . -printf '%s %p\n'| sort -nr | head -10
```
- Show top 10 largest files (descending order, starting from the current folder, recursively).

### Du
```sh
du -hsx * | sort -rh | head -10
```
- Show top 10 largest folders.

### Ps
```sh
ps axch -o cmd:15,%mem --sort=-%mem
```
- Show most memory intensive process.

```sh
ps axch -o cmd:15,%cpu --sort=-%cpu
```
- Show most CPU intensive process.

### Apt-mark
```sh
apt-mark showmanual
```
- Show explicit installed packages.

### Echo
```sh
echo $PATH | tr : \\n
```
- Show current PATH environment variable.

### Systemctl
```sh
systemctl list-unit-files --state=enabled --no-pager
```
- Show all installed services.

## User management magic
### Useradd
```sh
sudo useradd -d /home/miku -s /bin/bash -m miku -c "Miku Kuwajima"
```
- Add a new user, specifices home directory, default shell, creates home directory for new user if it does not exist and adds a comment/description.

### Setfacl
```sh
sudo setfacl -R -m u:username:rX /opt/admin/tomcat/logs
```
- Grant specific user read only access to the directory belongs to other user, for read and list out all the content in the directory without changing owner and ownership of the directory.

```sh
sudo setfacl -R -x u:username /opt/admin/tomcat/logs
```
- Remove read only access given previously.

## LVM and disk management magic
### Fdisk
```sh
fdisk -l
```
- List partitions

```sh
fdisk /dev/sdX
```
- Start the partition process:
    - `m` for help
    - `n` create a new partition
    - `t` change partition type
    - `w` write changes to the disk
    - `q` to quit
    - `i` print information about a partition

### Pvcreate
```sh
pvcreate /dev/sdX?
```
- Create physical volume. Check using `pvs` command.

### Vgcreate
```sh
vgcreate vg-anyname /dev/sdX?
```
- Create volume group. Check using `vgs` command.

### Lvcreate
```sh
lvcreate -L sizeG -n lv-anyname vg-anyname
```
- Create logical volume with the specified size. Check using `lvs` command.

```sh
lvcreate -l 100%FREE -n lv-anyname vg-anyname
```
- Create logical volume by using remaining free space from volume group, in this case all available space (100%).
- This can also fix error `volume group vg-anyname has insufficient free space (14799 extents): 14800 required when creating logical volume`.

### Mkfs
```sh
mkfs.xfs /dev/vg-anyname/lv-anyname
```
- Create `xfs` file system on the LVM disk

### Mount
```sh
cd / ; mkdir -p newdir ; mount /dev/vg-anyname/lv-anyname newdir/
```
- Change to root directory, create new directory named `newdir` and mount newly formatted disk on `newdir` directory.

### Fstab
```sh
/dev/mapper/vg-anyname/lv-anyname   /newdir     xfs     defaults    0 0
```
- Edit `fstab` file and the parameter above to make sure the mounted disk is boot persistent.
- Check the disk using `fdisk -l`.
- Save and exit the file.

### Umount
```sh
umount /dev/mapper/vg-anyname/lv-anyname
```
- Pretty much the opposite of `mount`. **Don't forget to mount back**

## Network magic
### Ip
```sh
ip a show scope global dynamic | awk '$1 == "inet" {print $2, $4}'
```
- Show IP and broadcast address.

### Curl
```sh
curl -4 https://icanhazip.com
```
- Show my external IP address.

## Text and data manipulation magic
### Sed
```sh
sed 's/"//g'
```
- Remove all double quotes on each line.

```sh
sed 's/[^ ]* //'
```
- Remove everything before the first space.

```sh
sed 's/,$//'
```
- Remove last comma on each line.

```sh
sed -e 's/\[[^][]*\]//g'
```
- Remove bracket and everything between a pair of bracket.

```sh
sed 's/,/ /g'
```
- Replace all commas with spaces.

### Awk
```sh
awk '{print $0}'
```
- Print all column.

```sh
awk '(NR>1)'
```
- Print all line except first.

```sh
awk 'NF'
```
- Remove empty lines from list result.

```sh
awk '{$1=$1};1
```
- Remove all leading, trailing whitespaces and tabs from each line.

```sh
awk 'NR==2{ print; }'
```
- Grep specific line numbers.

### Grep
```sh
grep -oP '\[\K[^\]]+'
```
- Extract everything inside brackets.

```sh
grep -m1 'searchstring' file_name
```
- Find only first occurence in a file.

## Multimedia magic
### FFmpeg
```{sh}
ffmpeg -i video_name.mp4 -i audio_name.m4a -c copy -map 0:v -map 1:a output_name.mp4
```
- Adding audio to a video without re-encoding.

### Yt-dlp
```sh
yt-dlp --extract-audio --audio-format mp3 https://ytvideolink
```
- Extract audio track from Youtube video and store as mp3.

## Security magic
### Nmap
```sh
nmap -sC -sV -p- -oN output-name ip-address
```
- Scan all of target port using default script, version detection and output the result normally.

```sh
nmap --script=http-enum -p80 ip-address
```
- Enumerate specific port using http-enum script.

### Gobuster
```sh
gobuster dir -u http://ip-address -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```
- Search name of the hidden directory on the web server with wordlist.

### Enum4linux 
```sh
enum4linux -M -G -S ip-address
```
- Enumerate machine list, group and user list with share list.

### Smbclient
```sh
smbclient //ip-address/Anonymous
```
- Mount and connect to the sharename (Anonymous).

### Hydra
```sh
hydra -l ssh-username -P /usr/share/wordlists/rockyou.txt ssh://ip-address
```
- Bruteforce SSH password with wordlist from known SSH username.
