



**shell脚本编辑**

- 变量定义
  - 数字字母下划线, 不能数字开头,不能使用关键字
  - 使用定义过的变量只要在变量名前加  $ 即可
  - $(ls ) 将当前目录下的文件遍历出来
- 去变量值  $
- 变量边界 ${}
- 子符长度${#}
- 去元素   ${数组名[下标]}

**`$` 的使用**

- `$#`传递到脚本的参数
- `$*`以一个单字符串显示向脚本传递的参数,  即 传递给脚本的参数组装
- `$$`脚本运行的进程号
- `$!`后台运行的最后一个进程号


**`test`命令 **  可参考菜鸟教程[shell test 命令](http://www.runoob.com/linux/linux-shell-test.html)

 数值测试

- `-eq ` 等于则为 true  ` equal`
- `-ne `不等于 true   `not equal`
- `-gt`  大于为true  `great`
- `-ge `大于等于 true ` great equal`
- `-lt `小于 true  `little`
- ` -le `小于等于 true     `little equal`

字符串测试

=    !=   -z    字符串长度为0   -n  字符串长度不为0

文件测试

- -e 文件存在  true
- -r 可读文件
- -w 可写文件
- -x 可执行
- -s 文件至少有一个字符
- -d 文件存在且为目录
- -f 文件为普通文件
- -c 文件为特殊文件 
- -b 文件为块特殊文件



代码示例

```shell
#!/bin/bash

func(){
    result=1
    n=1 
    while test $n -le $2
    do
        result=$(($result*$1))
        n=$(($n+1))
    done
    return $result 
}

func 5 3 
echo $?
```

```shell
#!/bin/bash

if test -e ~/abc
then
    rm -rf ~/abc
fi

mkdir ~/test/abc
num=1
while test $num -le 100
do
    touch "user${num}.text
    mv "user${num}.txt ~/test/abc/
    num=$(($num+1))
done 
```

