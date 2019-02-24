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

# redis 
## 1 pub sub 

### 1.1 sub 端
~~~
public static function redisSub(){

    /**
     * redis sub(消息订阅端)
     */
    $redis = new Redis();
    $res = $redis->pconnect('127.0.0.1', 6379,0);
    $redis->auth('XXX');//密码
    $redis->setOption(Redis::OPT_READ_TIMEOUT, -1);//超时时间
    $redis->subscribe(array('exampleTopic'), 'callback');
    
}


// 回调函数,自己的处理逻辑
public static function callback($instance, $channelName, $message)
{
    switch ($channelName) {
        case 'exampleTopic'://调用自定义的接口，执行业务逻辑
            Post('https://xxx.com/Xxx/Xx/xx');
            break;

        default:

    }
}
~~~

### 1.2 pub 端

~~~

//redis发布端
public static function RedisPub(){

    $redis = new Redis();
    $res = $redis->pconnect('127.0.0.1', 6379,0);
    // ... 业务了逻辑
    
    //业务逻辑结束 发布订阅消息
    $redis->PUBLISH('exampleTopic ', '消息');

}
~~~


##  2 、counter 

~~~
public static function redisCounter(){
  
    $value = Mod::app()->redis->incr('website_visitors');
    Mod::app()->redis->expire('website_visitors', 1);
    echo "Total website visitors: " .  $value . ".";

}
~~~

##  3 、 set

~~~

public static function RedisS(){

    Mod::app()->redis->delete('s');
    Mod::app()->redis->sAdd('s', 5);
    Mod::app()->redis->sAdd('s', 4);
    Mod::app()->redis->sAdd('s', 2);
    Mod::app()->redis->sAdd('s', 1);
    Mod::app()->redis->sAdd('s', 3);

    var_dump(Mod::app()->redis->sort('s')); // 1,2,3,4,5
    var_dump(Mod::app()->redis->sort('s', array('sort' => 'desc'))); // 5,4,3,2,1
    var_dump(Mod::app()->redis->sort('s', array('sort' => 'desc', 'store' => 'out'))); // (int)5
    // SISMEMBER  "1" 1是否在s里

}

~~~


## 4、hash


~~~

public static function RedisH(){
    
    Mod::app()->redis->delete('h');
    Mod::app()->redis->hSet('h', 'key1', 'hello'); /* 1, 'key1' => 'hello' in the hash at "h" */
    Mod::app()->redis->hGet('h', 'key1'); /* returns "hello" */

    Mod::app()->redis->hSet('h', 'key1', 'plop'); /* 0, value was replaced. */
    Mod::app()->redis->hGet('h', 'key1'); /* returns "plop" */
    
}
~~~
## 5 list
~~~
public static function RedisList(){
    /* Non blocking feature */
    Mod::app()->redis->lPush('key1', 'A');
    Mod::app()->redis->delete('key2');

    Mod::app()->redis->blPop('key1', 'key2', 10); /* array('key1', 'A') */
    /* OR */
    Mod::app()->redis->blPop(array('key1', 'key2'), 10); /* array('key1', 'A') */

    Mod::app()->redis->brPop('key1', 'key2', 10); /* array('key1', 'A') */
    /* OR */
    Mod::app()->redis->brPop(array('key1', 'key2'), 10); /* array('key1', 'A') */

    /* Blocking feature */

    /* process 1 */
    Mod::app()->redis->delete('key1');
    Mod::app()->redis->blPop('key1', 10);
    /* blocking for 10 seconds */

    /* process 2 */
    Mod::app()->redis->lPush('key1', 'A');

    /* process 1 */
    /* array('key1', 'A') is returned*/
}
~~~


# vim
##  1、
### 1.1 、替换
~~~
 :%s/this*/that/g
 :%s/\\"//g
~~~
~~~
# 删除空格 换行 回车

:%s/\s//g


:%s/\r//g


:%s/\n//g

~~~

### 1.2 
~~~

vimdiff a.txt b.txt

:set enc=utf8   
:set encoding=utf-8

~~~

##  2、grep  awk


~~~

grep 'patternOne' your.log |awk '{print $12"  " $8 "  "$5}' | sort -rn|head -n 12

~~~

# git
## 1 、 config 
~~~
# git 现有的config lists
git config --list 

git config --global user.name "yourName"
git config --global user.email "yourEmail@gmai.com"
 
# add 

git add .

# commit 

git commit -m 'comments'


~~~

~~~

# stash

git stash   # 切换到别的分支之前 先执行 git stash 保存到缓存区

git stash list  #查看所有 

