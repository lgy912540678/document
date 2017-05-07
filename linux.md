                                                                                         					CentOS 6.3 安装过程
准备工作：
	用VMware创建一个新的虚拟机，系统选择CentOS ，硬盘选择20G，安装目录自己指定。
1、放入光盘

注：首先打开虚拟硬件的配置界面，选择CD/DVD的选项，在如图的地方添加镜像文件的位置（给你们拷贝的CentOS 6.3的镜像），然后打开虚拟机电源。
2、安装欢迎界面

进入安装欢迎界面，有四个选项：
“Install or upgrade an existing system”：安装或升级现有系统
“Install system with basic video driver”：安装过程采用基本的显卡驱动
“Rescue installed system”：进入系统修复模式
“Boot from local drive”：退出安装从硬盘启动
“Memory test”：存储介质检测

3、检测安装介质

注：由于我们是在虚拟的物理环境下安装，所有的硬件是没有问题的，所以直接skip 跳过，不需要检测。

4、语言设置


5、键盘设置


6、存储设备选择界面


7、主机名配置


8、时区选择


9、设置管理员密码

根基密码原则：复杂性，易记性，时效性
（为了试验快速，可以设置为简单的123123）

10、选择分区类型

选择创建自定义布局
11、分区界面


	1）点击创建
	
	选择标准分区
	2）添加分区

三个必须要有的分区/boot、swap和/分区，注意三个分区的大小

12、引导分区安装


13、软件包选择

选择basic server 安装常用软件包
14、安装过程

最后完成之后点击重新引导，等待系统重新启动进入操作系统


重启之后的界面：

输入用户名和你自己安装时设置的密码进行登录
用户名：root    输入密码后直接点击回车登录


登录成功后会显示上面红色框里面的内容（最后一次登录时间以及提示符）
1. 修改IP地址和防火墙
在提示符后面输入setup命令回车会显示下面的内容：

操作方法：光标上下键可以选中条目，回车键进入（或者确认），tab键进行按钮的切换。
1.首先修改防火墙设置（关闭防火墙），用空格键取消掉*号，确认修改保存，回到上图界面。（此操作重启后依旧生效）
2.回车进入第三行的网络配置，选择网卡配置，选择第一块网卡（新装的只有一个），进入下图界面：

修改完成后保存修改，最后退出setup命令界面。
2.关闭Linux防护机制
①临时关闭防火墙：
命令：iptables –F		#清空防火墙列表
②临时关闭SElinux防护：
命令：setenforce 0	#关闭特殊防护
注：命令和选项之间有空格
3.修改网卡配置文件，允许网卡能随着系统开机一起启动。
用vim 命令打开/etc/sysconfig/network-scripts/ifcfg-eth0    （#路径比较长，认真写，不要出现错字母）
打开后正常的显示：

将文件中的ONBOOT=no   修改为ONBOOT=yes
（修改流程： vim 打开文件后，先按下i 键 进入编辑模式，光标移动到相应位置进行内容修改。修改完成后按下esc键退出编辑模式，然后按shift+：（冒号键）在后面直接输入wq，回车并退出了修改）
4.最后我们需要重新启动下网卡的服务：
 service  network  restart  
正常启动后用ifconfig 命令查看下网卡eth0 是否启动成功，并且IP地址是否生效。
成功的如下图：
 
5.修改虚拟机的链接方式：

注：设备状态两个都要勾上，选择桥接（不要选择复制物理网络连接状态）

实现远程工具连接：
目的：
1.	省去用ctrl+alt切换时间
2.	终端的字体颜色或者背景都可以修改，并且可以同时开几个窗口。
3.	方便了文件的互传
先把Xshell 安装好，然后打开新建一个会话连接

确定修改，然后连接，输入用户名和密码，登录成功

Winscp文件传输工具：

输入主机地址，以及账号，密码登录即OK

第二天	Linux常用命令

		一 	linux命令的格式

		1、命令  [选项]  [参数]

		ls	list	显示目录下内容

		①	命令名称：ls
			命令英文原意：list
			命令所在路径：/bin/ls
			执行权限：所有用户
			功能描述：显示目录文件

		②	ls	名直接回车，显示目录下内容

		ls  -l			长格式显示		(缩略选项用一个减号，完整选项用两个减号)


		-rw-------    1   root    root    1190    08-10 23:37     anaconda-ks.cfg
		第一项：	        权限位	
		第二项：  1		引用计数
		第三项：  root	所有者
		第四项：  root   属组
		第五项：  		大小	
		第六项			最后一次修改时间
		第七项			文件名

		ls  -a   	显示所有文件（包含隐藏文件）
		ls  -al
		ls  -hl		文件大小显示为常见大小单位	B	KB	MB
		ls  -d		显示目录本身，而不是里面的子文件

		ls  -l       文件名		

	提示符：（特殊字符）
		[root@localhost src]#

		[当前登录用户@主机名 当前所在目录]#

				#		超级用户
				$		普通用户

				当前所在目录：~		    用户家目录	
							管理员		/root
							普通用户		/home/用户名

	二 	目录操作命令

			1）	cd	切换所在目录

				①	命令名称：cd
					命令英文原意：change directory
					命令所在路径：shell内置命令
					执行权限：所有用户


				②cd  /usr/local/src
  
				相对路径：参照当前所在目录，进行查找。一定要先确定当前所在目录。    root]#cd  ../usr/local/src
				绝对路径：cd  /usr/local/src		从根目录开始指定，一级一级递归查找。在任何目录下，都能进入指定位置

				cd  ~		进入当前用户的家目录		/root		/home/aa/
				cd		
				cd  -		进入上次目录
				cd  ..		进入上一级目录
				cd  .		进入当前目录

			2)	pwd	显示当前所在目录
				命令名称：pwd
				命令英文原意：print working directory
				命令所在路径：/bin/pwd
				执行权限：所有用户

			3）	linux常见目录
				/		根目录
				/bin		命令保存目录（普通用户就可以读取的命令）
				/boot	启动目录，启动相关文件
				/dev		设备文件保存目录
				/etc		配置文件保存目录
				/home	普通用户的家目录
				/mnt		系统挂载目录
				/media		挂载目录
				/root	超级用户的家目录
				/tmp		临时目录
				/sbin	命令保存目录（超级用户才能使用的目录）
				/proc	直接写入内存的		
				/usr		系统软件资源目录
					/usr/bin/		系统命令（普通用户）
					/usr/sbin/		系统命令（超级用户）
				/var		系统相关文档内容
					/var/log/		系统日志位置 


				
			4）	建立目录
				mkdir  目录名
				命令名称：mkdir
				命令英文原意：make directories
				命令所在路径：/bin/mkdir
				执行权限：所有用户

				mkdir  -p  11/22/33/44		递归建立目录
			
			5）	删除目录
				rmdir  目录			只能删除空目录
				命令名称：rmdir
				命令英文原意：remove empty directories
				命令所在路径：/bin/rmdir
				执行权限：所有用户

三 	文件操作命令

			1）创建空文件或修改文件时间

				touch  文件名
				命令名称：touch
				命令所在路径：/bin/touch
				执行权限：所有用户

			2）删除
				rm  -rf  文件名
					-r  删除目录
					-f	强制
				命令名称：rm
				命令英文原意：remove
				命令所在路径：/bin/rm
				执行权限：所有用户


			3）cat  文件名		查看全部文件内容
				命令名称：cat
				命令所在路径：/bin/cat
				执行权限：所有用户

				-n	列出行号

			4）more  文件名	分屏显示文件内容
				命令名称：more
				命令所在路径：/bin/more
				执行权限：所有用户

				空格向下翻页			b   向上翻页		q  退出

			5） head  文件名 	显示文件前10行    tail 
				命令名称：head
				命令所在路径：/usr/bin/head
				执行权限：所有用户
		
				head  -n  行数   文件名		指定显示文件前n行
				head  -n  20  文件名
				head  -20  文件名

				ctrl+c		强制终止
				ctrl+l			清屏


			6）	链接文件		
			ln
			命令名称：ln
			命令英文原意：link
			命令所在路径：/bin/ln
			执行权限：所有用户

					快捷方式
					新建的链接，占用不同的硬盘位置
					修改一个文件，两都改变
					删除源文件，软连接打不开

					ln  -s  源文件  目标文件		文件名都必须写绝对路径

四	文件和目录都能操作的命令

			1）rm		删除文件或目录

			2）复制
			命令名称：cp
			命令英文原意：copy
			命令所在路径：/bin/cp
			执行权限：所有用户

			cp  源文件  目标位置

				-r  复制目录
				-p	连带文件属性复制
				-d	若源文件是链接文件，则复制链接属性
				-a	相当于  -pdr

			cp  aa  /tmp/		原名复制
			cp  aa  /tmp/bb		改名复制


			3）剪切或改名
			命令名称：mv
			命令英文原意：move
			命令所在路径：/bin/mv
			执行权限：所有用户

			mv  源文件  目标位置

			mv  /root/aa  /tmp/

			mv  aa  bb


五	权限管理

		1	权限位
			-rw-r--r--   1   root root     0 08-11 01:45 aa

			权限位是十位
			第一位：	代表文件类型

				-	普通文件
				d	目录文件
				l	链接文件
		

			九位		属主权限u=user    属组权限g=group     其他人权限o=other

				r	读		4
				w	写		2
				x	执行		1

		2	修改权限
			chmod
			命令名称：chmod
			命令英文原意：change the permissions mode of a file
			命令所在路径：/bin/chmod
			执行权限：所有用户

			chmod  u+x  aa		aa文件的属主加上执行权限
			chmod  u-x  aa
			chmod  g+w,o+w  aa
			chmod  u=rwx  aa

			chmod  755  aa		
			chmod  644  aa


		3	权限意义：
			1）权限对文件的含义
				r：读取文件内容		cat  more  head  tail
				w：编辑、新增、修改文件内容		vi  echo  nano
				   但是不包含删除文件
				x：可执行		
							
			2）权限对目录的含义
				r：可以查询目录下文件名		ls
				w：具有修改目录结构的权限。如新建文件和目录，删除此目录下文件和目录，重命名此目录下文件和目录，剪切			touch  rm  mv  cp
				x：可以进入目录			cd

		4	属主和属组命令
			chown
			命令名称：chown
			命令英文原意：change file ownership
			命令所在路径：/bin/chown
			执行权限：所有用户

			chown  用户名  文件名		改变文件属主

			chown  user1  aa		user1必须存在

			chown  user1:user1  aa	改变属主同时改变属组

			useradd  用户名 		    添加用户
			passwd  用户名			设定用户密码			

六	帮助命令
		1	man  命令名			查看命令的帮助	
			命令名称：man
			命令英文原意：manual
			命令所在路径：/usr/bin/man
			执行权限：所有用户
	
		2	命令  --help			查看命令的常见选项
	
