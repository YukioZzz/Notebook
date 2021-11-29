### A way to extract infos and store them in files
    
    echo "running benchmark"
    ./benchmark.sh | grep Requests/sec | awk '{print $2}' > bench.out

### single/double quote and parentheses

- 单引号即原封不动保持原样
- 双引号即执行$命令
- 小括号把 command group 放在subshell去执行，也叫做 nested sub-shell
- 中括号则是在同一个 shell 內完成，也称为 non-namedcommand group
- 两个括号(())，是代表算数扩展，就是对其包括的东西进行标准的算数计算——注意，不能算浮点数，如果需要算浮点数，需要用bc做
- 使用[[...]]条件判断结构, 而不是[...], 能够防止脚本中的许多逻辑错误.比如,&&, ||, <,和> 操作符能够正常存在于[[ ]]条件判断结构中, 但是如果出现在[ ]结构中的话,会报错。

### special dollar

reference to this [link](https://stackoverflow.com/questions/5163144/what-are-the-special-dollar-sign-shell-variables)
    
- `$1`, `$2`, `$3`, ... are the positional parameters.
- `$@` is an array-like construct of all positional parameters, {$1, $2, $3 ...}.
- `$*` is the IFS expansion of all positional parameters, $1 $2 $3 ....
- `$#` is the number of positional parameters.
- `$-` current options set for the shell.
- `$$` pid of the current shell (not subshell).
- `$_` most recent parameter (or the abs path of the command to start the current shell immediately after startup).
- `$IFS` is the (input) field separator.
- `$?` is the most recent foreground pipeline exit status.
- `$!` is the PID of the most recent background command.
- `$0` is the name of the shell or shell script.

### boolean
```
the_world_is_flat=true
# ...do something interesting...
if [ "$the_world_is_flat" = true ] ; then
    echo 'Be careful not to fall off!'
fi
```
参见[stackoverflow](https://stackoverflow.com/questions/2953646/how-can-i-declare-and-use-boolean-variables-in-a-shell-script)

### Check if file exists
如下两种皆可，其中对于`test`命令,`-e`表示任意文件，`-f`要求是regular file(即不是目录或设备)
```
FILE=/etc/resolv.conf
if test -f "$FILE"; then
    echo "$FILE exists."
fi
```

```
FILE=/etc/resolv.conf
if [ -f "$FILE" ]; then
    echo "$FILE exists."
fi
```

### 列表和循环
详细可见[文章](https://www.cnblogs.com/sparkdev/p/7152164.html)
第一种range loop

    arr=(1 2 3 4 5)
    for value in ${arr[@]}  ##这里是引用所有元素
    do
      echo $value
    done

或者

    arr=(1 2 3 4 5)
    for ((i=0; i<${#arr[@]};i++}))  ##这里是求了个长度，注意必须双括号
    do
      echo ${arr[$i]} 
    done

### 获取参数
```
while getopts ":d:" opt
do
    case $opt in
        d)
        if [ "$OPTARG" = false]; then
            DOWNLOAD=false
        fi
        ;;
        ?)
        echo "error params"
        exit 1;;
    esac
done
```
