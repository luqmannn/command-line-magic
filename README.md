# Introduction
Let's face it, we are mere mortals that keep forgetting everytime we learn something new. So that's why I created this simple cheatsheet. Every time I forget something important, I want to refer back in one single place instead of scouring the Internet for the same answer that I did before. Hope this will help.

## System magic
### Show C: disk usage
```sh
df -h | awk '/^C:\\/ {print $3 "/" $2}'
```

### Show formatted disk usage for ext4/xfs filesystems
```sh
df -h -t ext4 -t xfs | awk '{print $1, $3 "/" $2}' | column -t
```

### Show memory used/total
```sh
free -h | awk '/^Mem:/ {print $3 "/" $2}'
```

### Show top 10 largest files (descending order, starting from the current folder, recursively)
```sh
find . -printf '%s %p\n'| sort -nr | head -10
```

### Show top 10 largest folders
```sh
du -hsx * | sort -rh | head -10
```

### Show IP and broadcast address
```sh
ip a show scope global dynamic | awk '$1 == "inet" {print $2, $4}'
```

### Show most memory intensive process
```sh
ps axch -o cmd:15,%mem --sort=-%mem
```

### Show most CPU intensive process
```sh
ps axch -o cmd:15,%cpu --sort=-%cpu
```

### Show explicit installed packages
```sh
apt-mark showmanual
```

### Show current PATH environment variable
```sh
echo $PATH | tr : \\n
```

## Sed magic
### Remove all double quotes on each line
```sh
sed 's/"//g'
```

### Remove everything before the first space
```sh
sed 's/[^ ]* //'
```

### Remove last comma on each line
```sh
sed 's/,$//'
```

### Remove bracket and everything between a pair of bracket
```sh
sed -e 's/\[[^][]*\]//g'
```

### Replace all commas with spaces
```sh
sed 's/,/ /g'
```

## Awk magic
### Print all column
```sh
awk '{print $0}'
```

### Print all line except first
```sh
awk '(NR>1)'
```

### Remove empty lines from list result
```sh
awk 'NF'
```

### Remove all leading, trailing whitespaces and tabs from each line
```sh
awk '{$1=$1};1
```

### Grep specific line numbers
```sh
awk 'NR==2{ print; }'
```

## Grep magic
### Extract everything inside brackets
```sh
grep -oP '\[\K[^\]]+'
```

### Find only first occurence in a file
```sh
grep -m1 'searchstring' file_name
```

## Curl magic
### Show my external IP address
```sh
curl -4 https://icanhazip.com
```

## Systemctl magic
### Show all installed services
```sh
systemctl list-unit-files --state=enabled --no-pager
```

## FFmpeg magic
### Adding audio to a video without re-encoding
```{sh}
ffmpeg -i video_name.mp4 -i audio_name.m4a -c copy -map 0:v -map 1:a output_name.mp4
```

## Youtube-dl magic
### Extract audio track from Youtube video and store as mp3
```sh
youtube-dl --extract-audio --audio-format mp3 https://ytvideolink
```

## Nmap magic
### Scan all of target port using default script, version detection and output the result normally
```sh
nmap -sC -sV -p- -oN output-name ip-address
```

### Enumerate specific port using http-enum script
```sh
nmap --script=http-enum -p80 ip-address
```

## Gobuster magic
### Search name of the hidden directory on the web server with wordlist.
```sh
gobuster dir -u http://ip-address -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```

## Enum4linux magic
### Enumerate machine list, group and user list with share list
```sh
enum4linux -M -G -S ip-address
```

## Smbclient magic
### Mount and connect to the sharename (Anonymous).
```sh
smbclient //ip-address/Anonymous
```

## Hydra magic
### Bruteforce SSH password with wordlist from known SSH username
```sh
hydra -l ssh-username -P /usr/share/wordlists/rockyou.txt ssh://ip-address
```