git stash pop    # 取出最近的一次 stash

git stash apply stash@{X}    # 取出想要的stash
 
~~~


~~~
# reset  


git  reset --hard 'versionXXX'  # 彻底退回到某个版本，本地代码的修改一起没了
git  reset --soft 'versionXXX'  # 回到某个版本，commit 撤回了，本地代码的修改还在

git reset --hard HEAD^
git reset --soft HEAD^


~~~

# clean code 
## 原文摘自：https://github.com/php-cpm/clean-code-php


## 1、 命名
#### 1.1、变量命名
 
~~~
#Bad
$ymdstr = $moment->format('y-m-d');
--------------------------------------------------------
#good
$currentDate = $moment->format('y-m-d');

~~~

#### 1.2、方法命名
~~~

同一个实体要用相同的变量名
#Bad
getUserInfo();
getUserData();
getUserRecord();
getUserProfile();
--------------------------------------------------------
#Good
getUser();

~~~

#### 1.3、使用定义好的常量

~~~

#Bad
//谁知道4是啥？
if ($user->access & 4) {
// ...
}
 --------------------------------------------------------
#Good
class User
{
const ACCESS_READ = 1;
const ACCESS_CREATE = 2;
const ACCESS_UPDATE = 4;
const ACCESS_DELETE = 8;
}

if ($user->access & User::ACCESS_UPDATE) {
// do edit ...
}

~~~
#### 1.4 、无意义的变量名
~~~
#Bad
$l = ['Austin', 'New York', 'San Francisco'];

for ($i = 0; $i < count($l); $i++) {
    $li = $l[$i];
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
  //  `$li` 代表什么来着?
    dispatch($li);
}
--------------------------------------------------------
#Good
$locations = ['Austin', 'New York', 'San Francisco'];

foreach ($locations as $location) {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    dispatch($location);
}
~~~
####  1.5 不必要的上下文
~~~
#Bad
class Car
{
    public $carMake;
    public $carModel;
    public $carColor;

    //...
}
--------------------------------------------------------
#Good
class Car
{
    public $make;
    public $model;
    public $color;

    //...
}
~~~
 
## 2、简化写
#### 2.1 太多的 if else
~~~

#Bad
function isShopOpen($day): bool
{
    if ($day) {
        if (is_string($day)) {
            $day = strtolower($day);
            if ($day === 'friday') {
                return true;
            } elseif ($day === 'saturday') {
                return true;
            } elseif ($day === 'sunday') {
                return true;
            } else {
                return false;
            }
        } else {
            return false;
        }
    } else {
        return false;
    }
}
--------------------------------------------------------
#Good
function isShopOpen(string $day): bool
{
    if (empty($day)) {
        return false;
    }

    $openingDays = [
        'friday', 'saturday', 'sunday'
    ];

    return in_array(strtolower($day), $openingDays, true);
}
~~~
~~~
#Bad
function fibonacci(int $n)
{
    if ($n < 50) {
        if ($n !== 0) {
            if ($n !== 1) {
                return fibonacci($n - 1) + fibonacci($n - 2);
            } else {
                return 1;
            }
        } else {
            return 0;
        }
    } else {
        return 'Not supported';
    }
}
--------------------------------------------------------
#Good

function fibonacci(int $n): int
{
    if ($n === 0 || $n === 1) {
        return $n;
    }

    if ($n > 50) {
        throw new \Exception('Not supported');
    }

    return fibonacci($n - 1) + fibonacci($n - 2);
}

~~~

## 3、方法
## 3.1、参数最好少于3个
~~~
#Bad
function createMenu(string $title, string $body, string $buttonText, bool $cancellable): void
{
    // ...
}
--------------------------------------------------------
#Good
class MenuConfig
{
    public $title;
    public $body;
    public $buttonText;
    public $cancellable = false;
}

$config = new MenuConfig();
$config->title = 'Foo';
$config->body = 'Bar';
$config->buttonText = 'Baz';
$config->cancellable = true;

function createMenu(MenuConfig $config): void
{
    // ...
}
~~~

## 3.2、函数应该只做一件事
~~~
#Bad
function emailClients(array $clients): void
{
    foreach ($clients as $client) {
        $clientRecord = $db->find($client);
        if ($clientRecord->isActive()) {
            email($client);
        }
    }
}
--------------------------------------------------------
#Good
function emailClients(array $clients): void
{
    $activeClients = activeClients($clients);
    array_walk($activeClients, 'email');
}

function activeClients(array $clients): array
{
    return array_filter($clients, 'isClientActive');
}

