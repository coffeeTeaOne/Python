## Linux命令和操作方法



### **linux介绍**

1. Linux 的内核是开源的  下载 kernel.org 网站下载 开源内核  第二位(中间)是偶数是稳定版本
2. linux --> 基于 MINIX -- > 基于 UniX       
   - Linux 是通用操作系统
   - 第一台计算机    --> 帕斯卡发明的    Pascal   17岁 
   - 第一台数字计算机  ---差分机---没有软件只有硬件   -- 第一个程序员Ada
   - 第一台电子数字计算机    - ENIAC -  
   - 图灵   程序员  --死于一个毒苹果   --苹果Logo 纪念它
3. 内核是Linux  Linux发行版本    Redhat  Linux  Ubantu 
4. Nginx    -可以把阿里云变为 Web 服务器
   MySQL    -关系型数据库 -持久化数据   
   Redis    - 非关系型数据库
   FTP
   Mail
   防火墙   iptables /filewalls



#### **1.操作命令介绍**

- root 提示符是  #, 普通用户是  $
- who  查看哪些用户登录了系统
- whoami 查看自己是谁
- w  查看详细信息
- clear 清屏     windows系统  cls
- ps  查看内核  查看 shell 种类 (bash  zsh csh)
- adduser  创建用户 + 用户名
- passwd   + 用户名  回车   输入密码
- logout 断开服务器连接
- reboot  重启服务器   也可以使用 `init 6 `
- shutdown  关闭服务器   也可以使用 `init 0 `
- uname 查看操作系统名称
- hostname  主机名 
- tab tab 自动补全功能,给与提示
- 查看帮助
  - man shutdown   查看命令的使用手册    man + 命令
  - info  + 命令    专业角度详细解释命令
  - 命令 --help  查看帮助
  - whatis + 命令名 查看简短的秒数 
  - [apropos](https://baike.baidu.com/item/apropos/15852795)
- `which  python`  那个是python 找到python解释器的位置
- `whereis 文件/应用`  查看文件/应用所在的目录

- `Ctrl + c` 终止命令
- **jobs   查看有没有后台程序**
  1. fg  %1 将一号任务放在前台使用 
  2. bg %1 将任务放回后台
  3. ctrl + z  停止任务



#### **2.身份切换**

1. `sudo`  扮演超级管理员身份

2. `su` + 用户名  切换用户

3. `usrdel`删除用户

4. `ssh root@IP`地址   远程连接其他服务器    wall 超级管理员发警告

   - `scp `安全拷贝      可以从其他服务器里拷贝东西  
     - **`scp 原文件 目标文件  hellokitty@ip :/home/hellokitty`**


#### **3.文件操作**

- 文件参数
  - 修改参数修改内容    
  - 访问权限
  - 最后访问时间

*1.查看文件*

- `ls `
  - 查看文件夹   `list directory contents`
  - `ls -l  ` 查看完整的文件目录详情    列表开头是d 开始的都是文件目录
  - `ls -a  ` 查看所有文件       以  .  开头的文件和文件夹都是隐藏文件
  - `ls -la  `查看所有信息  长格式所有文件
  - `ls --help | less  `少显示
  - `ls -d   `显示问价夹
  - `ls -r    reverse `  反转显示    按首字母反序显示
  - `ls -R  `平铺式显示文件   所有文件夹都展开显示  `reucrsive ` ()递归)
- `cd` 
  - 回到上级目录  `cd ..  `相对路径   也可以用绝对路径  `cd /home/目录名  `
  - `cd ~ `回到用户主目录
  - `cd /   ` 系统根目录

*2.文件权限*

- rw- 不能执行文件   +x  ./文件名 执行文件
- rex  r-x   r-x  文件所有者可以读 写 执行(execute) 其他能读和执行
- chmod u+x + 文件名  加执行权限 
- chmod o+x,g+x 文件名   给同组用户 其他用户添加执行权限
- chmod 777 + 文件名   所有人都有读写权限  二进制  