七	查找命令
		1	whereis  命令名		查找命令的命令，同时看到帮助文档位置
			命令名称：whereis	
			命令所在路径：/usr/bin/whereis
			执行权限：所有用户

		2	find				搜索命令			
			命令名称：find
			命令所在路径：/usr/bin/find
			执行权限：所有用户

			按照文件名查找
			find  查找位置   -name  文件名
			find  /  -name  aabbcc			按照文件名查找
                     -iname			        按照文件名查找，不区分大小写

			按照用户
			-user  用户名		按照属主用户名查找文件
			-group  组名		    按照属组组名查找文件
			-nouser		        找没有属主的文件

		
			按照文件类型  权限  文件大小
				
			-type 类型 		按照文件类型查找		f：普通		d：目录		l：链接

			find   /root  -perm  644		 按照权限查找
             
             -size     -5k     +5k        k  M  G
           
             二次筛选
             -exec  命令    {}  \;
			

		3	grep 	“字符串”  文件名		查找符合条件的字串行。
			命令名称：grep
			命令所在路径：/bin/grep
			执行权限：所有用户

			grep  -i  “root”  /etc/passwd
				 -v		反向选择
				 -i 		忽略大小写


		4	管道符			
			命令1  |  命令2			命令1的执行结果，作为命令2的执行条件

			
			cat  文件名  |  grep  “字串”			提取含有字符串的行
			grep  “字符串”  文件名

			ls  -l  /etc  |  more						分屏显示ls内容

	
八	压缩和解压缩
		
			.gz		.bz2		linux可以识别的常见压缩格式	
			.tar.gz	.tar.bz2	常见的压缩和打包命令

			压缩同时打包
				tar  -zcvf  压缩文件名  源文件
				tar  -zcvf  aa.tar.gz  aa
					-z  识别.gz格式
					-c：	压缩
					-v：显示压缩过程
					-f：指定压缩包名

				tar  -zxvf  压缩文件名		解压缩同时解打包

				tar  -jcvf  压缩文件名  源文件	压缩同时打包
				tar  -jcvf  aa.tar.bz2  aa

				tar  -jxvf  aa.tar.bz2		解打包同时解压缩

			查看不解包
				tar  -ztvf  aa.tar.gz		查看不解包
				tar  -jtvf  aa.tar.bz2
					 -t  只查看，不解压

				tar -jxvf root.tar.bz2 -C /tmp/	指定解压缩位置

九	关闭和重启命令
		
			1）shutdown  -h  now			没有特殊情况，使用此命令
				-h	关机
				-r	重启

			   shutdown  -r  now

					命令名称：shutdown
					命令所在路径：/sbin/shutdown
					执行权限：root	
			2）reboot
				命令名称：reboot
				命令所在路径：/sbin/reboot
				执行权限：root
			
十	挂载命令
		
		linux所有存储设备都必须挂载使用，包括硬盘
			命令名称：mount
			命令所在路径：/bin/mount
			执行权限：所有用户

			光盘挂载

			/dev/sda1	第一个scsi硬盘的第一分区
			/dev/cdrom	光盘
			/dev/sr0		光盘			
		
			mount  -t  文件系统  设备描述文件  挂载点（已经存在空目录）
			mount  -t  iso9660  /dev/cdrom  /mnt/cdrom

			光盘卸载
			umount  /dev/cdrom 
			umount  /mnt/cdrom 		重点：退出挂载目录，才能卸载
			
		 	
            fdisk -l   查看设备名称  /dev/sda  /dev/sdb  /dev/sdc      

            mount /dev/sdb1  /mnt/usb
            
            umount /mnt/usb  (退出挂载点) 卸载 



十一 	网络命令
				
1	ifconfig  查询本机网络信息
				命令名称：ifconfig
				命令英文原意：interface configure
				命令所在路径：/sbin/ifconfig
				执行权限：root

2	ping	测试网络连通性
				命令名称：ping
				命令所在路径：/bin/ping
				执行权限：所有用户

			    ping  -c  次数  ip		探测网络通畅



第三天	vi编辑器		

	一 	vi编辑器简介
		vim		全屏幕纯文本编辑器

	二	vim使用
		1	vi 模式 
			vi  文件名
			


			命令模式
			输入模式
			末行模式

			命令---->输入    a  追加    i 插入   o  打开 
			命令---->末行   :w  保存    :q   不保存退出   
                    
			
		2	命令模式操作

			1）光标移动
			hjkl		

			:n		移动到第几行

			gg		移动文件头
			G		移动到文件尾

			3）删除字母
			x		删除单个字母
			nx		删除n个字母

			4）删除整行	剪切
			dd		删除单行
			ndd		删除多行
			p		粘贴
			P（大） 粘贴到光标前

			dG		从光标所在行删除到文件尾

			5）复制
			yy	
			nyy

			6）撤销
			u		撤销
			ctrl+r	反撤销

			7)显示行号
			:set  nu	
			:set  nonu	

			8)颜色开关
			:syntax  off
			:syntax  on

vi配置文件
~/.vimrc	手工建立的，vi配置文件

			9)查找			掌握
			/查找内容		向下查找
			
			n	下一个
			N	上一个

			10）替换		
			：1,10s/old/new/g		 替换1到10行的所有old为new
			：%s/old/new/g		     替换整个文件的old为new
						g	         范围内所有old换为new

			：1,5s/^/#/g			注释1到5行
			:1,5s/^#//g			取消注释

			:1,5s/^/\/\//g		文件头加入//
			:1,10s/^\/\///g	    取消注释

软件包安装	

一 	软件包分类
         Tarball  filename.tar.gz   filename.tar.bz2  
		源码包：	    优点：	特点	开源	 自由定制，效率更高
					缺点：	编译时间长，一旦报错，很难解决
		

		二进制包（编译之后的包）：  rpm包   redhat package manager
				特点：安装速度快		简易
				缺点：自定义性差		依赖性
        

	二	rpm安装

(一) 手工RPM命令安装

		1	包名-版本号-发布次数-适合linux系统-硬件平台.rpm

		2	依赖性

			库文件依赖查询		www.rpmfind.net
		    (rpm -ivh /mnt/CentOS/mysql-connector-odbc-3.51.26r1127-1.el5.i386.rpm )

			Libodbcinst.so.2


		3	安装 tree （目录树）
			
			rpm  -ivh  软件包（绝对路径）
				-i  安装	-v	显示详细信息		-h 显示进度

			rpm  -Uvh  软件包
				 -U     升级

		4	卸载
			rpm  -e  软件包
				--nodeps	不检查依赖性

		5	查询  
			rpm  -q  		查询包是否安装
			rpm  -qa  | grep  httpd   mysql 	显示所有安装包
			
			rpm  -qi   软件包	 查询包的信息		
			rpm  -qip  软件包	 查询没有安装的包的信息
				-i	information


			rpm  -ql   软件包	查询包中文件的安装位置
			rpm  -qlp  软件包	查询没有安装的包，将安装的位置
				 -l	list
					

			rpm  -qf  系统文件名		查询系统文件属于哪个包

	    	

		（二	）  yum 命令  rpm包管理方式

		yum  -y  install  软件包		安装			-y  自动回答yes
		yum  -y  remove   软件包		
		yum  -y  update   软件包
		yum  list		 查询所有可以安装的包

		光盘作为yum源：
			1	cd  /etc/yum.repos.d/
				mv  CentOS-Base.repo  CentOS-Base.repo.bak

			2	mount /dev/sr0  /mnt/cdrom

			3	vi  /etc/yum.repos.d/CentOS-Media.repo
				baseurl=file:///mnt/cdrom/	指定yum源位置
				enabled=1					yum源文件生效
				gpgcheck=0					rpm验证不生效

		yum  -y  install  gcc 		(gcc是c语言编译器，不装gcc，源码包不能安装)


	三	源码包安装

		1	远程传输工具传输apache到linux。
				httpd

		2	安装
			1） 解压

			2） cd  解压目录
			
			3）  查看安装文档

				INSTALL		README

			4）编译前准备
			./configure  --prefix=/usr/local/apache2

				功能：
					1	检测系统环境，生成Makefile
					2	定义软件选项

			5)编译				
			make

			6）编译安装
			make  install

			报错判断：
				第一：安装过程是否停止
				第二：注意error  warning  no  等错误报警
		
		3	启动
			/usr/local/apache2/bin/apachectl  start  （测试）

		4	删除   make  clean	

			直接删除安装目录     	


补充：
	date		查看系统时间
	date  -s  20190220		设定日期
	date  -s  09:30:00		设定时间


	du  -sh  目录名		统计目录大小
		-s	和
		-h	单位
第四天 	用户和用户组管理


一	用户管理命令		  

		用户信息文件：	/etc/passwd
			aa:x:501:501:空:/home/aa:/bin/bash
			第一列：用户名
			第二列：密码位
			第三列：UID		用户ID			=>500		普通用户
			第四列：GID		初始组ID
			第五列：用户说明
			第六列：家目录
			第七列：用户登录之后的权限

		影子文件：	 /etc/shadow

		组信息文件：	 /etc/group

			zhangsan:x:500:
			组名：组密码位：组ID：组中附加用户
        

		1	添加用户
			useradd  用户名		

			useradd  选项  用户名
			选项：
				-g  组名	指定初始组		
				-G  组名	指定附加组，把用户加入组，使用附加组
				-c  添加说明
				-d  手工指定家目录，目录不需要事先建立/home/
				-s  	/bin/bash	手工指定用户登录之后的权限	


			useradd  -g  aa  bb		添加bb用户，同时指定初始组为aa
			useradd  -G  user1  aa	添加用户aa，指定附加组为user1


			初始组：每个用户初始组只能有一个，一般都是和用户名相同的组作为初始组
			附加组：每个用户可以属于多个附加组。要把用户加入组，都是加入附加组

		2	设定密码			
			passwd	        用户名
			passwd			改变当前用户密码
			passwd  root		改变root密码

		3	删除用户				
			userdel  -r  用户名
				-r  连带家目录一起删除

		4	添加组				
			groupadd  组名

		5	删除组				
			groupdel  组名		注意：组中没有初始用户。

		6	把已经存在的用户加入组					
	
			gpasswd  -a  用户名  组名		用户加入组
			gpasswd  -d  用户名  组名		把用户从组中删除

	二	用户相关命令				
		1	id  用户名		显示用户的UID，初始组，和附加组
			[root@localhost home]# id 用户
			

		2	su  -  用户名		切换用户身份			
				-	连带环境变量一起切换		
         
	三	ACL权限				

	
    给特殊身份的用户设置权限。

		1	getfacl  文件名		查询文件的acl权限

		2	setfacl  选项  文件名		设定acl权限  （set 设置）
					-m			设定权限
				    -b			删除权限

			setfacl  -m  u:用户名:权限   文件名
			setfacl  -m  g:组名：权限   文件名

			setfacl  -m u:aa:rwx  /test		给test目录赋予aa是读写执行的acl权限

			setfacl -m  u:cc:rx -R soft/		赋予递归acl权限，只能赋予目录
				-R  递归	

			setfacl -b  /test		删除acl权限
			setfacl -x  u:用户名  文件名		删除指定用户的ACL权限

		3	setfacl  -m d:u:aa:rwx -R /test		

