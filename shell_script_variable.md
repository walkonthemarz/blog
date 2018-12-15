### Variables in shell scripts
Set shell output to a shell variable:
```
...
output=$(echo "$preVariable" |  awk -F '\/' '{print $4}')
...
```
Notice that $preVariable is quoted because this allows the script to deal with spaces in the $preVariable

Rsync -s: no space-splitting; wildcard chars only(Incase the filename you are syncing 
