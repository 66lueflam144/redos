好legend
>由于 Cookie “user”缺少正确的“sameSite”属性值，缺少“SameSite”或含有无效值的 Cookie 即将被视作指定为“Lax”，该 Cookie 将无法发送至第三方上下文中。若您的应用程序依赖这组 Cookie 以在不同上下文中工作，请添加“SameSite=None”属性。若要了解“SameSite”属性的更多信息，请参阅：https://developer.mozilla.org/docs/Web/HTTP/Headers/Set-Cookie/SameSite

翻来覆去找不到POST抓包文件，直接修改Method为POST也没什么反应，好心寒。
再打开一个，好绝望，输入进去没有回显[BJDCTF2020]Cookie is so stable
1，好心寒

```HTML
<!--
	~~~post money and password~~~
if (isset($_POST['password'])) {
	$password = $_POST['password'];
	if (is_numeric($password)) {
		echo "password can't be number</br>";
	}elseif ($password == 404) {
		echo "Password Right!</br>";
	}
}
-->
```