默认权限只能赋予目录
	
			注意：如果给目录赋予acl权限，两条命令都要输入
				-R 递归
				-m  u:用户名：权限 -R 	 只对已经存在的文件生效
				-m  d:u:用户名：权限	-R	只对未来要新建的文件生效

	四	输出重定向

		1	输出重定向
				把应该输出到屏幕的输出，重定向到文件。

				>	覆盖      ls  >  aa		覆盖到aa

				>>	追加      ls  >>  aa		追加到aa
			
      		ls  gdlslga  2>>aa		错误信息输出到aa		强调：错误输出，不能有空格
						2	错误信息
		
			lss  >>  aa  2>&1		错误和正确都输入到aa，可以追加
						2>&1  	    把标准错误重定向到标准正确输出
       
			lss  >>  aa  2>>/tmp/bb		正确信息输入aa，错误信息输入bb

                    

服务和进程管理

	进程管理三个主要任务：
		判断服务器健康状态
		查看所有正在运行的进程
		强制终止进程

	一	进程查看		

		1	ps  aux		查看当前系统所有运行的进程
			-a 	显示前台所有进程
			-u	显示用户名
			-x	显示后台进程

			user： 用户名
			pid：	进程id。PID		1  init  系统启动的第一个进程
			%CPU	cpu占用百分比
			%MEM	内存占用百分比
			VSZ	虚拟内存占用量		KB
			RSS	固定内存占有量
			tty	登录终端				alt+F1-F7
									
			stat	状态		S：睡眠		D：不可唤醒	R：运行	  T：停止  Z：僵死  W：进入内存交换	X：死掉的进程 	<:高优先级	N：低优先级	L：被锁进内存		s：含子进程	+：位于后台	l：多线程
			start	进程触发时间
			time		占用cpu时间
			command	进程本身

		2	pstree		 查看进程树

		3	top

			第一行：	系统当前时间		系统持续时间		登录用户		1,5,15分钟之前的平均负载
			第二行：进程总数
			第三行：CPU占用率		%id		空闲百分比
			第四行：内存使用：	    总共		使用		空闲		缓存
			第五航：swap使用

			操作命令		M	内存排序
						P	CPU排序
						q	退出


		4	进程管理		终止进程
             ps   aux    组合使用
			kill  PID		结束单个进程  结束进程
			-9  强制
             
             pstree
			killall  -9   进程名		结束一类进程
			pkill    -9   进程名

			w			判断登录用户
			pkill  -9  -t  终端号	把某个终端登录的用户踢出
			pkill  -9  -t tty1		把本地登录终端1登录用户踢出
   
 	二	linux服务管理
		
		1	分类
			1）系统默认安装的服务			
			2）源码包安装的服务 
		（一）系统默认安装的服务
		1	确定服务分类
			chkconfig  --list		查看服务的自启动状态
				运行级别：0-6
					0	关机
					1	单用户模式
					2	不完全多用户，不包含NFS服务  无网络登录
					3	完全多用户	字符界面   
					4	未分配
					5	图形界面
					6	重启

					init  0 关机	 	
init  6	重启

					runlevel			查询系统当前运行级别

					vi  /etc/inittab
					id:3:initdefault:		定义系统默认运行级别

		2	独立的服务器管理		

			1）启动			
				①
				/etc/rc.d/init.d/服务名   start|stop|restart|status
				/etc/rc.d/init.d/httpd  start

			    ②
				service   服务名   start|stop|restart|status

			2)自启动		
				①
				chkconfig  --level  2345  服务名  on|off

				②			
				vi  /etc/rc.local---->/etc/rc.d/rc.local
				/etc/rc.d/init.d/httpd  start

		3	ntsysv

			所有系统默认安装服务都可以使用ntsysv命令进行自启动管理

		（二）源码包安装的服务
			1源码包安装的服务			

			1）绝对路径启动
			/usr/local/apache2/bin/apachectl  start 

			2)自启动
			vi /etc/rc.local
			/usr/local/apache2/bin/apachectl  start
   
总结：
	服务管理：
		RPM包安装服务
				启动：	
					/etc/rc.d/init.d/服务名  start
					service  服务名  start

				自启动：
					chkconfig  --level  2345  服务名  on|off
					vi  /etc/rc.local		推荐
						/etc/rc.d/init.d/httpd  start


		源码包服务
			启动
					/usr/local/服务名/bin/服务名二进制执行文件  start

			自启动
					vi  /etc/rc.local
						/usr/local/apache2/bin/apachectl  start

	三	计划任务
             
			
			循环定时任务:		
				
			crontab  -e		编辑定时任务

			*  *  *  *  *   命令    

	　　　　第一个：一小时中第几分钟		0-59
			第二个：一天中第几个小时		0-23
			第三个：一个月中第几天		1-31
			第四个：一年第几个月			1-12
			第五个：一周中星期几			0-6	
             
             命令： 开启/关闭服务   service sshd start     service sshd stop
                                    /usr/local/apache2/bin/apachectl restart
                   
                    重启系统        reboot    shutdown -r  now
                    生成文件        echo  “Good Moring”  >>  /root/hello.txt
                    备份文件/目录   cp    /root/hello.txt   /tmp
                              

             0  6   *  *  *    命令
10  *  31  *  *   命令
			10  9  1  1  *    命令
			5   3  *  5,7,10  *  命令
			*/10  *  *  *  1-3   命令
		

			crontab  -l		查看系统定时任务
			crontab  -r  		删除定时任务

注意事项：
选项都不能为空，必须填入，不知道的值使用通配符*表示任何时间 
每个时间字段都可以指定多个值，不连续的值用,间隔，连续的值用-间隔
间隔固定时间执行书写为*/n格式 
命令应该给出绝对路径 
    星期几和第几天不能同时出现
    最小时间范围是分钟，最大时间范围是月

命令补充：
		    cat /proc/cpuinfo 文件保存了CPU设备信息  
			dmesg				查看系统启动信息

			cat  /var/log/dmesg		系统启动信息日志
			
			dmesg | grep eth0		查看eth0信息
			dmesg | grep CPU		    查看cpu信息


补充：

	IP地址：	Ipv4		2*32     Ipv6 

		tcp  	   网络通讯协议
		udp		   用户数据报协议


	常见网络端口：
		20  21	ftp服务		文件共享
		22		ssh服务		安全远程网络管理
		23		telnet服务
		25 		smtp：简单邮件传输协议	发信
		110		pop3：邮局协议			收信
		80		www	网页服务
		3306		mysql端口
		53		DNS端口
		
		/etc/services			所有系统常见端口

		端口数量 	tcp  65535		udp  65535


    网关：(Gateway)网间连接器、协议转换器。	

	DNS：	Domain  Name  System

			域名	 	-->		IP		正向解析
			IP		-->	   域名		反向解析

 北京网通：202.106.0.20   广州电信：202.96.128.143   114DNS：114.114.114.114 



	网络配置 

	一	IP地址配置

		1	setup 
		
			service network  restart

		2	ifconfig  eth0  ip  netmask  掩码			临时生效

		3	网卡配置文件

			1）/etc/sysconfig/network-scripts/ifcfg-eth0			网卡信息文件
		
DEVICE=eth0					网卡设备名
BOOTPROTO=none				是否自动获取IP。none：不生效	static：手动	dhcp：动态获取IP
BROADCAST=192.168.140.255			广播地址        
HWADDR=00:0c:29:21:80:48			mac地址
IPADDR=192.168.140.253			IP地址
IPV6INIT=yes					    IPv6开启
IPV6_AUTOCONF=yes				IPv6获取
NETMASK=255.255.255.0			掩码
NETWORK=192.168.140.0			网段
ONBOOT=yes					    网卡开机启动
TYPE=Ethernet					以太网
GATEWAY=192.168.140.1			网关

			2）/etc/sysconfig/network	 主机名配置文件 永久生效，但是要重启网卡
				HOSTNAME=localhost.localdomain

				hostname  	    临时设定主机名
				hostname			查看主机名

			3）/etc/resolv.conf			DNS配置文件

				nameserver  114.114.114.114  

	二	网络命令

		1	ifconfig		查看网卡信息

			
		2	ifup  eth0		ifdown  eth0		快速开启和关闭网卡 

		
		3	route				查看路由

			route  add   default  gw  192.168.140.1			手工设定网关，临时生效
			route  del   default  gw  192.168.190.6				删除网关
          
		4	netstat  	查看网络状态的命令
             		  -an		查看所有网络连接
					  -tlun		查看tcp和udp协议监听端口
					  -rn		查看路由	default：默认路由（网关）
             netstat -an | grep ESTABLISHED | wc -l		统计正在连接的网络连接数量

		5	ping  ip			探测网络通畅


		6	traceroute  ip或域名		探测/跟踪网络数据包的传输路径

		

VSFTP服务

		
	一	文件服务器简介

		ftp：在内网和公网使用。	服务器：windows，linux	客户端：windows，linux

		
        服务器搭建：

			1	ftp软件

				linux：	wu-ftp	    早期，不太安全
						proftp		增强ftp工具
						vsftp		安全，强大

				windows	IIS		 windows下网页搭建服务，可以搭建ftp服务
						Serv-U		专用ftp服务器

			2	原理

					开启 	21  	命令传输端口
							20	数据传输端口

			3	ftp的用户
				1）ftp允许登录用户	 系统用户   密码:系统密码
					上传位置：/home/家目录

				2）匿名用户	anonymous/ftp	密码：空  或者  邮箱地址	  				                 上传位置：/var/ftp/


		二	安装

			rpm  -ivh  vsftpd...........
			yum  install  vsftpd  -y

		三 	相关文件

			/etc/vsftpd/vsftpd.conf		配置文件

			/etc/vsftpd/ftpusers			用户访问控制文件	 写入此文件的用户都不能访问ftp服务器

			/etc/vsftpd/chroot_list		需要手工建立		定义是否把用户限制在家目录

		四	配置文件配置      修改配置文件后 需要重启服务

			/etc/vsftpd/vsftpd.conf

			1	主机相关配置
				listen_port=21			监听端口
				connect_from_port_20=YES	数据传输端口
				ftpd_banner=				欢迎信息


			2	匿名用户登录			在linux下识别为  ftp  用户

				anonymous_enable=YES			允许匿名用户登录


			3	本地用户
				local_enable=YES			允许系统用户登录
				write_enable=YES			允许上传
				local_umask=022			默认上传权限
				local_max_rate=300			上传限速

			4	限制用户访问目录
				chroot_local_user=YES		只有此句，所有用户限制在家目录下

				chroot_local_user=YES		如有三句话，只有文件chroot_list中的用户可以访问任何目录，其他用户限制在家目录
				chroot_list_enable=YES
				chroot_list_file=/etc/vsftpd/chroot_list			
                 useradd  zhangsan 
                 passwd   zhangsan 
           
            
	五	ftp客户端使用   selinux  firewall 关闭        
 
        重启服务  service vsftpd restart
     
