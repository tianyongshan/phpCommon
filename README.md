# phpstorm
##  1、注册激活
**Help>Register>Lisence Server**
输入 http://idea.imsxm.com

##  2、快捷键
**Alt+J**:依次选中所有下文出现的选中的内容

# NotePad++
##  1 、快捷键

**shift+Alt+鼠标选中：** 选择特定横向、纵向区域
# mysql
##  1、设置pager
**设置pager**：将mysql的执行结果快速原样保存到服务器文件
 ~~~
pager cat > /data/webroot/XXX/testMysql.csv   ; #设置pager
nopager ; #取消pager
 ~~~

##  2、mysql当前进程数
可用作紧急处理：

(1)当mysql query数满了，
 (2)或者 慢查询导致大片的query 处于等待
 紧急处理可以杀死query，先让系统不至于崩溃。
**注意**：只是紧急处理，需要排查是具体哪里导致的问题，从根源解决

 ~~~
show processlist   \G;#查询当前的query
kill '第一列的数字' ; #杀死该查询
 ~~~


## 3、case


~~~
select id,
case tableName.fieldName
when '0' then '情况1'
when '2' then '情况2'
when '3' then '情况3'
end as fieldName
from `tableName`

~~~

# curl
## 1 shell curl
###  1.1  params
~~~
#post传参
curl -d "page=1&paramsTwo=XXX"  XXX.com  
~~~
~~~
# post 传参 +绑定host
curl -G  -H 'Host: XXX.com' -d "paramsOne=02&paramTwo=CT"    http://111.111.111.111/XXXX/xxxx/xxxx
 
~~~
~~~

# 传数组
curl  -d "paramsOne=a" -d "array[0][xx]=1111&array[0][xxx]=1111"  xx.xx.xx.xx.com/xx/xxx/xxxx
~~~
~~~
# 传中文 [服务器上执行]
curl -G  -H 'Host: xxx.com'  --data-urlencode "paramOne=a" --data-urlencode "array[0][xx]=中文" 
  http://111.111.111/xxx/xxx/xxxx

~~~

# ini
## 1 、error report 
~~~
error_reporting(E_ALL);
ini_set('display_errors', '1');
~~~
