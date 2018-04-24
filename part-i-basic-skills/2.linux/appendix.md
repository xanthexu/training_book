# Appendix

## 附录：Linux常用命令

### 1.创建、删除与复制文件、文件夹

cd 目录切换（区别相对路径与绝对路径）

```text
cd               #切换到家目录

cd /home/usrs    #切换到/home/usrs目录

cd ..            #返回上级目录

cd ../..         #返回上上级目录
```

mkdir 创建文件夹

```text
mkdir folder
```

cp 复制文件

```text
cp old_file new_file
```

mv 重命名或移动文件、文件夹

```text
mv file_old_name file_new_name        #文件重命名

mv folder_old_neme folder_new_name    #文件夹重命名

mv file new_folder/                   #将文件移动到新目录
```

rmdir 删除文件夹

```text
rmdir folder
```

rm 删除文件、文件夹

```text
rm file         #删除文件

rm -r folder    #删除文件夹
```

### 2.读写文件、文件夹

ls 显示文件夹中文件列表

```text
ls -la    ＃显示全部详细格式

ll        ＃显示文件详细信息
```

cat 直接查看文件

more/less 翻页查看文件

sed 编辑文件

```text
sed ‘s/A/a/g’ file     ＃将文件中所有的A替换为a

sed -n ‘3,6 p’ file    ＃打印第3到6行

sed ‘5 q’ file         ＃打印前5行
```

&gt; 重定向，将终端结果输出给文件，会创建新文件或者覆盖原文件

```text
sed ‘s/A/a/g’ file > new_file
```

&gt;&gt; 重定向，将终端结果输出给文件，内容会加在原文件内容尾部

```text
sed ‘5 q’ file >> new_file
```

### 3.文件信息提取

wc 查看文件行数、字数

```text
 wc -l file     ＃查看文件行

 wc -c file     ＃查看文件字数
```

head 查看文件前几行

```text
head -n [N] file     ＃查看文件前N行
```

tail 查看文件后几行

```text
 tail -n [N] file    ＃查看文件后N行
```

grep 文件中关键词搜索，返回行

```text
grep ‘CDS’ file       ＃显示匹配上 ‘CDS’ 的所有行

grep -v ‘CDS’ file    ＃显示没有匹配上’CDS’的所有行

grep -w ‘CDS’ file    ＃必须与整个字匹配，如果另外有’CDSs’这样的字符串，就不会与之匹配

grep -x ‘CDS’ file    ＃仅匹配整行，即只显示整行就是’CDS’的所有行
```

cut 取出文件中的特定列或字符

```text
cut -f [N] file            ＃取出第N列

cut -d “;” -f [N] file     ＃以”；”作为输入字段的分隔符（默认为制表符），取出第N列
```

sort 排序

```text
sort -n file                 ＃按照数值排序（默认为按照ASCII码排序）

sort -k start[,stop] file    ＃按照指定的字段进行排序；start指定开始处字段，stop指定结尾处字段；如果省略stop，默认到行尾
```

uniq 去重复

```text
 uniq -c     ＃去重复并且计算重复频率
```

\| 管道，命令粘合剂，可将某命令的结果输出给另一个命令

```text
head -n 20 file | tail -n 10

cut -f 3 file | sort | uniq -c
```

### 4.查找文件

which 寻找可执行文件，显示路径

whereis 寻找特定文件

locate 按照关键词搜索文件

### 5. 查看、修改文件权限

用户及用户组：文件所有者u \(user\)，用户组g \(group\)，其他人o \(other\)，所有人a \(all\)

chmod 修改文件的访问权限，分为数字模式和符号模式。

_数字模式：_

```text
chmod 755 file

chmod -R 755 folder     ＃-R  修改该目录中所有文件的权限
```

三位数分别表示文件所有者，用户组，其他人

r表示可读，w表示可写，x表示可执行

用数字表示：可读r=1，可写w=2，可执行x=4

例如：777表示所有用户对文件具有读、写、执行权限；755表示文件所有者对文件具有可读、可写、可执行权限，其他用户只具有可读、可执行权限

_  
符号模式：_

```text
chmod u+x,go=rx file   #使file文件的所有者加上可执行权限，将用户组和其他人权限设置为可读和可执行

chmod o-x file         ＃使其他人对file文件除去可执行权限

chmod -R a+x folder    ＃使所有人对folder文件夹加上可执行权限
```

符号：+加入，－除去，＝设置

### 6.文件、文件夹的打包，压缩与解压缩

gzip 压缩文件

```text
gzip file
```

gunzip 解压缩文件（.gz文件）

```text
gunzip file.gz
```

tar 打包压缩文件、文件夹的产生与解压缩

```text
tar -zcv -f folder.tar.gz folder    ＃打包压缩文件夹（gzip格式）

tar -jcv -f folder.tar.gz folder    ＃打包压缩文件夹（bzip2格式）

tar –ztv -f folder.tar.gz           ＃查看压缩文件夹中的文件名（gzip格式）

tar –jtv -f folder.tar.gz           ＃查看压缩文件夹中的文件名（bzip2格式）

tar -zxv –f folder.tar.gz           ＃打开包并解压缩（gzip格式）

tar -jxv –f folder.tar.gz           ＃打开包解压缩（bzip2格式）
```

_  
-z 压缩（gzip格式）_

_  
-j 压缩（bzip2格式）_

_  
-c 打包_

_  
-v 显示过程_

_-f 新建的压缩文件名_

_  
-t 查看压缩包里的文件名_

_  
-x 解压缩_

### 7. 系统及进程

top 监视计算机使用情况

ctrl-c 终止当前进程

ctrl-z 暂停当前进程

bg 让暂停的进程在后台继续运行

fg 把后台运行的进程调到前台

ps 列出所有用户的进程

```text
ps -e     #列出所有的用户的进程

ps -f     #列出详细的列表
```

kill 终止特定进程

```text
kill PID_list     #终止PID进程
```

date 显示系统的时间和日期，可用于为程序运行时长进行计时
