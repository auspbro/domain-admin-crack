
## Windows入域电脑用户本地提权

### 目的:
本项目可以用来破解企业对加域电脑的权限管理，及对usb端口禁用的设定。
域用户在本地计算机上默认的权限不是很高，很多时候操作起来很不方便。拥有本地管理员权限的话，可以赋予域用户本地管理员的权限，方法有几种，但是命令行的方式最为简洁方便。

### 方法:（该方法，不需要域管理员的权限，只需本地管理员的权限就可以了。图形化界面，必须要有与管理员权限才容易操作。）

1. 制作 Dos 启动 U 盘，开机按 F12 选择从 U 盘启动
2. 输入 PWD_CHNG.EXE 启动 tool 破解 Windows Administrator 账户密码，破解成功后以 Administrator 账户登入系统
3. 用net命令：（需要联公司内网下执行，命令中 "quantacn.com" 为域名，"A0070575" 为待破解的用户名）
```
net localgroup Administrators /add quantacn.com\A0070575  
``` 
4. 进控制面板，用户，管理用户帐户，更改帐户 group 属性为 administrator, 将 quantacn.com 域中的用户 A0070575 加入本地的 Administrators 组。
5. 在组策略中配置关机脚本，开始-运行-GPEDIT.MSC,计算机配置→Windows设置-脚本-关机,浏览选择此脚本文件.
Reboot之前禁用本地连接
