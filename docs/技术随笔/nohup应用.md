以某个应用执行为例
```
#!/bin/bash
ps -e|grep doc|awk -F ' ' '{print $1}'|xargs kill
nohup ./bin/doc >> output.out 2>&1 &
```

基本含义

/dev/null 表示空设备文件
0 表示stdin标准输入
1 表示stdout标准输出
2 表示stderr标准错误
> file 表示将标准输出输出到file中，也就相当于 1>file

2> error 表示将错误输出到error文件中

2>&1 也就表示将错误重定向到标准输出上

2>&1 >file ：错误输出到终端，标准输出重定向到文件file，等于 > file 2>&1(标准输出重定向到文件，错误重定向到标准输出)。

& 放在命令到结尾，表示后台运行，防止终端一直被某个进程占用，这样终端可以执行别到任务，配合 >file 2>&1可以将log保存到某个文件中，但如果终端关闭，则进程也停止运行。如 command > file.log 2>&1 & 。

nohup放在命令的开头，表示不挂起（no hang up），也即，关闭终端或者退出某个账号，进程也继续保持运行状态，一般配合&符号一起使用。如nohup command &。
