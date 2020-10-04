Git udemy course
=================

|Command|Description|Example ||
|---:|---:|---:|----:|
|pwd|  print working directory  |-|-|
|cd|change directory| cd ..| cd ~ == cd /Users/myaccount|
|ls| list files and directories| ||
||ls -l | list, permissions, size, ..|
||ls -lh | size with KB, MB, ..|
||ls -a | include [.whatever] hidden files|
||ls -p | include the file type | |
||ls -s | ordered by size | |
||ls -r | list in reverse alphabetical order|  |
||ls -r ~/Documents/ |   | |
||ls -G | coloured result   | |
||ls -R | Recursively list subdirectories  | |
|Permissions||||
|chown  |change owner|||
|chmod  ReadWriteExecute(RWE) OwnerGroupOthers|chmod 764 file.txt|Owner=RWE(4+2+1=7), Group=RW(4+2=6) Others=R(4)||
|Delete file||||
|rm filename|rm -rf dir/|rm ./*||
|Create files||||
|touch |touch new-file.txt| |
|Create directory||||
|mkdir| make logdir| ||
|Find|find dir|||
|find . -type f -name "*.txt"|find . -type f -iname "*.txt"|find . -type f -not -iname "*.txt"| -i == ignore case sensitive|
|find . -type d -iname "rrr"| ||
|find . -size -1M| ||
|find . -size -1M -maxdepth 1| ||
|Grep| find things in files |grep "string-to-search" file(s)||
|grep "main" f1.txt| |||
|grep -i "main" ./* | ||-i == ignore case sensitive|
|find . -iname "f*" -exec grep -i -n "is" {} +|chain commands|||
|Others||||
|open . | open the current directory in the default app (finder on Mac)  | |
|cd| change directory|  |
|.| Alias of the current directory| |
|..| Alias of the parent directory| |
|>|  Redirecting the result of a command into a file  |echo "Hello world" > new-file.txt |
| pipe `|` tee  |  Redirecting and seeing in the console the result of executing command  |echo "Hello wporld" | tee new-file.txt |
|>>| append to the file | |
|cat| show the content of the file| |
|clear|clear terminal| |
|tab|autocomplete| ||
|echo |print to the terminal| |
|man | help on specific command| |
|nano txt editor| edit file (Ctrl X to exit) (Ctrl O to save)|||
|top||||
|ps aux| ps aux | grep "chrome"|||
|pgrep chrome| get the process id||
|kill -9 id ||||
|-|-|-|
|MAC- packages available in brew| brew search wget| |
