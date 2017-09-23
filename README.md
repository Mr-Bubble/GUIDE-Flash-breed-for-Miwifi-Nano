# __刷机前注意自己路由版本！！！__

# __刷机前注意自己路由版本！！！__

# __刷机前注意自己路由版本！！！__

_重要的事情说三遍_

__请用开发版2.1.22，稳定版2.1.26或以前的版本固件，新版固件已经修复漏洞了.__

__老版固件进群下载： 299928214 （有福利）__ _秋大也在群里！_

# 小米路由器青春版免拆机破译SSH刷入breed教程

## 破译telnet

### 首先登陆路由器管理界面192.168.31.1

![picture1](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture1.png)

[http://192.168.31.1/cgi-bin/luci/;stok=075a9192918557c27cdbcae2175281d9/web/home#router](http://192.168.31.1/cgi-bin/luci/;stok=075a9192918557c27cdbcae2175281d9/web/home#router)

把你浏览器中的`/web/home#router`替换成`/api/xqsystem/set_name_password?oldPwd=当前路由的密码&newPwd=admin`

![picture2](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture2.png)

然后回车会出现这样的一个界面

![picture3](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture3.png)

admin为你的新管理密码和root密码

回车以后网页显示`{"code":0}`就是成功了。

和上面一样登陆路由器，把你浏览器中的/web/home#router替换成/api/xqnetwork/set_wifi_ap?ssid=tianbao&encryption=NONE&enctype=NONE&channel=1%3B%2Fusr%2Fsbin%2Ftelnetd
然后等一会浏览器返回：

![picture4](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture4.png)

`{"msg":"未能连接到指定WiFi(Probe timeout)","code":1616}`
这时候应该已经启动telnet了，telnet登陆路由吧(win10需控制面板添加telnet)

![picture5](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture5.png)

然后用putty中文版1.0v；
连接类型telnet 主机名称192.168.31.1

![picture6](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture6.png)

打开

![picture7](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture7.png)

输入root然后回车

![picture8](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture8.png)

然后输入密码admin《linux这个登陆指令是不显示密码的你直接输入然后回车就行了》

得到这样一个界面

![picture9](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture9.png)

依次输入指令:

``` bash
sed -i ":x;N;s/if \[.*\; then\n.*return 0\n.*fi/#tb/;b x" /etc/init.d/dropbear
/etc/init.d/dropbear start
nvram set ssh_en=1; nvram commit
```

![picture10](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture10.png)

这时候就可以用常用的__PuTTY__或者__WinSCP__登陆了。


输入SSH和备份编程固件

然后打开WINSCP文件协议SCP 主机名192.138.31.1 

用户名root密码admin

![picture11](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture11.png)

如果出现这个直接点更新  __{如果提示密码的话就输入admin}__

![picture12](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture12.png)


打开putty输入选择SSH

![picture13](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture13.png)

登录进去

输入指令 `cat /proc/mtd`

![picture14](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture14.png)

mtd0-10都是固件和分区其中mtd0是编程固件

输入指令`dd if=/dev/mtd0 of=//tmp/all.bin`
Mtd0是编程固件已经包括1-10里面的东西了，不放心的朋友可以都把他们备份下来这里我就不全部备份了。《输入指令后一定要备份到电脑上后才操作第二条指令预防空间不足备份失败。

![picture15](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture15.png)


这时候已经备份下来了在路由器tmp这个文件夹我们可以用WINSCP查看和下载了！下载的时候右击all.bin选择下载并删除。

![picture16](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture16.png)

![picture17](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture17.png)


下载的时候一定要选好文件夹避免等会到文件。如果要备份其他分区的话重复上述操作就行了
相关指令

``` bash
dd if=/dev/mtd0 of=//tmp/all.bin
dd if=/dev/mtd1 of=//tmp/bootloader.bin
dd if=/dev/mtd2 of=//tmp/config.bin
dd if=/dev/mtd3 of=//tmp/Factory.bin
dd if=/dev/mtd4 of=/tmp/OS1.bin
dd if=/dev/mtd5 of=/tmp/rootfs.bin
dd if=/dev/mtd6 of=/tmp/OS2.bin
dd if=/dev/mtd7 of=/tmp/data.bin
dd if=/dev/mtd8 of=/tmp/overlay.bin
dd if=/dev/mtd9 of=/tmp/crash.bin
dd if=/dev/mtd10 of=/tmp/firmware.bin
```

## 刷入reed
WINSCP  选择SCP协议 复制breed.bin 到/tmp

![picture18](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture18.png)

PUTTY写入breed
输入指令
`mtd -r write /tmp/breed.bin Bootloader`

![picture19](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture19.png)


刷入后，机器会重新启动，固定电脑有线网卡的IP为192.168.1.100

断电，用硬物顶住路由器set键开机，等到路由器闪的时候，松开reset键，电脑上在浏览器中输入192.168.1.1，就进入breed，在该控制台下就可以放心刷潘多拉等固件了！

![picture20](https://github.com/edward-p/GUIDE-Flash-breed-for-Miwifi-Nano/raw/master/screenshots/Picture20.png)