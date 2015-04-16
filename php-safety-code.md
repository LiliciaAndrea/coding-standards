
## 前言

本文档的目标是协助编写安全的 PHP 代码，降低出现 Web 漏洞的风险。

虽然本文档是针对 PHP 设计的，但是在使用 Python、ASP.NET、Java 等语言开发 Web 后台时，也可适当参考本文档的约定。


## 1 过滤用户输入

#### 需要假设用户的所有输入都是有害的，对于用户可控的变量必须根据情况做过滤。

### 1.1 SQL injection 防护

#### 1.1.1 使用框架或 `PDO`

数据库操作推荐使用国外成熟框架，如 `Laravel`、`CodeIgniter` 、`YII` 等。通过 `PDO` 预编译 SQL 语句也是一个好办法。

#### 1.1.2 过滤参数

当必须使用 SQL 语句拼接时，使用 `intval/floatval` 对数值型参数过滤，使用 `mysql_real_escape_string` 对字符串型参数进行过滤，

#### 1.1.3 保护参数

对于数值型和字符串型参数都需要用 `'` 包裹进行保护。

#### 1.1.4 统一字符集编码 

使用 `mysql_set_charset` 设置当前数据库字符集与前端页面字符集一致，建议全部字符集统一为 `UTF-8`。


### 1.2 XSS（跨站脚本攻击）防护

#### 1.2.1 使用 `htmlspecialchars`

对于将要输出在前端页面的变量，必须使用 `htmlspecialchars` 进行过滤。

#### 1.2.2 检查 ` URL`

攻击者可能会构造 `伪协议` 实施攻击：
```html
<a href="javascript:alert(1);">test</a>
```
还可以使用 `dataURL`、`vbscript` 等伪协议：
```html
<a href="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTs8L3NjcmlwdD4=">test</a>
```
上面这段代码是指用 test/html 的格式加载编码为 base64 的数据，加载完成后实际上是：
```html
<script>alert(1);</script>
```
或者通过 `闭合引号` 引入 `事件`：
```html
<a href="x" onclick="alert(1)">test</a>
```
**解决方法：**检查变量是否以 `http` 开头，再对变量进行 `urlencode`。

### 1.3 文件包含/任意文件下载防护

#### 1.3.1 过滤 `.` 和 `/`

攻击者会尝试使用 `../../etc/passwd` 之类的系统 file 路径寻找文件包含或者任意下载漏洞。

#### 1.3.2 配置 `open_basedir`

open_basedir 可以用来限制 PHP 可读写的文件目录。


## 2 文件上传

#### 文件上传功能是最容易出现安全问题的弱点之一，建议直接使用公开漏洞较少的富文本编辑器，如 Ueditor ，而不是自己编写上传类。

### 2.1 使用白名单检查文件后缀

黑名单检测往往容易被绕过，应使用 `strrpos` 来截取后缀名判断其是否在白名单中。
```php
	$allowedType = array('jpg', 'gif', 'bmp', 'png', 'jpeg');
	$extpos = strrpos($fileName, '.');
	$ext = substr($filename, $extpos + 1);
	if(!in_array($ext, $allowedType)){
		return false;
	}
```

### 2.2 文件名处理

#### 2.2.1 检查文件名

如果不能重命名文件，应该对文件名进行 `过滤`，过滤方法请参考上文。

#### 2.2.2 重命名

尽量对文件重命名，并且不要返回给用户重命名后的文件名。
建议使用 `日期` + `随机字符串` 的形式重命名文件。

### 2.3 配置

#### 2.3.1 取消上传目录的执行权限

万一上传被突破，`取消执行权限` 可以防止恶意脚本的执行。

#### 2.3.2 使用最新版本的 Web Server
旧版本的 Web Server 如 IIS 6 、Nginx 0.8 等存在高危解析漏洞，应及时升级 Web Server 并按时打补丁。


## 3 其他

#### 代码编写中应注意的一些逻辑问题等。

### 3.1 CSRF（跨站请求伪造）

#### 3.1.1 检查请求来源

检查 HTTP 请求头中的 `Referer` 是否来源于本域名。

##### 3.1.2 在表单中增加 `token` 字段

在每个表单中增加一个 `一次性` 且 `随机` 的 token 隐藏字段，提交时与用户 `session` 中的 token 进行比对。

### 3.2 变量覆盖

对于每个变量应尽可能初始化，在 php.ini 中关闭全局变量注册 `register_global=Off`。

### 3.3 任意命令执行

避免使用 `eval`、`system` 等调用系统命令的函数。


## 4 参考资料

> https://www.owasp.org/index.php/PHP_Security_Cheat_Sheet  PHP_Security_Cheat_Sheet

> http://drops.wooyun.org/papers/4544  论PHP常见的漏洞

> http://drops.wooyun.org/papers/155  CSRF简单介绍及利用方法

> http://zone.wooyun.org/content/1910  PHP上传绕过及缺陷经验解说

> http://zone.wooyun.org/content/1872  PHP变量覆盖经验解说