function isClientActive(int $client): bool
{
    $clientRecord = $db->find($client);

    return $clientRecord->isActive();
}

~~~
 
## 4、面试
### 常问内容

~~~
1、介绍下自己
2、说下上家公司
3、为什么离职
4、职业规划
5、用过哪些框架
6、项目中遇到的问题，怎么解决的
~~~


### 常用http状态码

~~~
2**成功，操作被成功接收并处理

3**重定向，需要进一步的操作以完成请求

4**客户端错误，请求包含语法错误或无法完成请求

5**服务器错误，服务器在处理请求的过程中发生了错误

301 Moved Permanently永久移动
302 Found 临时移动
304 Not Modified 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源

400 Bad Request客户端请求的语法错误，服务器无法理解
401 Unauthorized请求要求用户的身份认证
403 Forbidden服务器理解请求客户端的请求，但是拒绝执行此请求 
404 Not Found 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面

500 Internal Server Error服务器内部错误，无法完成请求
502 Bad Gateway作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应
504Gateway Time-out充当网关或代理的服务器，未及时从远端服务器获取请求

~~~

### php 相关

~~~
你常用到的array方法



~~~


### mysql 相关

~~~
mysql引擎有哪些 

mysql 锁有哪些，怎么加锁

mysql conf 里哪些配置项可以提高性能？

~~~
###  linux 相关

~~~
安装
服务器查看php安装到哪里了

用到的linux命令 

根据日志筛选 都有哪些ip访问过我们的命令？（前五？）



~~~

###  项目相关
~~~
做过哪些项目，介绍下
项目中有没有遇到安全攻击，介绍下
项目记的log，介绍下
项目中要设置时区时间的话怎么设置

~~~

### 缓存
~~~
用过哪些缓存？

redis数据类型，以及你实际应用他们的场景？

~~~

### 版本控制
~~~
用过哪些版本管理工具？

git 流程？ 常用哪些git 命令？

~~~

### 

~~~
现有多个无序的网络请求需要同时发起，你会怎么实现
php muti curl 怎么实现的？

表数据有五千万的数据量，用户想导出到TXT，怎么写导出接口

php怎么执行命令，如果执行不成功，可能是因为什么，如何允许php执行命令

让你写一个上传接口，需要注意什么？怎么保证安全？（都会校验什么）

php 写 api 接口，你都会注意什么

定位到了 mysql 查询 怎么优化，

让你负责一个新项目，技术架构业务架构怎么考虑？

不知道php安装位置，想查看php配置？

自己实现的一个缓存，运行长时间后，现需要删除一些，如何做保证删除后缓存命中率比较高？

业务需求，需要生产不能重复的不限位数 的ID，自己怎么实现一个不会重复出现的ID？

mysql注入？为什么会导致mysql注入？

知道的常用的设计模式？

PHP的垃圾收集机制是怎样的？


不知道请求的客户端，不知道请求方式，怎么获取变量名？

现在服务器的架构是什么样子


高并发,高流量情况下,如何设计秒杀或抢红包架构？

技术架构、业务架构？

~~~
 

## 5、运行模式
### PHP 运行模式
原文地址：https://segmentfault.com/a/1190000014913877
##### 目前常见的4种PHP运行模式

~~~
1.  CGI通用网关接口模式
2.  FAST-CGI模式
3.  CLI命令行模式
4.  模块模式

~~~

**CGI通用网关接口模式**

~~~
每有一个用户请求，都会先要创建cgi的子进程，然后处理请求，处理完后结束这个子进程
比较老大多已经不用了

~~~
**FAST-CGI模式** 

~~~
它可以一直执行着，只要激活后，不会每次都要花费时间去fork 一次，
在CGI模式中，可以想象 CGI通常有多慢。每一个Web请求PHP都必须重新解析php.ini、重新载入全部dll扩展并重初始化全部数据结构。使用FastCGI，所有这些都只在进程启动时发生一次。一个额外的好处是，持续数据库连接(Persistent database connection)可以工作。

~~~
**CLI命令行模式**
**模块模式**
~~~
1.  Apache + mod_php
2.  lighttp + spawn-fcgi
3.  nginx + PHP-FPM

  
~~~
## 6、网络协议
原文地址：
# 常见网络协议
常见的网络协议有：  
**1TCP/IP**
~~~
传输控制协议/Internet协议  

传输控制协议TCP：好比货物装箱单，保证数据在传输过程中不会丢失
网络协议IP:好比收发货人的地址和姓名，保证数据到达指定的地点。  