1、使用命令登录
		ftp  ip
			get  文件名		下载
			put  文件名		上传		不能上传和下载目录  
             help
2、使用windows窗口
ftp://用户名@IP

3、使用第三方工具登录
       FileZilla   




ssh安全登录			22端口

	一	联机加密工具
		非对称钥匙对加密

		安装		默认安装		openssh

		启动		默认开机自启动		service  sshd  restart

		配置文件	/etc/ssh/sshd_config


	二	ssh远程安全联机		

		ssh   用户名@ip

	三	scp	网络复制，网络文件传输			

		1	下载

			scp   用户名@ip:路径   本地路径

			scp  root@192.168.140.93:/root/abc  /root

			scp  -r  root@192.168.140.93:/root/11  /root		下载目录

		2	上传
			scp  本地文件或目录  用户名@ip:路径

			scp  -r  /root/11  root@192.168.140.93:/root		上传目录




一、准备工作

1、安装编译工具gcc、gcc-c++
注意解决依赖关系，推荐使用yum安装，若不能联网可使用安装光盘做为yum源——
1）编辑yum配置文件：
# mount /dev/cdrom /mnt/cdrom
# vi /etc/yum.repos.d/CentOS-Media.repo 
[c6-media] 
name=CentOS-$releasever - Media
baseurl=file:///mnt/cdrom   * 修改为光盘挂载点
gpgcheck=0
enabled=1  * 改为1意为启用
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
2）剪切/etc/yum.repos.d/CentOS-Base.repo
# mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
3）依次安装gcc、gcc-c++
# yum -y install gcc
# yum -y install gcc-c++

2、关闭系统RPM安装包的Apache、MySQL的服务
关闭启动的服务httpd、mysqld
# service httpd stop
# service mysqld stop

确定rpm包安装的httpd和mysqld不能开机自启动
chkconfig  --level  2345  httpd（mysqld）  off
					

3、关闭SELinux，允许防火墙80端口访问
使用setup	
	关闭防火墙和SElinux

1）关闭SELinux
# vi /etc/selinux/config
SELINUX=disabled   * 若安装时没有禁用SELinux ，将enforcing改为disabled
修改后需重新启动Linux方可生效！
2）关闭防火墙Netfilter/iptables
因尚未做防火墙讲解，直接简单的关闭所有防火墙设置：
# iptables  -F     * 如果没有禁用防火墙，默认80端口禁止访问
iptables	 -Z		
iptables  -X

4、关闭不必要自启动服务
# ntsysv
以下列出服务可保持自启动，未列出的服务都可以关闭：
atd    
crond        # atd、crond计划任务
irqbalance
microcode_ctl   # 系统irq端口调用，系统服务
network    #网络设置
sendmail   #邮件
sshd      #远程管理
syslog    #系统日志

5、拷贝源码包，解包解压缩
 建议将LAMP环境安装源码包统一存放在一个目录下，如/lamp
 可编写个批量处理脚本，一次性把所有.tar.gz的安装包解包解压缩
 # vi tar.sh    
 	cd /lamp
/bin/ls *.tar.gz > ls.list
/bin/ls *.tgz >> ls.list
 	for TAR in `cat ls.list`
 do
		/bin/tar -zxf $TAR
 done
/bin/rm ls.list

6、查看确认磁盘空间未满
df -h
  * 若/分区已满，可以移动安装包到其他分区或删除其他无用文件


如何确定报错：
1）安装过程停止
2）停止后，一页界面中出现error或者warning

如何确定安装成功：
	进入安装目录，确认安装程序出现，就是成功

二、编译安装

 * 每个源码包配置编译安装完成后，确认安装目录下是否生成安装文件
	
 # 安装libxml2
Libxml2 是一个xml c语言版的解析器，本来是为Gnome项目开发的工具，是一个基于MIT License的免费开源软件。它除了支持c语言版以外，还支持c++、PHP、Pascal、Ruby、Tcl等语言的绑定，能在Windows、Linux、Solaris、MacOsX等平台上运行。功能还是相当强大的，相信满足一般用户需求没有任何问题。
libxml是一个用来解析XML文档的函数库。它用C语言写成, 并且能为多种语言所调用，例如C语言，C++，XSH。C#, Python，Kylix/Delphi，Ruby，和PHP等。Perl中也可以使用XML::LibXML模块。它最初是为GNOME开发的项目，但现在可以用在各种各样的方面。libXML 代码可移植性非常好，因为它基于标准的ANSI C库, 并采用MIT许可证。

#yum  install  -y  libxml2-devel	如果报错，安装此包后再尝试安装

yum -y install python-devel			必须安装

 cd /lamp/libxml2-2.9.1
 ./configure --prefix=/usr/local/libxml2/
 make 
 make install

 # 安装libmcrypt					
libmcrypt是加密算法扩展库。支持DES, 3DES, RIJNDAEL, Twofish, IDEA, GOST, CAST-256, ARCFOUR, SERPENT, SAFER+等算法。
 cd /lamp/libmcrypt-2.5.8
 ./configure --prefix=/usr/local/libmcrypt/
 make 
 make install
 * 需调用gcc-c++编译器，未安装会报错

# 安装libltdl，也在libmcrypt源码目录中，非新软件
 cd /lamp/libmcrypt-2.5.8/libltdl
 ./configure --enable-ltdl-install
 make
 make install


# 安装mhash	
Mhash是基于离散数学原理的不可逆向的php加密方式扩展库，其在默认情况下不开启。mhash的可以用于创建校验数值，消息摘要，消息认证码，以及无需原文的关键信息保存（如密码）等。
cd /lamp/mhash-0.9.9.9
./configure 
make
make install



# 安装mcrypt	
mcrypt 是 php 里面重要的加密支持扩展库。Mcrypt库支持20多种加密算法和8种加密模式
cd /lamp/mcrypt-2.6.8
LD_LIBRARY_PATH=/usr/local/libmcrypt/lib:/usr/local/lib  \
./configure --with-libmcrypt-prefix=/usr/local/libmcrypt
#以上为一条命令。LD_LIBRARY_PATH用于指定libmcrypt和mhash的库的位置。
--with-libmcrypt-prefix用于指定libmcrypt软件位置
make
make install
#如果mcrypt没有安装完成，这是php的模块，需要等php安装完成之后，再继续安装

 # 安装zlib			
zlib是提供数据压缩用的函式库，由Jean-loup Gailly与Mark Adler所开发，初版0.9版在1995年5月1日发表。zlib使用DEFLATE算法，最初是为libpng函式库所写的，后来普遍为许多软件所使用。此函式库为自由软件，使用zlib授权
 cd /lamp/zlib-1.2.3			
./configure
 make
 make install  >>  /root/zlib.log
 * zlib指定安装目录可能造成libpng安装失败，故不指定，为卸载方便，建议make install执行结果输出到安装日志文件，便于日后卸载

# 安装libpng	
libpng 软件包包含 libpng 库.这些库被其他程式用于解码png图片
 cd /lamp/libpng-1.2.31
 ./configure --prefix=/usr/local/libpng
 make
 make install

 # 安装jpeg6			
用于解码.jpg和.jpeg图片
mkdir /usr/local/jpeg6	
 mkdir /usr/local/jpeg6/bin
 mkdir /usr/local/jpeg6/lib
 mkdir /usr/local/jpeg6/include
 mkdir -p /usr/local/jpeg6/man/man1
#目录必须手工建立
 cd /lamp/jpeg-6b
 ./configure --prefix=/usr/local/jpeg6/ --enable-shared --enable-static
 make	
 make install
 * --enable-shared与--enable-static参数分别为建立共享库和静态库使用的libtool

 # 安装freetype			
FreeType库是一个完全免费(开源)的、高质量的且可移植的字体引擎，它提供统一的接口来访问多种字体格式文件，包括TrueType, OpenType, Type1, CID, CFF, Windows FON/FNT, X11 PCF等。支持单色位图、反走样位图的渲染。FreeType库是高度模块化的程序库，虽然它是使用ANSI C开发，但是采用面向对象的思想，因此，FreeType的用户可以灵活地对它进行裁剪。
 cd /lamp/freetype-2.3.5
./configure --prefix=/usr/local/freetype/
 make
 make install

# 安装Apache
configure: error: Bundled APR requested but not found at ./srclib/. Download and unpack the corresponding apr and apr-util packages to ./srclib/.
#如果报错，则：
tar  zxvf  apr-1.4.6.tar.gz
tar  zxvf  apr-util-1.4.1.tar.gz  （已经解压）
cp  -r  /lamp/apr-1.4.6  /lamp/httpd-2.4.7/srclib/apr
cp  -r  /lamp/apr-util-1.4.1  /lamp/httpd-2.4.7/srclib/apr-util
#解压apr和apr-util，复制并取消版本号

configure: error: pcre-config for libpcre not found. PCRE is required and available from
#如果报错，则：
tar zxvf pcre-8.34.tar.gz
cd /lamp/pcre-8.34  
./configure && make && make install

checking whether to enable mod_ssl... configure: error: mod_ssl has been requested but can not be built due to prerequisite failures
#如果报错，则：
yum install openssl-devel

安装apache
 cd /lamp/httpd-2.4.7
 ./configure --prefix=/usr/local/apache2/ --sysconfdir=/usr/local/apache2/etc/ --with-included-apr --enable-so --enable-deflate=shared --enable-expires=shared --enable-rewrite=shared
 make
 make install
  * 若前面配置zlib时没有指定安装目录，Apache配置时不要添加--with-z=/usr/local/zlib/参数

 启动Apache测试：
/usr/local/apache2/bin/apachectl start
ps  aux | grep httpd
netstat –tlun | grep :80
* 若启动时提示/usr/local/apache2/modules/mod_deflate.so无权限，可关闭SELinux或者执行命令chcon -t texrel_shlib_t /usr/local/apache2/modules/mod_deflate.so ，类似此类.so文件不能载入或没有权限的问题，都是SELinux问题，使用命令：“chcon -t texrel_shlib_t 文件名”即可解决，MySQL和Apache也可能有类似问题。
通过浏览器输入地址访问：http://Apache服务器地址，若显示“It works”即表明Apache正常工作

设置Apache系统引导时启动：
echo "/usr/local/apache2/bin/apachectl start" >> /etc/rc.d/rc.local

