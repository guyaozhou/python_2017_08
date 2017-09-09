三剑客
grep、sed、awk
一、grep
grep：
	（global search regular expression(RE) and print out the line，全面搜索正则表达式并把行打印出来）是一种强大的文本搜索工具。

作用：
	文本搜索工具，根据用户指定的“模式”对目标文本逐行进行匹配检查，它能使用正则表达式搜索文本，并把匹配的行打印出来。

模式：
	由正则表达式字符及文本字符所编写的过滤条件
	
语法：
	grep [OPTIONS] PATTERN [FILE...]
	
	model：
		
		root@Eric(Eric) ~]#grep  Eric /etc/passwd
		Eric:x:1001:1001::/home/Eric:/bin/bash
		
		
	grep的基本正则表达式命令选项：
		
		--color=auto: 对匹配到的文本着色显示
			model：
				
				[root@centos6(Eric) ~]#grep --color=auto root /etc/passwd   
				root:x:0:0:root:/root:/bin/bash
				operator:x:11:0:operator:/root:/sbin/nologin
				注：因为编辑器的原因，所以颜色没有高亮显示
				
		-v: 显示不被pattern匹配到的行
			
			[root@centos6(Eric) Eric]#grep -v root passwd 
			bin:x:1:1:bin:/bin:/sbin/nologin
			daemon:x:2:2:daemon:/sbin:/sbin/nologin
			adm:x:3:4:adm:/var/adm:/sbin/nologin
			lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
			sync:x:5:0:sync:/sbin:/bin/sync
			shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
			halt:x:7:0:halt:/sbin:/sbin/halt
			mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
			uucp:x:10:14:uucp:/var/spool/uucp:/sbin/nologin
			games:x:12:100:games:/usr/games:/sbin/nologin
			gopher:x:13:30:gopher:/var/gopher:/sbin/nologin
			ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
			nobody:x:99:99:Nobody:/:/sbin/nologin
			dbus:x:81:81:System message bus:/:/sbin/nologin
			usbmuxd:x:113:113:usbmuxd user:/:/sbin/nologin
			rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
			rtkit:x:499:499:RealtimeKit:/proc:/sbin/nologin
			avahi-autoipd:x:170:170:Avahi IPv4LL Stack:/var/lib/avahi-autoipd:/sbin/nologin
			vcsa:x:69:69:virtual console memory owner:/dev:/sbin/nologin
			abrt:x:173:173::/etc/abrt:/sbin/nologin
			rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
			nfsnobody:x:65534:65534:Anonymous NFS User:/var/lib/nfs:/sbin/nologin
			haldaemon:x:68:68:HAL daemon:/:/sbin/nologin
			ntp:x:38:38::/etc/ntp:/sbin/nologin
			apache:x:48:48:Apache:/var/www:/sbin/nologin
			saslauth:x:498:76:Saslauthd user:/var/empty/saslauth:/sbin/nologin
			postfix:x:89:89::/var/spool/postfix:/sbin/nologin
			gdm:x:42:42::/var/lib/gdm:/sbin/nologin
			pulse:x:497:496:PulseAudio System Daemon:/var/run/pulse:/sbin/nologin
			sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
			tcpdump:x:72:72::/:/sbin/nologin
			Eric:x:500:500::/home/Eric:/bin/bash
			
			注：为了显示匹配的效果，所以，匹配这个文本有点长，如果自己测试，可以新建或者拷贝一个文件，删除一些内容。
		
		-i: 忽略字符大小写
			
			[root@centos6(Eric) Eric]#grep eric -i passwd 
			Eric:x:500:500::/home/Eric:/bin/bash
		
		-n：显示匹配的行号
			
			[root@centos6(Eric) Eric]#grep Eric -n passwd   
			34:Eric:x:500:500::/home/Eric:/bin/bash
			
			注：前面的34就是显示的行号。
		
		-c: 统计匹配的行数
			
			[root@centos6(Eric) Eric]#grep root -c passwd 
			2
			
			下面结果证明显示的匹配到行数是正确
			
			[root@centos6(Eric) Eric]#grep root  passwd   
			root:x:0:0:root:/root:/bin/bash
			operator:x:11:0:operator:/root:/sbin/nologin
		
		-o：仅显示匹配到的字符串
			
			[root@centos6(Eric) Eric]#grep root  -o passwd 
			root
			root
			root
			root
		
		-q：静默模式，不输出任何信息
			
			[root@centos6(Eric) Eric]#grep root  -q passwd 
		
		-A #：after，后#行
			
			[root@centos6(Eric) Eric]#grep root  -A 3 passwd 
			root:x:0:0:root:/root:/bin/bash
			bin:x:1:1:bin:/bin:/sbin/nologin
			daemon:x:2:2:daemon:/sbin:/sbin/nologin
			adm:x:3:4:adm:/var/adm:/sbin/nologin
			--
			operator:x:11:0:operator:/root:/sbin/nologin
			games:x:12:100:games:/usr/games:/sbin/nologin
			gopher:x:13:30:gopher:/var/gopher:/sbin/nologin
			ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
		
		-B #：before，前#行
			
			[root@centos6(Eric) Eric]#grep Eric  -B 3 passwd  
			pulse:x:497:496:PulseAudio System Daemon:/var/run/pulse:/sbin/nologin
			sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
			tcpdump:x:72:72::/:/sbin/nologin
			Eric:x:500:500::/home/Eric:/bin/bash
		
		-C #：context，前后各#行
			
			[root@centos6(Eric) Eric]#grep ftp  -C 3 passwd    
			operator:x:11:0:operator:/root:/sbin/nologin
			games:x:12:100:games:/usr/games:/sbin/nologin
			gopher:x:13:30:gopher:/var/gopher:/sbin/nologin
			ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
			nobody:x:99:99:Nobody:/:/sbin/nologin
			dbus:x:81:81:System message bus:/:/sbin/nologin
			usbmuxd:x:113:113:usbmuxd user:/:/sbin/nologin
		
		-e：实现多个选项间的逻辑or关系
			grep -e 'Eric' -e 'root' file
			
			[root@centos6(Eric) Eric]#grep -e Eric -e root passwd 
			root:x:0:0:root:/root:/bin/bash
			operator:x:11:0:operator:/root:/sbin/nologin
			Eric:x:500:500::/home/Eric:/bin/bash
			
		-w：匹配整个单词
		
		[root@centos6(Eric) Eric]#grep /var/ftp -w passwd     
		ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
		
		-E：使用ERE
		-F：相当于fgrep，不支持正则表达式
		
	基本正则表达式
		
		REGEXP：由一类特殊字符及文本字符所编写的模式，其中有些自负（元字符）不表示字符字面意义，而表示控制或通配的功能

		程序支持：grep,sed,awk,vim,less,nginx,varnish等

		分两类：
			基本正则表达式：BRE
			扩展正则表达式：ERE
				grep - E，egrep
		正则表达式引擎：
			采用不同算法，检查处理正则表达式的软件模块PCRE(Perl Compatible Regular Expressions)

			元字符分类：
				字符匹配、匹配次数、位置锚定、分组

			man 7 regex

			字符匹配：
				.： 匹配任意单个字符

					[root@centos6(Eric) ~]#ls in*
					install.log  install.log.syslog

				[]：匹配指定范围内的任意单个字符

					[root@centos6(Eric) ~]#ls in[a-z]tall.log
					install.log

				[^]：匹配指定范围外的任意单个字符

					[root@centos6(Eric) ~]#touch 968a.log
					[root@centos6(Eric) ~]#ls 968[^0-9].log
					968a.log

				[:alnum:]：字母和数字

					[root@centos6(Eric) ~]#ls [[:alnum:]]*.log
					968a.log  968.log  install.log

				[:alpha:]：代表任何英文大小写字符，亦即A-Z,a-z

					[root@centos6(Eric) ~]#ls [[:alpha:]]*.log
					install.log

				[:lower:]：小写字母

					[root@centos6(Eric) ~]#touch Eric.log
					[root@centos6(Eric) ~]#ls [[:lower:]]*.log
					install.log

				[:upper:]：大写字母
					
					[root@centos6(Eric) ~]#ls [[:upper:]]*.log
					Eric.log

				[:blank:]：空白字符
				[:space:]：水平和垂直的空白字符（比[:blank:]包含的范围广）
				[:cntrl:]：不可打印的控制字符（退格、删除、警铃...）
				[:digit:]：十进制数字
				[:xdigit:]：十六进制数字
				[:graph]：可打印的非空白字符
				[:print:]：可打印字符
				[:punct:]：标点符号

	正则表达式
		匹配次数：用在用指定次数的字符后面，用于指定前面的字符要出现的次数

			*：匹配前面的字符任意次，包括0次
				贪婪模式：尽可能长的匹配
			.*：任意长度的任意字符
			\?：匹配其前面的字符0次或1次
			\+：匹配其前面的字符至少1次
			\{n\}：匹配前面的字符b次
			\{m,n\}：匹配前面的字符至少m次，至多n次
			\{,n\}：匹配前面的字符至多n次
			\{n,\}匹配前面的字符至少n次
		
		位置锚定：定位出现的位置
			^：行首锚定，用于模式的最左侧
			$：行尾锚定，用于模式的最右侧
			^PATTERN$：用于模式匹配整行
				^$：空行
				^[[:space:]]*$：空白行
			\<或\b：词首锚定，用于单词模式的左侧
			\>或\b：词尾锚定，用于单词模式的右侧
			\<PATTERN\>：匹配整个单词

		分组：\(\)：将一个或多个字符捆绑在一起，当作一个整体进行处理，如：\(root\)\+

		分组括号中的模式匹配到的内容会被正则表达式引擎记录于内部的变量中，这些变量的命名方式为：\1,\2,\3,...

		\1：表示从左侧起的第一个左括号以及与之匹配右括号之间的模式所匹配到的字符
			示例：
				\(string1\+\(string2\)*\)
				\1：string1\+\(string2\)*
				\2：string2
		后向引用：引用前面的分组括号中的模式所匹配字符，而非模式本身

		或者：\|
			示例：a\|b：a或b C\|cat：C或cat  \(C\|c)at：Cat或cat

		model：
			1、显示/proc/meminfo文件中以大小s开头的行(要求:使用两 种方法) 
			[root@centos6(Eric) Eric]#grep -i “^s” /proc/meminfo
			SwapCached:            0 kB
			SwapTotal:       2097148 kB
			SwapFree:        2097148 kB
			Shmem:               248 kB
			Slab:              35684 kB
			SReclaimable:       9104 kB
			SUnreclaim:        26580 kB

			以下方法都可：
			grep "^[Ss]" /proc/meminfo 
			grep "^S\|^s" /proc/meminfo
			grep -e ^s -e ^S  /proc/meminfo

			2、显示/etc/passwd文件中不以/bin/bash结尾的行 

				[root@centos6(Eric) Eric]#grep -v "/bin/bash$" /etc/passwd
				bin:x:1:1:bin:/bin:/sbin/nologin
				daemon:x:2:2:daemon:/sbin:/sbin/nologin
				adm:x:3:4:adm:/var/adm:/sbin/nologin
				lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
				sync:x:5:0:sync:/sbin:/bin/sync
				shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
				halt:x:7:0:halt:/sbin:/sbin/halt
				...	
			3、显示用户rpc默认的shell程序 
			[root@centos6(Eric) Eric]#grep "rpc\b" /etc/passwd  |cut -d: -f7
