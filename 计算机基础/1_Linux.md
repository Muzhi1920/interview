# Linux
- ps：查看当前进程
  1. ps -ef (system v 输出) 
  2. ps -aux bsd 格式输出
  3. ps -ef | grep pid
- ln：软连接，快捷方式；硬链接：同步更改。
- chmod abc file a,b,c：分别表示User、Group、及Other的权限。若要rwx属性则4+2+1=7
- vi 文件名编辑 cat file | more | less
  1. more 分页 
  2. less 分页可回翻
  3. tail 尾部
  4. head 头部
- wc 命令 - c 统计字节数 - l 统计行数 - w 统计字数。
- grep [string] filename grep [^string] filename
- nohup sh **.sh & 后台运行 job -l fg bg
- kill -9 pid
- 1. find <指定目录> <指定条件> <指定动作>
  2. whereis 加参数与文件名
  3. locate 只加文件名
- history ,who am i,
- df -hl 磁盘使用
- hash：记录了已执行过的命令的完整路径, 用该命令可以打印出你所使用过的命令以及执行的次数。
- let 可以进行整型数的数学运算

---
## 一、man_page
1. 内部命令：echo
查看内部命令帮助：help echo 或者 man echo
2. 外部命令：ls
查看外部命令帮助：ls --help 或者 man ls 或者 info ls
3. 快捷键：
ctrl + c：停止进程
ctrl + l：清屏
ctrl + r：搜索历史命令
ctrl + q：退出
4.善于用tab键：文件名补全

## 二、常用命令
1. 进入到用户根目录
cd ~ 或 cd
2. 查看当前所在目录
pwd
3. 进入到itcast用户根目录
cd ~itcast
4. 返回到原来目录
cd -
5. 返回到上一级目录
cd ..
6. 查看itcast用户根目录下的所有文件
ls -la
7. 在根目录下创建一个itcast的文件夹
mkdir /itcast
8. 在/itcast目录下创建src和WebRoot两个文件夹
分别创建：mkdir /itcast/src     mkdir /itcast/WebRoot         同时创建：mkdir /itcast/{src,WebRoot}
进入到/itcast目录，在该目录下创建.classpath和README文件
分别创建：touch .classpath      touch README                     同时创建：touch {.classpath,README}
9. 查看/itcast目录下面的所有文件
ls -la
10. 在/itcast目录下面创建一个test.txt文件,同时写入内容"this is test"
echo "this is test" > test.txt
11. 查看一下test.txt的内容
cat test.txt
more test.txt
less test.txt
12. 向README文件追加写入"please read me first"
echo "please read me first" >> README
13. 将test.txt的内容追加到README文件中
cat test.txt >> README
14. 拷贝/itcast目录下的所有文件到/itcast-bak
cp -r /itcast /itcast-bak
15. 进入到/itcast-bak目录，将test.txt移动到src目录下，并修改文件名为Student.java
mv test.txt src/Student.java
16. 在src目录下创建一个struts.xml
> struts.xml
17. 删除所有的xml类型的文件
rm -rf *.xml
18. 删除/itcast-bak目录和下面的所有文件
rm -rf /itcast-bak
19. 返回到/itcast目录，查看一下README文件有多单词，多少个少行
wc -w README
wc -l README
20. 返回到根目录，将/itcast目录先打包，再用gzip压缩
分步完成：tar -cvf itcast.tar itcast    gzip itcast.tar
一步完成：tar -zcvf itcast.tar.gz itcast
21. 将其解压缩，再取消打包
分步完成：gzip -d itcast.tar.gz 或 gunzip itcast.tar.gz
一步完成：tar -zxvf itcast.tar.gz
将/itcast目录先打包，同时用bzip2压缩，并保存到/tmp目录下
tar -jcvf /tmp/itcast.tar.bz2 itcast
将/tmp/itcast.tar.bz2解压到/usr目录下面
tar -jxvf itcast.tar.bz2 -C /usr/