# 安装ncurses
Ncurses 提供字符终端处理库，包括面板和菜单。它提供了一套控制光标，建立窗口，改变前景背景颜色以及处理鼠标操作的函数。使用户在字符终端下编写应用程序时绕过了那些恼人的底层机制。简而言之，他是一个可以使应用程序直接控制终端屏幕显示的函数库。
1、二进制安装
yum -y install ncurses-devel
注：如果报错，包找不到，是*通配符没有识别，给文件名加双引号  “ncurses*”
2、源代码编译:
cd /lamp/ncurses-5.9
./configure --with-shared --without-debug --without-ada --enable-overwrite
make 
make install
* 若不安装ncurses编译MySQL时会报错
* --without-ada参数为设定不编译为ada绑定，因进入chroot环境不能使用ada ；--enable-overwrite参数为定义把头文件安装到/tools/include下而不是/tools/include/ncurses目录
*	--with-shared	生成共享库

#安装cmake和bison
mysql在5.5以后，不再使用./configure工具，进行编译安装。而使用cmake工具替代了./configure工具。cmake的具体用法参考文档cmake说明。
bison是一个自由软件，用于自动生成语法分析器程序，可用于所有常见的操作系统
yum -y install cmake
yum -y install bison

 # 安装MySQL				
 groupadd mysql
 useradd -g mysql mysql
* 添加用户组mysql ，将mysql用户默认组设置为mysql用户组

cd /lamp/mysql-5.5.23
cmake  -DCMAKE_INSTALL_PREFIX=/usr/local/mysql    -DMYSQL_UNIX_ADDR=/tmp/mysql.sock  -DEXTRA_CHARSETS=all   -DDEFAULT_CHARSET=utf8    -DDEFAULT_COLLATION=utf8_general_ci    -DWITH_MYISAM_STORAGE_ENGINE=1   -DWITH_INNOBASE_STORAGE_ENGINE=1    -DWITH_MEMORY_STORAGE_ENGINE=1  -DWITH_READLINE=1    -DENABLED_LOCAL_INFILE=1   -DMYSQL_USER=mysql  -DMYSQL_TCP_PORT=3306

	-DCMAKE_INSTALL_PREFIX=/usr/local/mysql		安装位置
	-DMYSQL_UNIX_ADDR=/tmp/mysql.sock			指定socket（套接字）文件位置
	-DEXTRA_CHARSETS=all						扩展字符支持
	-DDEFAULT_CHARSET=utf8    					默认字符集
	-DDEFAULT_COLLATION=utf8_general_ci    		默认字符校对
	-DWITH_MYISAM_STORAGE_ENGINE=1   			安装myisam存储引擎
	-DWITH_INNOBASE_STORAGE_ENGINE=1    		安装innodb存储引擎
	-DWITH_MEMORY_STORAGE_ENGINE=1  			安装memory存储引擎
	-DWITH_READLINE=1    						支持readline库
	-DENABLED_LOCAL_INFILE=1   					启用加载本地数据
	-DMYSQL_USER=mysql  						指定mysql运行用户
	-DMYSQL_TCP_PORT=3306						指定mysql端口


 make
 make install

make clean 
rm CMakeCache.txt
#如果报错，清除缓存，请使用以上命令

cd /usr/local/mysql/
chown -R mysql .
chgrp -R mysql .
#修改mysql目录权限
/usr/local/mysql/scripts/mysql_install_db --user=mysql
#创建数据库授权表，初始化数据库
chown -R root .
chown -R mysql data
#修改mysql目录权限

cp support-files/my-medium.cnf /etc/my.cnf
#复制mysql配置文件

二次授权
/usr/local/mysql/scripts/mysql_install_db --user=mysql

启动MySQL服务：
1.用原本源代码的方式去使用和启动mysql
/usr/local/mysql/bin/mysqld_safe --user=mysql &
2.重启以后还要生效:
vi /etc/rc.local
/usr/local/mysql/bin/mysqld_safe --user=mysql &
3.设定mysql密码
/usr/local/mysql/bin/mysqladmin -uroot password 123
	清空历史命令 	history  -c
* 给mysql用户root加密码123
*	注意密码不能写成 “123”	
 /usr/local/mysql/bin/mysql -u root -p 
mysql>show databases;
mysql>use test;
mysql>show tables;
mysql>\s			#查看字符集是否改为utf8
* 进入mysql以后用set来改密码
 mysql> exit
 * 登录MySQL客户端控制台设置指定root密码
 
 # 安装PHP					
编译前确保系统已经安装了libtool和libtool-ltdl软件包，安装：
yum -y install “libtool*”

cd /lamp/php-5.6.15
./configure --prefix=/usr/local/php/ --with-config-file-path=/usr/local/php/etc/ --with-apxs2=/usr/local/apache2/bin/apxs --with-mysql=/usr/local/mysql/ --with-libxml-dir=/usr/local/libxml2/ --with-jpeg-dir=/usr/local/jpeg6/ --with-png-dir=/usr/local/libpng/ --with-freetype-dir=/usr/local/freetype/  --with-gd  --with-mcrypt=/usr/local/libmcrypt/ --with-mysqli=/usr/local/mysql/bin/mysql_config --enable-soap --enable-mbstring=all --enable-sockets  --with-pdo-mysql=/usr/local/mysql  --without-pear


若前面配置zlib时没有指定安装目录，PHP配置时不要添加--with-zlib-dir=/usr/local/zlib/参数
选项：
	--with-config-file-path=/usr/local/php/etc/	指定配置文件目录
	--with-apxs2=/usr/local/apache2/bin/apxs	指定apache动态模块位置
	--with-mysql=/usr/local/mysql/			    指定mysql位置
	--with-libxml-dir=/usr/local/libxml2/		指定libxml位置
	--with-jpeg-dir=/usr/local/jpeg6/			指定jpeg位置
	--with-png-dir=/usr/local/libpng/			指定libpng位置
	--with-freetype-dir=/usr/local/freetype/	指定freetype位置
--with-mcrypt=/usr/local/libmcrypt/		    指定libmcrypt位置
	--with-mysqli=/usr/local/mysql/bin/mysql_config		指定mysqli位置
    --with-gd                                启用gd库
	--enable-soap							支持soap服务
	--enable-mbstring=all					支持多字节，字符串
	--enable-sockets						支持套接字
	--with-pdo-mysql=/usr/local/mysql		启用mysql的pdo模块支持
	--without-pear							不安装pear(安装pear需要连接互联网。												PEAR是PHP扩展与应用库)
make
 make install

生成php.ini
cp /lamp/php-5.6.15/php.ini-production /usr/local/php/etc/php.ini  
# mkdir /usr/local/php/etc/

测试Apache与PHP的连通性，看Apache是否能解析php文件
vi /usr/local/apache2/etc/httpd.conf
 AddType application/x-httpd-php .php .phtml 
 AddType application/x-httpd-php-source .phps
（注意大小写）
 * .phtml为将.phps做为PHP源文件进行语法高亮显示
 重启Apache服务：/usr/local/apache2/bin/apachectl stop
				  /usr/local/apache2/bin/apachectl start

* Apache无法启动，提示cannot restore segment prot after reloc: Permission denied错误，为SELinux问题，可关闭SELinux或者执行命令chcon -t texrel_shlib_t /usr/local/apache2/modules/libphp5.so   
测试：vi /usr/local/apache2/htdocs/test.php    
 	<?php
		phpinfo();
 ?>
通过浏览器输入地址访问：http://Apache服务器地址/test.php 
Rpm包安装的网页默认目录	/var/www/html/
* 有时第一次浏览器测试会失败，关闭浏览器重启再尝试即可，非编译错误


添加环境变量
whereis php
echo $PATH
#/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
export PATH=/usr/local/php/bin:$PATH
echo $PATH
#/usr/local/php/bin:/usr/local/php/bin:/usr/lib/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
#php -v
PHP 5.6.15 (cli) (built: Nov  3 2015 03:04:34) 
Copyright (c) 1997-2015 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2015 Zend Technologies
vim /etc/profile
在最后一行加上export PATH="/usr/local/php/bin:$PATH"
source /etc/profile

# 安装openssl	
OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。

yum -y install openssl-devel   必须安装
cd /lamp/php-5.6.15/ext/openssl
mv config0.m4 config.m4                否则报错：找不到config.m4
/usr/local/php/bin/phpize 
./configure --with-openssl --with-php-config=/usr/local/php/bin/php-config 
make
make install


# 编译安装memcache		
Memcache是一个高性能的分布式的内存对象缓存系统，通过在内存里维护一个统一的巨大的hash表，它能够用来存储各种格式的数据，包括图像、视频、文件以及数据库检索的结果等。简单的说就是将数据调用到内存中，然后从内存中读取，从而大大提高读取速度。

yum -y install zlib-devel  
cd memcache-3.0.8 
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install

#编译安装mcrypt
cd /lamp/php-5.6.15/ext/mcrypt/
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config --with-mcrypt=/usr/local/libmcrypt/

make
make install
#php安装完成后，通过这些命令安装mcrypt模块


修改/usr/local/php/etc/php.ini
extension_dir = "/usr/local/php/lib/php/extensions/no-debug-zts-20131226/"
#打开注释，并修改
extension="memcache.so";
extension="mcrypt.so"; 
extension="openssl.so";
#添加
#重启apache，在phpinfo中可以找到这三个模块


#安装memcache源代码
首先安装依赖包libevent
yum -y install “libevent*”		
#在CentOS 6.3第二张光盘中，请换盘
umount /mnt/cdrom 
#放入CentOS 6.3第二张光盘
#mount /dev/sr0 /mnt/cdrom

cd /lamp/memcached-1.4.17
./configure --prefix=/usr/local/memcache
make && make install

useradd memcache
#添加memcache用户，此用户不用登录，不设置密码
/usr/local/memcache/bin/memcached -umemcache &    
netstat -an | grep :11211
写入自启动：
vi /etc/rc.d/rc.local
/usr/local/memcache/bin/memcached -umemcache &

# 安装phpMyAdmin
cp -r phpMyAdmin-4.1.4-all-languages /usr/local/apache2/htdocs/phpmyadmin
cd /usr/local/apache2/htdocs/phpmyadmin
cp config.sample.inc.php config.inc.php
vi config.inc.php
$cfg['Servers'][$i]['auth_type'] = 'cookie';
$cfg['Servers'][$i]['auth_type'] = 'http';
* 设置auth_type为http ，即设置为HTTP身份认证模式
通过浏览器输入地址访问：http://Apache服务器地址/phpmyadmin/index.php
用户名为root ，密码为MySQL设置时指定的root密码123（lampbrother）



安装过程中大多错误其实为输入错误，可以通过history命令查看历史记录检查。

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123' WITH GRANT OPTION;
sql语句，不是linux命令

