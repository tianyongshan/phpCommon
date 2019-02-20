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
 
 