三、文件相关命令
1. 进入到用户根目录
cd ~ 或者 cd
cd ~hadoop
回到原来路径
cd -
2. 查看文件详情
stat a.txt
3. 移动
mv a.txt /ect/
改名
mv b.txt a.txt
移动并改名
mv a.txt ../b.txt
4. 拷贝并改名
cp a.txt /etc/b.txt
5. vi撤销修改
ctrl + u (undo)
恢复
ctrl + r (redo)
6. 名令设置别名(重启后无效)
alias ll="ls -l"
取消
unalias ll
7. 如果想让别名重启后仍然有效需要修改
vi ~/.bashrc
8. 添加用户
useradd hadoop
passwd hadoop
9. 创建多个文件
touch a.txt b.txt
touch /home/{a.txt,b.txt}
10. 将一个文件的内容复制到里另一个文件中
cat a.txt > b.txt
追加内容
cat a.txt >> b.txt 
11. 将a.txt 与b.txt设为其拥有者和其所属同一个组者可写入，但其他以外的人则不可写入:
chmod ug+w,o-w a.txt b.txt
chmod a=wx c.txt
12. 将当前目录下的所有文件与子目录皆设为任何人可读取:
chmod -R a+r *
13. 将a.txt的用户拥有者设为users,组的拥有者设为jessie:
chown users:jessie a.txt
14. 将当前目录下的所有文件与子目录的用户的使用者为lamport,组拥有者皆设为users，
chown -R lamport:users *
15. 将所有的java语言程式拷贝至finished子目录中:
cp *.java finished
16. 将目前目录及其子目录下所有扩展名是java的文件列出来。
find -name "*.java"
查找当前目录下扩展名是java 的文件
find -name *.java
17. 删除当前目录下扩展名是java的文件
rm -f *.java

##四、系统命令
1. 查看主机名
hostname
2. 修改主机名(重启后无效)
hostname hadoop
3. 修改主机名(重启后永久生效)
vi /ect/sysconfig/network
4. 修改IP(重启后无效)
ifconfig eth0 192.168.12.22
5. 修改IP(重启后永久生效)
vi /etc/sysconfig/network-scripts/ifcfg-eth0
6. 查看系统信息
uname -a
uname -r
7. 查看ID命令
id -u
id -g
8. 日期
date
date +%Y-%m-%d
date +%T
date +%Y-%m-%d" "%T
9. 日历
cal 2012
10. 查看文件信息
file filename
11. 挂载硬盘
mount
umount
加载windows共享
mount -t cifs //192.168.1.100/tools /mnt
12. 查看文件大小
du -h
du -ah
13. 查看分区
df -h
14. ssh
ssh hadoop@192.168.1.1
15. 关机
shutdown -h now /init 0
shutdown -r now /reboot

## 五、用户和组
1. 添加一个tom用户，设置它属于users组，并添加注释信息
分步完成：useradd tom
          usermod -g users tom
          usermod -c "hr tom" tom
一步完成：useradd -g users -c "hr tom" tom

2. 设置tom用户的密码
passwd tom
3. 修改tom用户的登陆名为tomcat
usermod -l tomcat tom
4. 将tomcat添加到sys和root组中
usermod -G sys,root tomcat
6. 查看tomcat的组信息
groups tomcat
7. 添加一个jerry用户并设置密码
useradd jerry
passwd jerry
8. 添加一个交america的组
groupadd america
9. 将jerry添加到america组中
usermod -g america jerry
10. 将tomcat用户从root组和sys组删除
gpasswd -d tomcat root
gpasswd -d tomcat sys
11. 将america组名修改为am
groupmod -n am america

## 六、权限
创建a.txt和b.txt文件，将他们设为其拥有者和所在组可写入，但其他以外的人则不可写入:
chmod ug+w,o-w a.txt b.txt

创建c.txt文件所有人都可以写和执行
chmod a=wx c.txt 或chmod 666 c.txt

将/itcast目录下的所有文件与子目录皆设为任何人可读取
chmod -R a+r /itcast

将/itcast目录下的所有文件与子目录的拥有者设为root，用户拥有组为users
chown -R root:users /itcast

将当前目录下的所有文件与子目录的用户皆设为itcast，组设为users
chown -R itcast:users *

## 七、文件夹属性
1. 查看文件夹属性
ls -ld test

2. 文件夹的rwx
--x:可以cd进去\
r-x:可以cd进去并ls\
-wx:可以cd进去并touch，rm自己的文件，并且可以vi其他用户的文件\
-wt:可以cd进去并touch，rm自己的文件\
ls -ld /tmp\
drwxrwxrwt的权限值是1777(sticky)\