第八天 	Apache服务器

	一	简介

		1	www：world  wide  web	万维网

			http	协议：	超文本传输协议

			HTML语言：	超文本标识语言

		2	URL：统一资源定位		协议+域名：端口+网页文件名
					http://www.sina.com.cn:80/11/index.html
                     
		3	搭建www的服务器的方法
				windows  	IIS+asp+SQLserver
						Internet  Information  server
				Linux		apache+mysql+php

	二	安装
		1、lamp源码安装	
                        生产环境，技术要求高，复杂
		2、rpm包安装     yum安装
			httpd
			mysql
			mysql-server		
			php
			php-devel
			php-mysql

	三	相关文件

		apache配置文件
			源码包安装：/usr/lcoal/apache2/etc/httpd.conf
				    /usr/local/apache2/etc/extra/*.conf
				    
			rpm包安装：/etc/httpd/conf/httpd.conf

		默认网页保存位置：
			源码包：/usr/local/apache2/htdocs/
			rpm包安装：/var/www/html/

		日志保存位置
			源码包：/usr/local/apache2/logs/
			rpm包： /var/log/httpd/
            rpm包默认使用日志处理程序   /var下都会轮替   源码包才需要设置
日志处理：
1日志切割  apache自带日志里面自带日志切割
2日志轮替  linux自带日志管理logrorate.conf	
           加入/usr/local/apache2/logs/access_log{
                                 daily
                                 rotate  30
}
          logrotate -f /etc/logrotate.conf

             所有日志都要进行日志轮替
 

	四	配置文件

vi /root/.bashrc
   alias sta=’/usr/local/apache2/bin/apachectl start’
   alias sto=’/usr/local/apache2/bin/apachectl stop’

source /root/.bashrc
		
		注意：apache配置文件严格区分大小写

		1	针对主机环境的基本配置

		ServerRoot		apache主目录          
		Listen			监听端口              
		LoadModule		加载的相关模块
		
		User
		Group			用户和组
		ServerAdmin		管理员邮箱
		ServerName		服务器名（没有域名解析时，使用临时解析。不开启）
		ErrorLog "logs/error_log	错误日志
		CustomLog "logs/access_log" common		正确访问日志
		DirectoryIndex index.html index.php		默认网页文件名,优先级顺序
		Include  etc/extra/httpd-vhosts.conf		子配置文件中内容也会加载生效
		
		2	主页目录及权限  

			DocumentRoot "/usr/local/apache2//htdocs"          
				主页目录

			<Directory "/usr/local/apache2//htdocs">                 
				#Directory关键字定义目录权限

				Options Indexes FollowSymLinks                    
					#options 
						None：没有任何额外权限
						All： 所有权限
						Indexes：浏览权限（当此目录下没有默认网页文件时，显示目录内容）
						FollowSymLinks：准许软连接到其他目录
				AllowOverride None
					#定义是否允许目录下.htaccess文件中的权限生效
						None：.htaccess中权限不生效
						All：文件中所有权限都生效
						AuthConfig：文件中，只有网页认证的权限生效。

				Require all granted	访问控制列表   403错误   404错误
    				
#定义此目录的允许访问权限
例1：仅允许IP为192.168.1.1的主机访问
      Require all  denied 
      Require ip 192.168.1.1 

例子2.仅允许192.168.1.0/24网络的主机访问
      Require  all  denied 
      Require ip 192.168.1.0/24 

例子3.禁止192.168.1.2的主机访问,其他的都允许访问,
<RequireAll> 
      Require all  granted 
      Require not ip 192.168.1.2                        
</RequireAll> 

例子4.允许所有访问,
Require all  granted				

例子5.拒绝所有访问,
Require all  denied				


		3	目录别名  用途 扩展网站目录，增加服务器，使用二级域名，使用目录别名    
子配置文件名	etc/extra/httpd-autoindex.conf

Alias /icons/ "/usr/local/apache2//icons/"
    apache以为在这里		实际目录位置
	定义别名  /icons/----               
		http://192.168.1.253/icons/

<Directory "/usr/local/apache2//icons">
    Options Indexes MultiViews			
    AllowOverride None
    Require all granted                   	
</Directory>

		4	用户认证   
		限制特定目录，只有指定用户可以访问。

		1）	建立需要保护的目录

			

		使用别名，在系统位置建立目录，然后保护

				mkdir  -p  /share/soft

		2)修改配置文件，允许权限文件生效
		vi  /usr/local/apache2/etc/httpd.conf
Alias /soft/ "/share/soft/"

<Directory "/share/soft">
    Options Indexes 
    AllowOverride All			#开启权限认证文件.htaccess
    Require all granted 
</Directory>

		重启apache

		3）在指定目录建立权限文件
		cd  /share/soft

		vi  .htaccess			#不区分大小写
AuthName "50 docs"
	#提示信息
AuthType basic
	#加密类型
AuthUserFile /share/apache.passwd
	#密码文件，文件名自定义。
require valid-user
	#允许密码文件中所有用户访问

		4）建立密码文件，加入允许访问的用户。用户和系统用户无关
		/usr/local/apache2/bin/htpasswd  -c  /share/apache.passwd  test1
			-c  建立密码文件，只有添加第一个用户时，才能-c
		/usr/local/apache2/bin/htpasswd  -m  /share/apache.passwd  test2
			-m  再添加更多用户时

	5	虚拟主机

				
		1）分类
	      基于IP的虚拟主机:	一台服务器，多个IP，搭建多个网站
	  	基于端口的虚拟主机：	一台服务器，一个ip，搭建多个网站，每个网络使用不同端口访问
	    基于名字的虚拟主机：	一台服务器，一个ip，搭建多个网站，每个网站使用不同域名访问

		2）步骤：
			①	解析试验域名
				www.sina.com
				www.sohu.com
C:\WINDOWS\system32\drivers\etc\hosts		windows
/etc/hosts								Linux


			②	规划网站主目录
				/share/sina--------------www.sina.com
				/share/sohu ------------ www.sohu.com

			③ 	修改配置文件
				vi  /usr/local/apache2/etc/httpd.conf
					Include etc//extra/httpd-vhosts.conf
						#打开虚拟主机配置文件
				vi /usr/local/apache2/etc/extra/httpd-vhosts.conf

 

<Directory "/usr/local/apache2/htdocs/sina">
    Options Indexes
    AllowOverride None
Require all granted 
</Directory>

<Directory "/usr/local/apache2/htdocs/sohu">
    Options Indexes
    AllowOverride None
    Require all granted 
</Directory>

<VirtualHost 192.168.150.253>
	#注意，只能写ip
    ServerAdmin webmaster@sina.com
		#管理员邮箱
    DocumentRoot "/usr/local/apache2/htdocs/sina"
		#网站主目录
    ServerName www.sina.com
		#完整域名
    ErrorLog "logs/sina-error_log"
		#错误日志
    CustomLog "logs/sina-access_log" common
		#访问日志
</VirtualHost>

<VirtualHost 192.168.150.253>
    ServerAdmin webmaster@sohu.com
    DocumentRoot "/usr/local/apache2/htdocs/sohu"
    ServerName www.sohu.com
    ErrorLog "logs/sohu.com-error_log"
    CustomLog "logs/sohu.com-access_log" common
</VirtualHost>
				

	6	rewrite	重写功能   URL 
		在URL中输入一个地址，会自动跳转为另一个

		1）域名跳转	www.sina.com  ------>  www.sohu.com

			开启虚拟主机，并正常访问 

[root@localhost ~]# vi /usr/local/apache2/etc/httpd.conf
LoadModule rewrite_module modules/mod_rewrite.so
#打开重写模块，记得重启apache

			修改配置文件，使sina目录的.htaccess文件生效

[root@localhost etc]# vi extra/httpd-vhosts.conf

<Directory "/usr/local/apache2/htdocs/sina">
    Options Indexes FollowSymLinks
    AllowOverride All
Require all granted 
</Directory>

			vi  /usr/local/apache2/htdocs/sina/.htaccess
RewriteEngine on
	#开启rewrite功能
RewriteCond %{HTTP_HOST} www.sina.com
	把以www.sina.com	开头的内容赋值给HTTP_HOST变量
RewriteRule  .*   http://www.sohu.com
	.*  输入任何地址，都跳转到http://www.sohu.com

2)网页文件跳转 
vi  /usr/local/apache2/htdocs/sina/.htaccess
RewriteEngine on
RewriteRule index(\d+).html index.php?id=$1
	#	输入index(数值).html时，跳转到index.php文件，同时把数值当成变量传入index.php

  

	7	常用子配置文件

		httpd-autoindex.conf			apache系统别名

		httpd-default.conf			线程控制			*

		httpd-info.conf			   状态统计网页

		httpd-languages.conf			语言编码			*

		httpd-manual.conf			apache帮助文档

		httpd-mpm.conf			    最大连接数			*
			
		httpd-multilang-errordoc.conf		错页面			*

		httpd-ssl.conf			   ssl安全套接字访问

		httpd-userdir.conf			用户主目录配置

		httpd-vhosts.conf			虚拟主机
LNMP安装与配置

Nginx与apache、lighttp性能综合对比，如下图:


一.系统需求:
CentOS/RHEL/Fedora/Debian/Ubuntu系统
需要3GB以上硬盘剩余空间
MySQL 5.6及MariaDB 10必须1G以上内存。
Linux下区分大小写，输入命令时请注意！
确定yum源正常使用！
二.安装步骤:
1、下载并安装LNMP一键安装包：
#tar -zxvf lnmp1.2-full.tar.gz
#cd lnmp1.2-full
#./install.sh lnmp 
安装LNMP执行：wget -c http://soft.vpser.net/lnmp/lnmp1.2-full.tar.gz && tar zxf lnmp1.2-full.tar.gz && cd lnmp1.2-full && ./install.sh lnmp
如需要安装LNMPA或LAMP，将./install.sh 后面的参数替换为lnmpa或lamp即可。
按上述命令执行后，会出现如下提示：


需要设置MySQL的root密码（不输入直接回车将会设置为root），输入后回车进入下一步，如下图所示：

这里需要确认是否启用MySQL InnoDB，如果不确定是否启用可以输入 y ，输入 y 表示启用，输入 n 表示不启用。默认为y 启用，输入后回车进入下一步，选择MySQL版本：

输入MySQL或MariaDB版本的序号，回车进入下一步，选择PHP版本：

输入PHP版本的序号，回车进入下一步，选择是否安装内存优化：

可以选择不安装、Jemalloc或TCmalloc，输入对应序号回车。
如果是LNMPA或LAMP的话还需要设置管理员邮箱

再选择Apache版本

提示"Press any key to install...or Press Ctrl+c to cancel"后，按回车键确认开始安装。
LNMP脚本就会自动安装编译Nginx、MySQL、PHP、phpMyAdmin、Zend Optimizer这几个软件。
3、安装完成
如果显示Nginx: OK，MySQL: OK，PHP: OK

并且Nginx、MySQL、PHP都是running，80和3306端口都存在，并Install lnmp V1.2 completed! enjoy it.的话，说明已经安装成功。

4、安装失败

如果出现类似上图的提示，则表明安装失败，说明没有安装成功！！

二.LNMP相关软件安装目录
Nginx 目录: /usr/local/nginx/
MySQL 目录 : /usr/local/mysql/
MySQL数据库所在目录：/usr/local/mysql/var/
PHP目录 : /usr/local/php/
PHPMyAdmin目录 : /home/wwwroot/default/phpmyadmin/ 
默认网站目录 : /home/wwwroot/default/
Nginx日志目录：/home/wwwlogs/

三.LNMP相关配置文件位置
Nginx主配置文件：/usr/local/nginx/conf/nginx.conf
MySQL配置文件：/etc/my.cnf
PHP配置文件：/usr/local/php/etc/php.ini
php-fpm配置文件：/usr/local/php/etc/php-fpm.conf

四.LNMP状态管理命令
LNMP 状态管理: lnmp {start|stop|reload|restart|kill|status}
LNMP 各个程序状态管理: lnmp {nginx|mysql|mariadb|php-fpm|pureftpd} {start|stop|reload|restart|kill|status}
五.配置文件
#vi /usr/local/nginx/conf/nginx.conf
user  www www;

worker_processes auto;
#启动进程

error_log  /home/wwwlogs/nginx_error.log  crit;
#错误日志

pid        /usr/local/nginx/logs/nginx.pid;
#主进程PID保存文件

#Specifies the value for maximum file descriptors that can be opened by this process. 
worker_rlimit_nofile 51200;
#文件描述符数量

events 
	{
  		use epoll;
		#网络I/O模型，建议linux使用epoll，FreeBSD使用kqueue
		#epoll是多路复用IO(I/O Multiplexing)中的一种方式,但是仅用于linux2.6以				上内	核,可以大大提高nginx的性能
  		worker_connections 51200;
#单个工作进程最大允许连接数
multi_accept on;
    
	}

http 
#整体环境配置
	{
  			include       mime.types;
  			default_type  application/octet-stream;
				#设定mime类型,文件传送类型由mime.type文件定义

                server_names_hash_bucket_size 128;		#保存服务器名字的hash表大小
                client_header_buffer_size 32k;			#客户端请求头部缓冲区大小
                large_client_header_buffers 4 32k;			#最大客户端头缓冲大小
                client_max_body_size 50m;				#客户端最大上传文件大小（M）

                sendfile on;
				#sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文					件，对于普通应用，必须设为 on。如果用来进行下载等应用磁盘IO重负载应用，可				设置为off，以平衡磁盘与网络I/O处理速度，降低系统的uptime.
				#高效文件传输
                tcp_nopush     on;
				#这个是默认的，结果就是数据包不会马上传送出去，等到数据包最大时，一次性的					传输出去，这样有助于解决网络堵塞。（只在sendfile on时有效）


                keepalive_timeout 60;
				#连接超时时间

                tcp_nodelay on;
				#禁用nagle算法，也即不缓存数据。有效解决网络阻塞

                fastcgi_connect_timeout 300;
                fastcgi_send_timeout 300;
                fastcgi_read_timeout 300;
                fastcgi_buffer_size 64k;
                fastcgi_buffers 4 64k;
                fastcgi_busy_buffers_size 128k;
                fastcgi_temp_file_write_size 256k;
				#fastcgi设置

       			gzip on;
        		gzip_min_length  1k;
       			gzip_buffers     4 16k;
        		gzip_http_version 1.1;
        		gzip_comp_level 2;
        		gzip_types     text/plain application/javascript 		application/x-javascript text/javascript text/css application/xml 	application/xml+rss;
        		gzip_vary on;
        		gzip_proxied   expired no-cache no-store private auth;
        		gzip_disable   "MSIE [1-6]\.";

        #limit_conn_zone $binary_remote_addr zone=perip:10m;
        ##If enable limit_conn_zone,add "limit_conn perip 10;" to server section.
                server_tokens off;
				#隐藏nginx版本号（curl -I 192.168.4.154可以查看，更加安全）

               	#log format
        		log_format  access  '$remote_addr - $remote_user [$time_local] "$request" '
             '$status $body_bytes_sent "$http_referer" '
             '"$http_user_agent" $http_x_forwarded_for';
				#定义日志格式

server
        {
                listen 80 default_server;
        		#listen [::]:80 default_server ipv6only=on;
				#监听80端口
                server_name www.lnmp.org;
				#服务器名
                index index.html index.htm index.php;
				#默认网页文件
                root  /home/wwwroot/default;
				#网页主目录

#error_page   404   /404.html;
include enable-php.conf;
                
location /nginx_status
        {
            stub_status on;
            access_log   off;
        }
#开启status状态监测
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires      30d;
        }
#静态文件处理，保存期30天
location ~ .*\.(js|css)?$
        {
            expires      12h;
        }
#js和css文件处理，保存期12小时
location ~ /\.
        {
            deny all;
        }

 access_log  /home/wwwlogs/access.log  access;
#正确访问日志
 }
include vhost/*.conf;
#vhost/下子配置文件生效

}





检查nginx配置文件语句错误
/usr/local/nginx/sbin/nginx -t

平滑重启nginx进程
1)pkill -HUP nginx
2)kill -HUP `pgrep -uroot nginx`
   Pgrep  -uroot  nginx  取出nginx主进程PID
3)/usr/local/nginx/sbin/nginx -s reload


六.配置实验
1.nginx虚拟主机

sina和sohu域名事先解析

Vi /usr/local/nginx/conf/nginx.conf
==www.sina.com公司网站

server
    {
        listen 80 ;
        #listen [::]:80 default_server ipv6only=on;
        server_name www.sina.com;
        index index.html index.htm index.php;
        root  /home/wwwroot/sina;
        #error_page   404   /404.html;
        include enable-php.conf;
    }
==www.sohu.com公司网站
server
    {
        listen 80 ;
        #listen [::]:80 default_server ipv6only=on;
        server_name www.sohu.com;
        index index.html index.htm index.php;
        root  /home/wwwroot/sohu;
        #error_page   404   /404.html;
        include enable-php.conf;
    }

重启nginx
最后在客户端测试虚拟主机www.baidu.com和www.sina.com两家公司网站

2.列表页显示
server
        {
                listen       80;
                server_name www.sina.com;
                index index.html index.htm index.php;
                root  /home/wwwroot/sina;
                autoindex on;

3.nginx状态监控
location /nginx_status{
        stub_status on;
        access_log  off;
        }
#客户端访问网址:http://IP/nginx_status

4.rewrite正则过滤
location ~ \.php$ {
        proxy_pass   http://127.0.0.1;
        }
Rewrite指令最后一项参数为flag标记，支持的flag标记如下:
Last
	停止执行当前这一轮的ngx_http_rewrite_module指令集，然后查找匹配改变后URI的新location；
Break
	停止执行当前这一轮的ngx_http_rewrite_module指令集；
Redirect 
	在replacement字符串未以“http://”或“https://”开头时，使用返回状态码为302的临时重定向；
Permanent
	返回状态码为301的永久重定向。

Last和break用来实现uri重写，浏览器地址栏的url地址不变，但在服务器访问的路径发生了变化，redirect和permanent用来实现url跳转，浏览器地址栏会显示跳转后的url地址，使用alias指令时必须使用last标记，使用proxy_pass指令时要使用break标记，last标记在本条rewrite规则执行完毕后，会对其所在的server{}标签重新发起请求，而break标记则在本条规则匹配完成后，终止匹配，不再匹配后面的规则.


例1：域名跳转
输入www.sina.com，跳转到www.sohu.com
server
        {
                listen       80;
                server_name www.sina.com;
                index index.html index.htm index.php;
                root  /home/wwwroot/sina;

                if ($http_host = www.sina.com) {
                        rewrite  (.*)  http://www.sohu.com  permanent;
                }
		}

server
        {
                listen       80;
                server_name www.sohu.com;
                index index.html index.htm index.php;
                root  /home/wwwroot/sohu;
        }

例2：文件跳转
server
        {
                listen       80;
                server_name www.sina.com;
                index index.html index.htm index.php;
                root  /home/wwwroot/sina;
                rewrite index(\d+).html  /index.php?id=$1 last;

		}


5. 代理负载均衡技术（反向代理）
http
        
upstream myweb1 {
	#定义地址池
        server 192.168.242.100:80;
        server 192.168.242.111:80;
}
    server {
        listen       80;
        server_name  www.sohu.com;
		#使用www.sohu.com访问
location / {
proxy_pass http://myweb1;
		#使用地址池
proxy_next_upstream http_500 http_502 http_503 error timeout invalid_header;
	#定义故障转移。后端服务器节点返回500、502、503、504和超时等错误时，自动把请求转发到另一台服务器，转移故障。
proxy_set_header Host $host;
		#利用HOST变量向后端服务器传递需要解析的客户端访问的域名（传递域名）
proxy_set_header X-Forwarded-For $remote_addr;
		#$remote_addr 把客户端真实IP赋予X-Forwarded-For。后端服务器才能获取真实的客户端IP。以便记录日志，要不日志中记录的访问信息都是负载服务器，而不是客户端（传递IP）
}
}


# OOP #
OOP
一，面向对象
	1. 面向对象的介绍
		1.1 面向对象的两个方向：1、思想（重要）    2、语法

		1.2 对象: 一切皆对象

		1.3 面向对象：按照对象的思想去编程

		1.4 面向过程: ①强调的是事件 ②注重的是步骤 ③函数 ④通过调用来实现步骤

		1.5 面向对象的优点： 重用性   灵活性   可扩展性 
	    1.6 面向对象编程的特点： 封装 继承 多态
		1.7 PHP中的面向对象
			面向对象不能提高程序的性能（代码执行效率）
			提高开发的效率（一个项目的开发使用时间）

			面向方向
		php5	php4	php3



	2. 类和对象的关系
		1.1 类和对象的关系
			类是对象的抽象，对象是类的实例
		
		1.2 对象
			人这个对象
			1,	头，手，脚（属性）
			2,	思考，打豆豆，走（方法）
			3,	标示
		
		1.3 类
			具有相同特性和行为的对象,的抽象就是类
			人这个类
				头，手，脚（属性）
				思考，打豆豆，走（方法）

			
二，抽象一个类
		1, 类的定义
			[修饰符] class 类名
        	{
		   		【成员属性】定义变量   

		   		【成员方法】定义函数
			}

			[修饰符] class 类名 [extends 父类] [implements 接口1[,接口2...]]
			{
		   		【成员属性】定义变量   

		   		【成员方法】定义函数
			}
			
		2. 成员属性(变量)
			修饰符 属性名;
			修饰符 属性名 = 值;

			修饰符：public private protected   var(过时，在php4的时候可用)  static(静态)
			
		3. 成员方法（函数）
			修饰符 function 方法名([参数，参数]){
				方法体
				[return]
			}
			修饰符：public private protected static final(定义的类和方法不能被继承) abstract(抽象)
			
三， 对象的操作
		1. 实例化一个对象
			new 类名([参数]);
			
		2. 对象成员的访问
			运算符：->
			
		3. 对象在内存中的存贮(了解)
			栈：先进后出。先来的反而最后出，简单的数据
			堆：用来存放复杂的数据：资源，对象，数组
			静态段：放常量和静态属性
			代码段：放的函数和方法

		4. $this 
			$this：代表你使用的对象
			可以在类里面访问对象的属性和方法
			$this->param;
			$this->funName();

		5. 构造方法和析构方法
			构造方法:
				定义：当实例化对象的时候，自动触发构造方法
				格式：public function __construct(){
					
				}
				作用：
					1.打开资源
					2.给属性赋初始值
				注意：
					构造方法是用来构造对象的（不对的）
					在实例化对象的时候自动执行的方法
					在php4中，如果有方法名和类名相同，这个方法也是构造方法

			析构方法：
				定义：对象销毁的时候自动触发
				格式：public function __destruct(){
					
				}
				作用：
					1.关闭资源
				注意：
					析构方法是用来销毁对象的（不对）



第二天
=====================================================	
四 封装性
		1.封装性 
			概念： 封装性是面向对象编程的三大的特性之一 ，给成员添加除了public之外修饰符.
		2. 设置私有成员
			2.1 封装成员方法
				1 提高代码重用性
				2 把部分功能封装小的方法,这些不让其他人调用,只让他自己的原来大的方法调用.
				注意:如果把构造方法封装了之后,导致new的时候无法调用
			2.2 封装成员属性
				把成员属性设置为非公有
				作用:实现访问控制
		3.访问私有成员
			3.1 通过共有的方法访问私有成员
				$this->封装之后的属性或者方法
			3.2 通过魔术方法访问私有成员 
				作用:实现访问控制
				__get()
				public function __get($param){
					return $this->$param;
				}
				__set()
				public function __set($param,$value){
					return $this->$param = $value;
				}
				__isset()
				public function __isset($param){
					return $this->$param;
				}
				__unset()
				public function __unset($param){
					unset($this->$param);
				}
				注意:这些魔术方法必须是公有的

第三天
==============================================
五， 继承
	1 类继承的应用
		继承：子类继承父类，具有父类的属性和方法
		父类依然有属性和方法
		
		格式：
			class 类名 extends 类名{
				//添加新的属性和方法
			}
	2 访问控制
		 public     private     protected
	子类类内  yes	      NO	  Yes
	类外	  Yes         No          No
	自己类内  Yes         yes         yes

	特点：
		1、父类有Public属性 子类覆盖设置成public 不可以private protected
		2、如果子类中的属性与父类相同，子类的属性会覆盖父类的属性（除了私有的）
		3、子类的方法与父类方法名相同，子类会覆盖父类的方法

	3.子类中重写父类的方法
		子类中方法名与父类方法名相同，会重写方法
		在子类中可以调用父类中的方法（把父类方法执行一遍）
			parent::方法名([参数]);
		
			
六， 常见的关键字
	1. final 关键字
		修饰 类 和 方法  
		final修饰类 该类不能被继承
		final修饰方法   该方法不能被重写
		
		作用： 提高安全性，防止类被继承之后重写里面的方法
				没有必要被继承
		
	2. static
		静态： 修饰属性和方法
		静态属性的访问：  类外部  类名::$属性名    类的内部： 
self::$属性名
		静态方法的访问：  类外部  ①类名::方法名() ② 对象名->方法
名()   类内部：self::方法名()   $this->方法名()
		静态方法中不能出现非静态的内容，（$this）
		静态方法没有static关键字，但是里面没有非静态内容，默认也是
静态方法（会有语法更正 ）
		
	
	3. 单态 
		设计模式
		一个类只能有一个实例  
		① 通过私有化构造方法 使类不能被实例
		② 添加静态方法，留后门
		③ 在静态方法中实例化该类
		④ 定义静态属性存放实例化的对象， 每次调用静态方法的时候，进行判断，如果类已经被实例化，就返回以前的，否则重新实例化
		
	4. const
		定义常量  在类里面定义常量
		class 类名{
			const 常量 = 值;
		}
		
		常量的访问： 类外部： 类名::常量    类的内部:  self::常量
		作用： 给方法定义参数 
	
	5. instanceof
		运算符  
		判读一个对象是否是指定类的实例，或者是该类子类的实例
		对象 instanceof 类名
	
				
	6. 克隆对象 
		关键字 clone
		魔术方法 __clone();在克隆的时候自动触发
		注意：
			① 对象时引用赋值   $a=10  $b=&$a  
			② 克隆时候，里面的属性也是对象，仅仅会把对象地址复制
		    	③ 魔术方法__clone 可以把类型是对象的属性也克隆一个
			
			
	7. 类中通用的方法 __toString()
			当使用 echo 或 print 输出对象的时候 会自动触发 
			__toString() 必须返回字符串
			该方法通常用来返回对象的基本信息
		
	8. __call() 方法的应用
		当调用对象中一个不存在的方法会自动调用
		传入两个参数 第一个参数是调用到方法名，第二个参数数组，调用时给的参数
		
	9. 自动加载类
		__autoload(){}
		当实例化一个不存在的类会自动调用autoload 并且把类名作为第一个参数传入
		可以再方法中包含类文件
		不是魔术方法，写在类的外面
		
		
	10. 对象串行化
		把对象的当前状态以字符串的形式保存下来 就是串行化
		string = serialize(Object)
		Object = unserialize(string)
		
		__sleep() 串行化的是自动调用
		__wakeup() 反串行化自动调用 
		
	11. 类型约束 
		int fun(string a,int b)  强类型定义函数
		php的类型智能约束 数组和对象
		
		类型约束 约束函数参数只能是数组
				约束参数只能是某个类或子类的实例
第五天======================================================				
				
1.抽象类和接口
	
	1.1. 抽象类和抽象方法
		抽象方法： 没有方法体的方法就是抽象方法，必须使用abstract来修饰
		抽象类：  一个类中含有抽象方法，就是抽象类,使用关键字abstract
			abstract class 类名{
				abstract public function 方法();
			}
		特点: 
			①类定义成抽象类，里面可以没有抽象方法，但反之则不行
			②抽象类不能被实例化，要想使用抽象类，必须通过子类继承，必须重写所有的抽象方法，没有重写所有，子类仍然是抽象类
			③ 抽象方法不能设置成私有，私有会导致在子类中不能重写
			
		作用：
			指定行为规范       
			
	1.2. 接口 interface
		接口： 如果一个类所有的方法都是抽象方法，该类可以定义为接口
		     interface 接口名{
				//抽象方法
				public function fun();
			 }
		特点： 
			① 接口不能定义属性
			② 接口中不能有任何非抽象的方法
			③ 接口中可以定义常量
			④ 接口同样不能被实例化，要想使用接口，必须通过子类实现接口，重写所有的方法，如果没有重写所有的，该类还是抽象类
		
		
		接口继承：
			implements关键字   class 类名 implements 接口名{}
			支持多继承
			接口之间继承使用extends
			
			
		作用：
			同抽象类
			
	1.3. 抽象类和接口不同
		1. 抽象类可以有成员属性，接口只能有常量
		2. 抽象类可以包含非抽象方法，接口只有抽象方法
		3. 抽象类是单继承，接口多"继承"
		
		
多态的应用
	概念：父类的属性和方法被子类继承之后可以表现出不同的数据类型和不同的行为
1.2 PHP的异常处理
	1.1 异常处理 
		正常执行过程中的异常  
		格式
		try{
			//如果出现异常
			throw new Exception("message")
		}catch(Exception $e){
			echo $e->getMessage();
		}
		
		注意：
			① 抛出异常之后，try里面的代码会停止，try外面的照常执行
			② 如果一个函数中有异常处理，（函数中出现异常会抛出），调用的时候在try中
			③ 如果一类构造方法中会抛出异常，该类是具有异常处理的类，实例化该类的时候写到try中
		
	1.2 系统自带的异常处理类
		class Exception
		{
			protected $message = 'Unknown exception';   // 异常信息
			protected $code = 0;                        // 用户自定义异常代码
			protected $file;                            // 发生异常的文件名
			protected $line;                            // 发生异常的代码行号

			function __construct($message = null, $code = 0);

			final function getMessage();                // 返回异常信息
			final function getCode();                   // 返回异常代码
			final function getFile();                   // 返回发生异常的文件名
			final function getLine();                   // 返回发生异常的代码行号
			final function getTrace();                  // backtrace() 数组
			final function getTraceAsString();          // 已格成化成字符串的 getTrace() 信息

			/* 可重载的方法 */
			function __toString();                       // 可输出的字符串
		}
	1.3 自定义异常处理类
		继承系统的异常处理类
		系统异常处理类的方法不了能重写， 可以添加方法
		
	1.4 处理多个异常
		格式
			try{
				//抛出不同类型的异常
			}catch(){
			
			}catch(){
			
			}catch(){
			
			}
			
		注意： 系统的异常类一定最后一个catch
		
		
	格式 ： try
	系统的异常处理类的方法
	自定义异常处理类
	处理多个异常
		

2. PHP类与对象的相关函数
	class_exists(clasName) 
		返回布尔 判断类是否存在 类名以字符串的形式给出
		该方法会自动触发__autoload()  但是 class_exists()第二个参数设置为false不会触发__autoload()
	
	get_class_methods(clasName/ob) 
		返回数组，得到类中所有的公有方法（），参数可以对象也可以是类 
	
	get_class_vars(clasName) 
		返回数组，得到类中的所有公有属性，参数是类
	
	get_object_vars(ob) 
		返回数组，得到对象中所有的公有属性，参数是对象
	
	get_class(ob) 
		返回类名  根据对象得到类
		
	get_parent_class(ob/className)
		返回类名 根据对象或类名得到父类，如果没有父类得到空字符串
	
	method_exists(ob/className,funName) 
		返回布尔，判断方法是否存在与类或对象中 不分共有私有
		class_exists()
		method_exists()
		file_exists()
		
	property_exists(ob/className,varName) 
		返回布尔值  判断 属性是否在 对象或 类中 不分共有私有
	
	
	is_a(ob,className) 同instanceof
		判断对象时指定类或该类子类的实例
	
	
	get_declared_classes() 
		返回数组， 得到所有已经定义的类 包括系统类和自定义类
	
		

	
	




