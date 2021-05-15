# 生产器yield

## 外部传递数据

​	我们可以通过Generarot::send方法来向生产器中传入一个值。传入的这个值将会被当做生成器当前yield的放回值。然后我们根据这个值做一些操作，比如根据外部条件中断

```php
function test1() {
    for ($i = 0; $i < 10; $i++) {
        $data = (yield $i + 1);
        if ($data == 'stop') {
            retrun;
        }
    }
}
$t1 = test1();
foreach ($t1 as $t) {
    if ($t == 3) {
        $t1->send('stop');
    }
    echo $t, PHP_EOL;
}
```

注意：把yield的值赋值给变量yield需要用括号引用。

# 链接

https://blog.csdn.net/weixin_39540725/article/details/115156034

# 命令

启动内置服务器

cd 项目目录

```bash
php -S localhost:8080
```

# array

## array_filter去除数组空字符

使用php数组函数array_filter去除数组中的空字符元素

```php
<?php
$str1_array=array('奇葩天地网','','//www.qipa250.com','','qipa250','');
$str1_array=array_filter($str1_array);
print_r($str1_array);
Array
(
[0] => 奇葩天地网
[2] => //www.qipa250.com
[4] => qipa250
) 