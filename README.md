# Introduction
Let's face it, we are mere mortals that keep forgetting everytime we learn something new. So that's why I created this simple cheatsheet. Every time I forget something important, I want to refer back in one single place instead of scouring the Internet for the same answer that I did before. Hope this will help.

## System magic
### Show disk usage
```sh
df -h | awk '/^C:\\/ {print $3 "/" $2}'
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
ip addr | grep -m1 inet | sed 's/    //; s/scope global dynamic//g'
```

### Show most memory intensive process
```sh
ps axch -o cmd:15,%mem --sort=-%mem
```

### Show most CPU intensive process
```sh
ps axch -o cmd:15,%cpu --sort=-%cpu
```

### Show explicit installed packages (from history.log in Ubuntu)
```sh
cat /var/log/apt/history.log | grep install | awk '(NR>1)' | sed 's/[^ ]*//' | sed 's/apt install//g' | sed 's/  //'
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

## Youtube-dl magic
### Extract audio track from Youtube video and store as mp3
```sh
youtube-dl --extract-audio --audio-format mp3 https://ytvideolink
```
