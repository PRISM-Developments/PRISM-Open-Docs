# 拆除早期的Windows时间炸弹

如果使用测试版Windows系统老要担心时间炸弹，未免有点让人不爽。于是就有许多人研究如何使得时间过了以后系统也不会反常，这种行为称为“拆炸弹”或者“破（解）炸弹”。



有时候，拆炸弹会有一些附带效果。Windows 2000测试版中，拆炸弹可以同时去除水印。~~另一些效果则属于副作用，比如有的情况下拆除炸弹会导致系统无法激活。~~(这是因为替换不正确的SPP文件后SPP服务挂掉了，属于方法不完美而不是拆炸弹本身的问题。)



目前，NT5已经有确定可行的办法拆炸弹，NT6以上部分系统也有。但是另一些办法则还有争议。

---

### TweakNT拆弹法  

---

(适合5048及之前所有能运行 TweakNT 的 Windows NT)

---

### 修改注册表拆弹法

---

(适合5048及之前所有的Windows NT，是依照TweakNT工作原理手动操作的方法。对于打开不了TweakNT的早期NT 5.0测试版具有重要意义。)



1.在安装了测试版系统的机器（当然平常都是虚拟机）上启动一个Windows PE，打开regedit。

2.选中HKEY_LOCAL_MACHINE，点击文件->加载配置单元。定位到 测试版系统目录（一般是C:\WINNT或C:\WINDOWS）\System32\config\ 目录下。

3.打开SYSTEM这个无后缀名的文件。

4.起个名字挂载进去。

5.挂载后，点进HKEY_LOCAL_MACHINE\刚刚的名字\ControlSet001\Control\Session Manager\Executive\。

6.修改PriorityQuantumMatrix的左起第5、6、7字节的值为00 00 00。

7.点进HKEY_LOCAL_MACHINE\刚刚的名字\setup\。

8.修改SystemPreFix的左起第3、4、5字节的值为00 00 00。

9.卸载刚刚挂载的配置单元。

之后重启[1]
。


用相似的原理还可以更改时间炸弹的长短甚至给无时间炸弹的系统加上炸弹，具体请参考在[百度贴吧的帖子](http://tieba.baidu.com/p/5197847676)

---

### Windows NT 6.x（Build 5112 及更新的版本）

---

#### AntiWPA拆弹法(适合5112～5270)


1.下载Antiwpa-Vista 5112或Antiwpa-Vista 5259或Antiwpa-Vista 5270。

https://pan.baidu.com/s/1hsACo1u 密码:mqyy

2.在Antiwpa-Vista的目录里提取slc.dll，并在PE打开5112～5270的Windows\System32里找到同名文件替换即可。

（提示1：建议备份原slc.dll文件）

（提示2：Antiwpa-Vista 5112可用于5112 5212 5219；Antiwpa-Vista 5259可用于5231 5259；Antiwpa-Vista 5270可用于5270。）

Windows 8 Build 7850 KMS Activator and Timebomb Remover v0.9.4

在打开该工具后，左边有一个"Remove Timebomb!"按钮。点击即可，工具会提示你重启。如果重启后没有作用，那就再试一次。

注意:由于该工具开发时，该Build仅泄露了32位的企业版一个镜像，因此64位版及其他SKU使用该工具可能没有效果。


#### Windows 8.1 spp替换法(适用于 Windows 10 的早期 Build)


这个方法已确认在 Windows 10 Build 9780-9845 测试可用。 

在开始之前，你需要:

一.将待处理系统的install.wim解压出来。它位于光盘镜像中的sources文件夹。

二.下载 Windows 8.1 的spp文件。你可以在[这里](http://pan.baidu.com/s/1eRE4i9G)下载。 

步骤:

一.以管理员方式打开命令提示符。



二.输入以下命令，先看一下你需要哪个索引。



dism /get-wiminfo /wimfile:"盘符:\文件夹名\install.wim"



（注意：名称很有可能会显示为Windows 8.1，不要介意）



三.挂载镜像。输入以下命令。 dism /mount-wim/wimfile:"盘符:\文件夹名\install.wim" /index:索引号 /mountdir:盘符:\挂载目标文件夹

操作完成后，打开挂载目标文件夹，你就可以看到待破解系统的文件了。

四.进入正题，进入盘符:\挂载目标文件夹\Windows\System32，然后搜索spp。

把以下文件和文件夹；


```
spp

spp.dll

sppsvc.exe

sppc.dll

sppobjs.dll

sppwinob.dll

sppwmi.dll

sppcext.dll

SppExtComObj.Exe
 ```
全部替换为 Windows 8.1 的版本。



如果是64位系统，你还需要替换这些位于C:\Windows\SysWOW64下的文件和文件夹：

```

spp

spp.dll

sppc.dll

sppwmi.dll

sppcext.dll

```
提示1:

NTSERVICE\TrustedInstaller 用这个可以恢复文件本来的Trustedinstaller权限。



提示2:



NTSERVICE\TrustedInstaller

NTSERVICE\sppsvc

批量获得完全控制权方法

首先把所有者从 TrustedInstaller 或其他东西改成你的用户名。



然后打开命令提示符
```bat
cacls 你要获得完全控制权的文件夹路径\*.* /T /E /G 你的用户名:F
```
五.这时候我们已经拆除炸弹了，是时候保存了。



保存命令：  
```bat
dism /unmount-wim /mountdir:盘符:\文件夹名 /commit
```


（注意：这时候不是 盘符:\文件夹名\install.wim）



提示：不要输错命令，否则功亏一篑。



卸载8.1的文件或出现保存时卸载不完全的卸载命令。

```bat

dism /unmount-wim /MountDir: 盘符:\文件夹名 /discard

```

提示：如果卸载不完全，先注销然后登录，再输入以上命令。②



具体请参考[在百度贴吧的帖子](http://tieba.baidu.com/p/4796090440)



注释以及引用  
[1] :由@BilinSun 著作，可能保留所有权利。

②由@维超尼尼 著作，保留所有权利。 