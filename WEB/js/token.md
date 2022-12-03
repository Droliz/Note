# token

## Token一般是存放在哪里？ 

Token放在cookie和放在localStorage、sessionStorage中有什么不同？

## Token是什么？


Token 其实就是**访问资源的凭证**。

一般是用户通过用户名和密码登录成功之后，服务器将登录凭证做数字签名，加密之后得到的字符串作为token。


## Token存放位置

它在用户登录成功之后会返回给客户端，客户端主要以下几种存储方式：

- 1、存储在localStorage中，每次调用接口的时候都把它当成一个字段传给后台

- 2、存储在cookie中，让它自动发送，不过缺点就是不能跨域

- 3、拿到之后存储在localStorage中，每次调用接口的时候放在HTTP请求头的Authorization字段里面。

token 在客户端一般存放于localStorage、cookie、或sessionStorage中。


## Token 放在 cookie、localStorage、sessionStorage中的不同点？


### 将Token存储于localStorage或 sessionStorage

Web存储（localStorage/sessionStorage）可以通过同一域商Javascript访问。这意味着任何在你的网站上的运行的JavaScript都可以访问Web存储，所以容易受到XSS攻击。尤其是项目中用到了很多第三方JavaScript类库。

为了防止XSS，一般的处理是避开和编码所有不可信的数据。但这并不能百分百防止XSS。比如我们使用托管在CDN或者其它一些公共的JavaScript库，还有像npm这样的包管理器导入别人的代码到我们的应用程序中。

如果你使用的脚本中有一个被盗用了怎么办？恶意的JavaScript可以嵌入到页面上，并且Web存储被盗用。这些类型的XSS攻击可以得到每个人的Web存储来访问你的网站。

这也是为什么许多组织建议不要在Web存储中存储任何有价值或信任任何Web存储中的信息。 这包括会话标识符和令牌。作为一种存储机制，Web存储在传输过程中不强制执行任何安全标准。

XSS攻击：Cross-Site Scripting（跨站脚本攻击）简称XSS，是一种代码注入攻击。攻击者通过在目标网站注入恶意脚本，使之在用户的浏览器上运行。利用这些恶意脚本，攻击者可以获取用户的敏感信息如Cookie、SessionID等，进而危害数据安全。


### 将Token存储与cookie

优点：可以制定httponly，来防止被JavaScript读取，也可以制定secure，来保证token只在HTTPS下传输。

缺点：不符合Restful 最佳实践。 容易遭受CSRF攻击（可以在服务器端检查Refer和Origin）

CSRF:跨站请求伪造，简单的说，是攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站并运行一些操作（如：发邮件、发信息、甚至财产操作如转账和购买商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去运行。这利用了web中用户身份验证的一个漏洞：简单的身份验证职能保证请求发自某个用户的浏览器，却不能保证请求本身是用户自愿发出去的。CSRF并不能够拿到用户的任何信息，它只是欺骗用户浏览器，让其以用户的名义进行操作。

## 总结
关于token 存在cookie还是localStorage有两个观点：

支持Cookie的开发人员会强烈建议不要将敏感信息（例如JWT)存储在localStorage中，因为它对于XSS毫无抵抗力。

支持localStorage的一派则认为：撇开localStorage的各种优点不谈，如果做好适当的XSS防护，收益是远大于风险的。

放在cookie中看似看全，看似“解决”（因为仍然存在XSS的问题）一个问题，却引入了另一个问题（CSRF）
localStorage具有更灵活，更大空间，天然免疫 CSRF的特征。Cookie空间有限，而JWT一半都占用较多字节，而且有时你不止需要存储一个JWT。