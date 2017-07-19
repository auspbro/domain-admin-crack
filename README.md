
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
5. 在组策略中配置开机和关机脚本，开始-运行-GPEDIT.MSC,计算机配置→Windows设置-脚本-开机／关机,浏览分别选择 EnNetwork.BAT 和 DisNetwork.BAT 脚本文件.

### Win7系统下U盘被拒绝访问怎么办?如何解决

win7系统下插入U盘可以显示信息，但是双击就会出现“位置不可用，无法访问，拒绝访问”，而在别的电脑上就可以访问。碰到这样的情况一般是你的电脑下载过了Win7旗舰版sp1安装包，在administrator帐户下安装的，卸载过360杀毒软件(有可能在这里禁用了可移动设备)。既然不是病毒的问题，那就是电脑某些设置的问题了，具体该如何解决呢?

解决方法：
* 第一次，找了一个夺取管理员权限的注册表代码(reg文件)，执行了，就得到权限，可以访问了。然而很可惜，第二次又被拒绝访问了。第三次，使用gpedit.msc，计算机配置-管理模版-系统-可移动存储访问，在所有可移动存储类：拒绝所有权限，里面显示的是未配置，按说是没有阻止访问的(也可能是显示的和注册表不一致呢)。点“已禁用”，应用，再点“未配置”，应用。重启计算机。

![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/image001.png)

* Select Computer Configuration or User Configuration based on the criteria noted above, then expand Administrative Templates → System → Removable Storage Access.Double-click the policy setting for WPD, (Windows Portable Devices) that corresponds to the kind of restriction you want enforced (for example, double-click WPD Devices: Deny read access if you want to deny read access to your Apple iPhone). Select the corresponding radio button to Enable or Disable a policy setting.
![image](https://github.com/auspbro/domain-admin-crack/blob/master/image/image002.png)

这样U盘可以访问了，插入U盘后双击就不会显示“位置不可用，无法访问，拒绝访问”，这也是在Win7系统下的解决方法，碰到其他系统则有可能是别的原因，分享的这个方法也希望能帮助遇到此问题的朋友。
