# URL解析

字符串 → URL编码加密：
- escape：对字符转义为十六进制的转义序列除了：**ASCII 数字字母**、**@/+-*.**
- encodeURI：对字符转义为十六进制的转义序列除了：**ASCII 数字字母**、**@/+-*.~!#$&()=:,;?'** 、
- encodeURIComponent：对字符转义为十六进制的转义序列除了：**ASCII 数字字母**、**-*.()!~** 、

URL编码 → 字符串解密：
- unescape
- decodeURI
- decodeURI



## escape、encodeURI、encodeURIComponent 的区别

- encodeURI 是对整个 URI 进行转义，将 URI 中的非法字符转换为合法字符，所以对于一些在 URI 中有特殊意义的字符不会进行转义。
- encodeURIComponent 是对 URI 的组成部分进行转义，所以一些特殊字符也会得到转义。
- escape 和 encodeURI 的作用相同，不过它们对于 unicode 编码为 0xff 之外字符的时候会有区别，escape 是直接在字符的 unicode 编码前加上 %u，而 encodeURI 首先会将字符转换为 UTF-8 的格式，再在每个字节前加上 %。

