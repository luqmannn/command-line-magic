# Introduction
Let's face it, we are mere mortals that keep forgetting everytime we learn something new. So that's why I created this simple cheatsheet. Every time I forget something important, I want to refer back in one single place instead of scouring the Internet for the same answer that I did before. Hope this will help.

## System magic
### Show disk usage
`df -h | awk '/^C:\\/ {print $3 "/" $2}'`

### Show memory used/total
`free -h | awk '/^Mem:/ {print $3 "/" $2}'`

### Show IP and broadcast address
`ip addr | grep -m1 inet | sed 's/    //' | sed -e 's/scope global dynamic//g'`

### Show most memory intensive process
`ps axch -o cmd:15,%mem --sort=-%mem`

### Show most CPU intensive process
`ps axch -o cmd:15,%cpu --sort=-%cpu`

### Show explicit installed packages (from history.log in Ubuntu)
`cat /var/log/apt/history.log | grep install | awk '(NR>1)' | sed 's/[^ ]*//' | sed 's/apt install//g' | sed 's/  //'`

## Sed magic
### Remove all double quotes on each line
`sed 's/"//g'`

### Remove everything before the first space
`sed 's/[^ ]* //'`

### Remove last comma on each line
`sed 's/,$//'`

### Remove bracket and everything between a pair of bracket
`sed -e 's/\[[^][]*\]//g'`

### Replace all commas with spaces
`sed 's/,/ /g'`

## Awk magic
### Print all column
`awk '{print $0}'`

### Print all line except first
`awk '(NR>1)'`

### Remove empty lines from list result
`awk 'NF'`

### Remove all leading, trailing whitespaces and tabs from each line
`awk '{$1=$1};1`

### Grep specific line numbers
`awk 'NR==2{ print; }'`

## Grep magic
### Extract everything inside brackets
`grep -oP '\[\K[^\]]+'`

### Find only first occurence in a file
`grep -m1 'searchstring' file_name`