/			sbin/nologin

			4、找出/etc/passwd中的两位或三位数 

			[root@centos6(Eric) Eric]#grep --color "[[:digit:]]\{2,3\}" /etc/passwd

			mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
			uucp:x:10:14:uucp:/var/spool/uucp:/sbin/nologin
			operator:x:11:0:operator:/root:/sbin/nologin
			games:x:12:100:games:/usr/games:/sbin/nologin
			gopher:x:13:30:gopher:/var/gopher:/sbin/nologin
			ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
			nobody:x:99:99:Nobody:/:/sbin/nologin
			vcsa:x:69:69:virtual console memory owner:/dev:/sbin/nologin
			saslauth:x:499:76:Saslauthd user:/var/empty/saslauth:/sbin/nologin
			postfix:x:89:89::/var/spool/postfix:/sbin/nologin
			sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
			Eric:x:500:500::/home/Eric:/bin/bash
			rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
			mysql:x:27:27:MySQL Server:/var/lib/mysql:/bin/bash
			ntp:x:38:38::/etc/ntp:/sbin/nologin

			5、显示CentOS7的/etc/grub2.cfg文件中，至少以一个空白 
			字符开头的且后面存非空白字符的行 

			[root@Eric(Eric) ~]#grep "^[[:space:]]\+[^[:space:]]" /etc/grub2.cfg

			注：后面显示内容较多，此处省略。

			6、找出“netstat -tan”命令的结果中以‘LISTEN’后跟任意多个空白字符结尾的行 

			[root@Eric(Eric) ~]#netstat -tan | grep "^LISTEN[[:space:]]\+"

			7、显示CentOS7上所有系统用户的用户名和UID 

			[root@Eric(Eric) ~]#cat /etc/passwd | cut -d: -f1,3 | grep "\b[0-9]\?[0-9]\?[0-9]\b"
			root:0
			bin:1
			daemon:2
			adm:3
			lp:4
			sync:5
			shutdown:6
			halt:7
			mail:8
			operator:11
			games:12
			ftp:14
			nobody:99

			8、添加用户bash、testbash、basher、sh、nologin(其shell 为/sbin/nologin),找出/etc/passwd用户名同shell名的行 

			cat /etc/passwd |grep "\(^.*\)\>.*\<\1$"
			cat /etc/passwd |egrep "(^.*)\>.*/\1$"

			9、利用df和grep，取出磁盘各分区利用率，并从大到小排序 
			[root@centos6(Eric) Eric]#df | grep -o "[[:digit:]]\+%" |sort -nr
			31%
			18%
			7%
			0%