```shell
* rwx  rwx  rwx    rwx rw- rw- rwx r-- r--  rw- -wx  -wx 
* 111  111  111    111 110 110 111 100 100  110  011  011
* 7    7    7       7   6  6   7   4   4     6    3    3
* 4 只读   5 读 执行  6 读写  7 读写执行
```



*2.文件创建删除*

- `pwd` 查看当前用户主目录
- `mkdir `创建文件夹  `make directory` 
- `rmdir` 删除文件夹
- `touch `创建空文件,  修改文件的访问时间戳  (访问时间改变)
  - touch 可以修改每个文件的最后访问时间
- `mv` 移动文件  也可原文件改名 
- 复制文件 `` cp ``    cp  文件名   要复制到文件目录 / 复制后的文件名



*3.cat*

- *查看文件内容 concatenate    cat + 文件名*

1. cat  文件名 | less / more   分页查看   | 添加管道
2. cat + meminfo     内存信息    cat + cpuinfo cpu信息
3. head 查看文件开头    head 文件名 - n   查看开头多少行
4. tail 文件结尾   -n 
5. find -name *.html查找文件   在当前路径下查找文



*4.grep*

- grep 查找字符串 在一段字符串中
- cat 文件名 | grep  查找内容 可加正则表达式
- grep + 查找的内容 + 文件名 -n  出现在多少行
- grep  查找内容     . / -n  当前路径下是所有文件下查找
- grep  查找内容   >  文件名 &  将查找的内容输出重定向 后台执行    在在后面加    2> error.txt   将错误输出写入到error.txt中   (f覆盖模式)
  < 输入重定向
- \>> 文件名   追加模式   



#### **4.压缩归档文件**

```
1、*.tar 用 tar –xvf 解压 
2、*.gz 用 gzip -d或者gunzip 解压 
3、*.tar.gz和*.tgz 用 tar –xzf 解压 
4、*.bz2 用 bzip2 -d或者用bunzip2 解压 
5、*.tar.bz2用tar –xjf 解压 
6、*.Z 用 uncompress 解压 
7、*.tar.Z 用tar –xZf 解压 
8、*.rar 用 unrar e解压 
9、*.zip 用 unzip 解压 
```



- **压缩**
  - `gzip`**压缩文件**     压缩比
  - `gunzip`  +文件名**解压缩文件** `后缀是.gz` 的文件  
  - `gz` 解压缩    
  - `xz`  将 后缀是 `.xz`格式的文件解压缩    
    - `-z  ` 压缩 后面  加` - 8` 指定压缩比 
    - ` -d`  解压缩  
- **归档**
  - `tar  -tf  ` 查看归档文件的内容
    - **`tar -cvf  all.tar *` **归档文件   将文件归档到all.tar 中  
    - `*` 表示将所有文件归档到一个文件中
    - 也可以写文件路径
  - **`tar -xvf`** 解归档   把一个文件 拆成多个文件
    - `-v  `列出过程   不加也可以 
    - `-f `指定文件名

#### **5.链接(备份)**

- **硬链接**   `ln`   更改之后所有的都改变了
  - 没有拷贝文件  ,不消耗内存, 只是创建一个链接引用文件
  - 给文件创建引用   `ln  文件名 要备份到的位置和新的文件名`
  - 只要对象有引用  垃圾回收不会回收文件,文件不会被删除
  - `ls -l  文件名`查看文件的**状态**  链接的个数 链接为  1  `rm`会被删除
  - 硬链接数表示文件被备份多少份   链接数是 1 时删除会被删除
- **软链接**   `ln -s`
  - ` ln -s  文件位置   软链接名  ` 给文件创建软链接 
  - history 输入命令记录   !num 再次输入
  - `HISTSIZE=2000`设置保存的历史指令的条数
  - **echo **回声命令  可以创建文件并写入内容
    - echo  `' print("hello")' > hello.py `  创建文件 并写入内容
    - echo  `$a`   取`变量` a 的值  `已设置  a = 2`
    - `echo  $((a + b)) `计算   a + b 的值
    - `echo  $HISTSIZE`查看历史记录条数

#### 6.网络下载

- wget + 网址   网络下载文件
  - wget -O +文件目录/ 文件名 + 地址   将文件放在哪个位置
- top 查看CPU占 用率