[Reference](https://www.geeksforgeeks.org/awk-command-unixlinux-examples)


``` bash
awk -F ":" '{print $1}' /etc/passwd #Change seperator

awk -F ":" '{print $1"\t"$6"\t"$7}' /etc/passwd # Seperator Column BY "\t"

awk 'BEGIN{FS=":"; OFS="-"} {print $1,$6,$7}' /etc/passwd #Combine of the 2 above

awk -F "/" '/^\// {print $NF}' /etc/shells # Using pattern and NF for last match column

df | awk '/\/dev\/loop/ {print $1"\t"$2 + $3}' # Using calculator

awk 'length($0) > 7' /etc/shells # Using conditional

awk '$1 ~ /^[b,c]/ {print $0}' .bashrc # Certain pattern on column

awk 'match($0, /0/) {print $0 "has \"o\" character at " RSTART}' numbered.txt 

```