egrep及扩展的正则表达式
	egrep = grep -E 

	语法：
		egrep [OPTIONS] PATTERN [FILE...]

	扩展正则表达式的元字符：

	字符匹配：
		.：任意单个字符
		[]：指定范围的字符
		[^]：不指定范围的字符
	次数匹配：
		*：匹配前面字符任意次
		?：0或1次
		+：1次或多次
		{m}：匹配m次
		{m,n}：至少m次，至多n次
	位置锚定：
		^：行首
		$：行尾
		\<,\b：词首
		\>,\b：词尾
	分组：
		（）：
		后向引用：\1,\2,...
	或者：
	a|b：a或b
	C|cat：C或cat
	(C|c)at：Cat或cat

	model:
		1、显示三个用户root、mage、wang的UID和默认shell
		[root@centos6(Eric) Eric]#cat /etc/passwd | grep -E "^(root|mage|wang)\b" |cut -d: -f3,7
		0:/bin/bash

		2、找出/etc/rc.d/init.d/functions文件中行首为某单词(包括下划线)后面跟一个小括号的行
		[root@centos6(Eric) Eric]#grep -E "^[[:alpha:]_]+\(\)" /etc/rc.d/init.d/functions
		
		3、使用egrep取出/etc/rc.d/init.d/functions中其基名
		[root@centos6(Eric) Eric]#echo /etc/rc.d/init.d/functions | grep -Eo "[^/]+/?$"
		functions
		
		4、使用egrep取出上面路径的目录名
		[root@centos6(Eric) Eric]#echo /etc/rc.d/init.d/functions | grep -Eo "^/.*/"
		/etc/rc.d/init.d/
		
		5、统计last命令中以root登录的每个主机IP地址登录次数
		[root@centos6(Eric) Eric]#last | egrep -o "^root\>.*[0-9]\.[0-9]{1,3}" |tr -s " " "#" | cut -d# -f3 |sort -n |uniq -c
      	1 172.16.15.100
     	26 172.16.99.1

     	[root@centos6(Eric) Eric]#last | grep ^root | egrep -o "([0-9]{1,3}\.){3}[0-9]{1,3}" | sort | uniq -c
      	1 172.16.15.100
     	26 172.16.99.1
		
		6、利用扩展正则表达式分别表示0-9、10-99、100-199、 200-249、250-255
		echo {1..100} |egrep -o "\b[0-9]\b"
		echo {1..100} |egrep -o "\b[0-9]{2}\b"
		echo {1..1000} |egrep -o "\b1[0-9][0-9]\b"
		echo {1..1000} |egrep -o "\b2[0-4][0-9]+\b"
		echo {1..1000} |egrep -o "\b25[0-5]+\b"

		
		7、显示ifconfig命令结果中所有IPv4地址
		ifconfig | egrep -o "([0-9]{1,3}\.){3}[0-9]{1,3}"
		
		8、将此字符串:welcome to magedu linux 中的每个字符 去重并排序，重复次数多的排到前面
		cho "welcome to magedu linux" |grep -Eo [a-z] | sort | uniq -c|sort -nr




		
		