## 八、vim
- i   插入编辑操作
- a/A
- o/O
- r + ?替换
- 0:文件当前行的开头
- $:文件当前行的末尾
- G:文件的最后一行开头
- 1 + G到第一行 
- 9 + G到第九行 = :9
- dd:删除一行
- 3dd：删除3行
- yy:复制一行
- 3yy:复制3行
- p:粘贴
- u:undo
- ctrl + r:redo
- "a剪切板a
- "b剪切板b
- "ap粘贴剪切板a的内容
- 每次进入vi就有行号
- vi ~/.vimrc
- set nu
- :w a.txt另存为
- :w >> a.txt内容追加到a.txt
- :e!恢复到最初状态
- :1,$s/hadoop/root/g 将第一行到追后一行的hadoop替换为root
- :1,$s/hadoop/root/c 将第一行到追后一行的hadoop替换为root(有提示)

## 九、查找
1. 查找可执行的命令：
which ls
2. 查找可执行的命令和帮助的位置：
whereis ls
3. 查找文件(需要更新库:updatedb)
locate hadoop.txt
4. 从某个文件夹开始查找
find / -name "hadooop*"
find / -name "hadooop*" -ls
5. 查找并删除
find / -name "hadooop*" -ok rm {} \;
find / -name "hadooop*" -exec rm {} \;
6. 查找用户为hadoop的文件
find /usr -user hadoop -ls
7. 查找用户为hadoop并且(-a)拥有组为root的文件
find /usr -user hadoop -a -group root -ls
8. 查找用户为hadoop或者(-o)拥有组为root并且是文件夹类型的文件
find /usr -user hadoop -o -group root -a -type d
9. 查找权限为777的文件
find / -perm -777 -type d -ls
10. 显示命令历史
history
11. grep
grep hadoop /etc/password

## 十、打包与压缩
1. gzip压缩
gzip a.txt
2. 解压
gunzip a.txt.gz
gzip -d a.txt.gz
3. bzip2压缩
bzip2 a
4. 解压
bunzip2 a.bz2
bzip2 -d a.bz2
5. 将当前目录的文件打包
tar -cvf bak.tar .
将/etc/password追加文件到bak.tar中(r)
tar -rvf bak.tar /etc/password
6. 解压
tar -xvf bak.tar
7. 打包并压缩gzip
tar -zcvf a.tar.gz
8. 解压缩
tar -zxvf a.tar.gz
解压到/usr/下
tar -zxvf a.tar.gz -C /usr
9. 查看压缩包内容
tar -ztvf a.tar.gz
zip/unzip
10. 打包并压缩成bz2
tar -jcvf a.tar.bz2
11. 解压bz2
tar -jxvf a.tar.bz2

## 十一、正则表达式
1. cut截取以:分割保留第七段
grep hadoop /etc/passwd | cut -d: -f7
2. 排序
du | sort -n 
3. 查询不包含hadoop的
grep -v hadoop /etc/passwd
4. 正则表达包含hadoop
grep 'hadoop' /etc/passwd
5. 正则表达(点代表任意一个字符)
grep 'h.*p' /etc/passwd
6. 正则表达以hadoop开头
grep '^hadoop' /etc/passwd
7. 正则表达以hadoop结尾
grep 'hadoop$' /etc/passwd

.  : 任意一个字符\
a* : 任意多个a(零个或多个a)\
a? : 零个或一个a\
a+ : 一个或多个a\
.* : 任意多个任意字符\
查找不是以#开头的行\
grep -v '^#' a.txt | grep -v '^$' \
以h或r开头的\
grep '^[hr]' /etc/passwd\
不是以h和r开头的\
grep '^[^hr]' /etc/passwd\
不是以h到r开头的\
grep '^[^h-r]' /etc/passwd

## 十二、进程控制
1. 查看用户最近登录情况
last
lastlog
2. 查看硬盘使用情况
df
3. 查看文件大小
du
4. 查看内存使用情况
free
5. 查看文件系统
/proc
6. 查看日志
ls /var/log/
7. 查看系统报错日志
tail /var/log/messages
8. 查看进程
top
9. 结束进程
kill 1234
kill -9 4333

