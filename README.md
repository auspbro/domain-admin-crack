
## Windows入域电脑用户本地提权

### 目的:
本项目可以用来破解企业对加域电脑的权限管理，及对USB端口禁用的设定。
域用户在本地计算机上默认的权限不是很高，很多时候操作起来很不方便。拥有本地管理员权限的话，可以赋予域用户本地管理员的权限，方法有几种，但是命令行的方式最为简洁方便。

### 准备工具：
0. USB flash disk ( >=1GB )
1. [rufus](https://rufus.akeo.ie/?locale=zh_CN) (Create bootable device utility)
2. [Active@ Password Changer](http://www.password-changer.com/download.htm) (MS-DOS tool for crack Windows password)
3. [WEPE](http://www.wepe.com.cn/download.html) (WinPE toolkit)

### 方法:（该方法，不需要域管理员的权限，只需本地管理员的权限就可以了。图形化界面，必须要有与管理员权限才容易操作）

### Preparing a boot device
1. 使用 rufus 制作纯 Dos 启动 U 盘

![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/rufus_usage.png)

2. Active@ Password Changer 使用说明
### Password recovery softwate starting
* Boot from the floppy, USB drive or CD/DVD-ROM
* Active@ Password Changer program starts automatically
* If not run Active@ Password Changer by typing this command, along with [Enter]:
```
C:\> PWD_CHNG.EXE
```
![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/PWDCHNG_DOS_01.png)

* Press [1], [2] or [3] key for the action you want to perform.
* Press [Esc] to exit the program.

If you chose the first option on the Options screen you may select a particular volume to scan for a SAM database
![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/PWDCHNG_DOS_02.PNG)

* Press the [0].[9] key to select a particular volume to be scanned, or the [A] key to scan all volumes
* Press [Enter] to perform the default action (scan all volumes).

### Password recovery SAM Select 
If only one SAM database is detected, press [ENTER] to get users information:
![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/PWDCHNG_DOS_03_01.PNG)

If several SAM databases are detected you will be asked to choose the one you want to view or edit:
![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/PWDCHNG_DOS_03.PNG)

* Press [0] . [9] key for a particular SAM database based on the appropriate database location and volume information.

### Password recovery SAM Select
After the SAM database is scanned, a list of defined local users will be displayed:
![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/PWDCHNG_DOS_04.PNG)
* Press the [0] . [9] key to display information for a specific user
* Press [PgUp] or [PgDown] to scroll the list of users

```bash
The primary (built-in) Administrator is displayed in Cyan,
Other users who have Administrator privileges are displayed in Green; or, Dark Green if their account has been disabled,
Users who don't have Administrator privileges are displayed in White; or, Grey if their account has been disabled.
```
### Password reset and changing account parameters
After you select a specific user account you can see the account information:
![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/PWDCHNG_DOS_05.PNG)

To view and change permitted logon days and hours press the [PgDn] key:
![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/PWDCHNG_DOS_06.PNG)

To select and choose days and hours use arrow keys and the [Space] bar. Please note that hours are displayed in GMT (Greenwich Mean Time). You should take that into account for your local time zone.
```
Default Windows system accounts do not have the option for "permitted logon hours".
```
Press [Y] if you want to save your changes or press [Esc] to leave the previous account information unchanged and return to previous window (List of accounts)
![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/PWDCHNG_DOS_07_PRO.PNG)

After you reset the user's password:
* User's password becomes empty
* "Account is Disabled" flag (if any) gets cleared
* "Password never expires" flag is set.

开机选择从 U 盘启动，输入 PWD_CHNG.EXE 启动 tool 破解 Windows Administrator 账户密码，破解成功后以 Administrator 账户登入系统。

3. 用net命令：（需要联公司内网下执行，命令中 "quantacn.com" 为域名，"A0070575" 为待破解的用户名）
```
net localgroup Administrators /add quantacn.com\A0070575  
``` 
4. 进控制面板，用户，管理用户帐户，更改帐户 group 属性为 administrator, 将 quantacn.com 域中的用户 A0070575 加入本地的 Administrators 组。
5. 在组策略中配置开机和关机脚本，开始-运行-GPEDIT.MSC,计算机配置→Windows设置-脚本-开机／关机,浏览分别选择 EnNetwork.BAT 和 DisNetwork.BAT 脚本文件.


-----------------------------------------------分割线---------------------------------------------------------

## Windows 7 系统下 U 盘或其他可移动存储设备被拒绝访问解决方法

Windows 7 系统下插入 U 盘或可移动存储设备在资源管理器中可以显示相关存储设备的卷标信息，但是双击该设备卷标却提示“位置不可用，无法访问，拒绝访问”，而在别的电脑上就可以访问。碰到这样的情况一般如果实在企业可能是公司域管理员限制了 USB 端口。如果是自己的电脑可能是因为下载过了 Win7 旗舰版 sp1 安装包，在 administrator 帐户下安装的，卸载过 360 杀毒软件(有可能在这里禁用了可移动设备)。既然不是病毒的问题，那就是电脑某些设置的问题了，具体该如何解决呢?

### 解决方法：
* 在运行窗口中输入 gpedit.msc 回车，打开计算机配置-管理模版-系统-可移动存储访问，在所有可移动存储类：拒绝所有权限，里面显示的是未配置，按说是没有阻止访问的(也可能是显示的和注册表不一致呢)。点“已禁用”->应用，再点“未配置”->应用。重启计算机。

![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/image001.png)

* Select Computer Configuration or User Configuration based on the criteria noted above, then expand Administrative Templates → System → Removable Storage Access.Double-click the policy setting for WPD, (Windows Portable Devices) that corresponds to the kind of restriction you want enforced (for example, double-click WPD Devices: Deny read access if you want to deny read access to your Apple iPhone). Select the corresponding radio button to Enable or Disable a policy setting.
![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/image002.png)

这样U盘可以访问了，插入U盘后双击就不会显示“位置不可用，无法访问，拒绝访问”，这也是在Win7系统下的解决方法，碰到其他系统则有可能是别的原因，分享的这个方法也希望能帮助遇到此问题的朋友。
