#### Supervisor 监控的进程工作目录

昨天使用supervisor监控swoole进程时，发现php总是无法读取到到某一个相对路径的文件，但是命令行手动启动却可以，后来才意识到是supervisor的工作目录问题，由于没有给监控的进程设置默认工作目录，读取文件使用的是相对路径，导致无法找到相应的文件。添加**directory**参数即可

```ini
[program:php-swoole]
directory=/swoole
command=/usr/bin/php /swoole/server.php
```