TCP/IP协议族中包括上百个互为关联的协议，  
其中有：Telnet（Remote Login）： 提供远程登录功能；  
FTP （FileTransfer Protocol)：远程文件传输协议，允许用户将远程主机上的文件拷贝到自己的计算机上；  
SMTP （Simple Messagetransfer Protocol)：简单信息传输协议，用于传输电子邮件；  
NFS(Network File Server)：网络文件服务器，可使多台计算机透明地访问彼此的目录；  
UDP （ User DatagramProtocol)：用户数据包协议。  
~~~
  
**2ARP地址解析协议**

~~~
用于映射计算机的物理地址和临时指定的网络地址。  

~~~
  
**3.HTTP超文本传输协议**

~~~
HTTPS安全超文本传输协议，用于对数据进行压缩和解压操作，并返回网络上传送回的结果。


~~~

**4.跟邮件相关的协议** 
~~~

**SMTP** 简单邮件传输协议。

**IMAP**交互式邮件存取协议，

~~~

## 7、linux
## php-fpm 
~~~
/usr/local/php/sbin/php-fpm start 
/etc/init.d/php-fpm restart

service php-fpm restart

查看php-fpm进程数：
ps aux | grep -c php-fpm

ps aux |grep php
ps aux | grep php-fpm

~~~

## nginx命令 
~~~
start： 
/usr/local/nginx/sbin/nginx 

stop： 
/usr/local/nginx/sbin/nginx -s stop 

reload： 
/usr/local/nginx/sbin/nginx -s reload

service nginx restart

~~~

## mysql命令 
~~~
start： 
/etc/init.d/mysqld start 

stop： 
/etc/init.d/mysqld stop 

restart： 
/etc/init.d/mysqld restart
~~~



 ## linux 后台执行命令
~~~

nohup /root/test.php &
~~~

## 定时任务
~~~

crontab -e   #编辑定时任务
crontab -l   #定时任务列表

3,15 * * * * myCommand  #每小时第三分钟、第十五分钟执行
* */1 * * * myCommand  #每1小时执行一次
~~~

## 磁盘
~~~

df  -h   # 当前目录
df -h /usr/ #指定目录
du  -max-depth=1  -h # 查看当前目录每个文件夹的情况
du --max-depth=1  -h /usr/ #制定目录
~~~
 
 
 ## 负载
~~~

top 
uptime
vmstat
~~~

## log
~~~
#根据log统计访问前十的ip地址
awk '{print $1}' access.log | sort| uniq -c | sort  -nr | head  -n 10

cat access.log |  grep "07/Nov/2013" |  awk '{print $1}' |   uniq -c |  sort -nr |  head -n 10

~~~
## 8、文件上传
###  1、修改配置文件的上传大小 php.ini   
  
~~~
post_max_size = 200M  
upload_max_filesize = 200M   

~~~
### 2、 文件类型校验， 

~~~

黑名单或白名单，
mime-type检查 
文件头检查

~~~

### 3、 文件名清洗
~~~
防止各种注入攻击、XSS。  

~~~

### 4、 检查文件大小。
~~~
防止DoS攻击。  

~~~

### 5、上传文件目录的访问控制授权

~~~

上传的文件要保存在Web服务器的http不能访问到，但PHP可以读出来的路径，
或者干脆保存在内网另一台服务器上，
而下载/使用的时候单独用一个PHP来读，
向浏览器返回真实文件名（这样要支持分块下载就有点麻烦了）。
同时要保证这个PHP、机器上php版本没有可以利用文件操作来执行任意代码的漏洞。


在linux下要设置文件权限，
上传的文件如图片文档什么的要设置成没有执行的权限，这样会安全很多
~~~



 ~~~
最安全的做法是，强制后缀名，而不要使用用户上传文件的后缀名，
比如先判断下是不是jpg后缀，
是则强制保存文件名后缀为jpg，
不是的话禁止上传。
不要使用mime-type进行检查，可以很轻松绕过的。

~~~

## 9、架构

~~~
（摘自sf等网友的回答https://segmentfault.com/q/1010000008661892）

架构包含的方面很广，小到代码的布局，大到网络的规划，项目的配合等等。

使用了哪些技术，mysql的读写分离，预防高并发，redis，
比如linux计划任务，比如开发的前后端分离等等，都属于架构范畴

网站采用什么架构大致：  
1.硬件方面  
2.系统方面，linux还是windows  
3.环境方面，比如是apache还是nginx,php的版本，数据库的选择等  
4.数据库方面，数据库的类型，数据库的引擎表结构的设计等  
5.代码方面，框架的选择等等

~~~

## LNMP
~~~
lnmp(linux nginx mysql php) 
~~~
 