## 十三、linux的命令操作
用vi文本编辑器来编辑生成文件
******最基本用法
vi  somefile.4
1. 首先会进入“一般模式”，此模式只接受各种快捷键，不能编辑文件内容
2. 按i键，就会从一般模式进入编辑模式，此模式下，敲入的都是文件内容
3. 编辑完成之后，按Esc键退出编辑模式，回到一般模式；
4. 再按：，进入“底行命令模式”，输入wq命令，回车即可

### 查看文件内容
cat    somefile    一次性将文件内容全部输出（控制台）\
more  somefile    可以翻页查看, 下翻一页(空格)    上翻一页（b）  退出（q）\
less  somefile      可以翻页查看,下翻一页(空格)    上翻一页（b），上翻一行(↑)  下翻一行（↓）  可以搜索关键字（/keyword）

tail -10  install.log  查看文件尾部的10行\
tail -f install.log    小f跟踪文件的唯一inode号，就算文件改名后，还是跟踪原来这个inode表示的文件\
tail -F install.log    大F按照文件名来跟踪\
head -10  install.log  查看文件头部的10行

文件内容查阅\
head -10 a.log 取前10行\
tail -10 a.log 取后10行\
tail -f a.log 实时检测日志更新\
tail -f30 a.log 实时监测该日志的后30行\
tab：代码补全，文件名补全\
文件类型权限 drwxrwx user，group / 4,2,1打分\
查看日志var/log\

### 3、文件权限的操作

drwxr-xr-x      （也可以用二进制表示  111 101 101  -->  755）
d：标识节点类型（d：文件夹  -：文件  l:链接）\
r：可读  w：可写    x：可执行 \
第一组rwx：  表示这个文件的拥有者对它的权限：可读可写可执行\
第二组r-x：  表示这个文件的所属组对它的权限：可读，不可写，可执行\
第三组r-x：  表示这个文件的其他用户（相对于上面两类用户）对它的权限：可读，不可写，可执行

chmod g-rw haha.dat    表示将haha.dat对所属组的rw权限取消\
chmod o-rw haha.dat    表示将haha.dat对其他人的rw权限取消\
chmod u+x haha.dat      表示将haha.dat对所属用户的权限增加x

也可以用数字的方式来修改权限\
chmod 664 haha.dat  \
就会修改成  rw-rw-r--

如果要将一个文件夹的所有内容权限统一修改，则可以-R参数\
chmod -R 770 aaa/\
chown angela:angela aaa/    <只有root能执行>

4、基本的用户管理
useradd  angela
要修改密码才能登陆 
passwd angela  按提示输入密码即可

**为用户配置sudo权限
用root编辑 vi /etc/sudoers
在文件的如下位置，为hadoop添加一行即可
root    ALL=(ALL)      ALL    \
hadoop  ALL=(ALL)      ALL

然后，hadoop用户就可以用sudo来执行系统级别的指令
[hadoop@shizhan ~]$ sudo useradd huangxiaoming

5、系统管理操作
- 查看主机名
hostname
- 修改主机名(重启后无效)
hostname hadoop
- 修改主机名(重启后永久生效)
vi /ect/sysconfig/network
- 修改IP(重启后无效)
ifconfig eth0 192.168.12.22

- 修改IP(重启后永久生效)
vi /etc/sysconfig/network-scripts/ifcfg-eth0

mkdir  /mnt/cdrom      创建一个目录，用来挂载\
mount -t iso9660 -o ro /dev/cdrom /mnt/cdrom/    将设备/dev/cdrom挂载到 挂载点 ：  /mnt/cdrom中

- umount
umount /mnt/cdrom

- 统计文件或文件夹的大小
du -sh  /mnt/cdrom/Packages\
df -h    查看磁盘的空间
- 关机
halt
- 重启
reboot

******配置主机之间的免密ssh登陆\
假如 A  要登陆  B\
在A上操作：\
%%首先生成密钥对\
ssh-keygen  (提示时，直接回车即可)\
%%再将A自己的公钥拷贝并追加到B的授权列表文件authorized_keys中\
ssh-copy-id  B

******后台服务管理
service network status  查看指定服务的状态\
service network stop    停止指定服务\
service network start    启动指定服务\
service network restart  重启指定服务\
service --status-all  查看系统中所有的后台服务

设置后台服务的自启配置\
chkconfig  查看所有服务器自启配置\
chkconfig iptables off  关掉指定服务的自动启动\
chkconfig iptables on  开启指定服务的自动启动
