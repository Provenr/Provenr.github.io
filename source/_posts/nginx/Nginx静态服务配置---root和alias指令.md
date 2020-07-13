## root 指令

```
location / {
    root   /root/provenr-local/nginx;
    index  index.html index.htm;
}

```
- 对于匹配后的url地址，将匹配的location中的root路径替换访问url的host即得到文件的真实地址。（多个斜杠其实等价于一个斜杠）
- 如果不匹配location，则寻找更外层的root做替换。
- root指令最后的斜杠可加可不加。

## alias 指令
alias这个词在计算机里很常用，字面意思是“别名”
```
location /blog {
    alias  /root/provenr-local/blog;
    index  index.html index.htm;
}
```

访问 https://www.provenr.cn/blog/test.html 时:
在服务器 /root/provenr-local/blog 目录下 寻找 test.html 文件

/blog 是 目录（/root/provenr-local/blog）的别名

⚠️：注意 多斜杠的情况

#### 1、 alias指令路径加上斜杠
```
location /blog {
    alias  /root/provenr-local/blog/;
    index  index.html index.htm;
}

==>

/root/provenr-local/blog/ + /test.html
```
这里多的斜杠是合法的。等价于一个斜杠的情况。

#### 2、 location 指令路径加上斜杠
```
location /blog/ {
    alias  /root/provenr-local/blog;
    index  index.html index.htm;
}

==>

/root/provenr-local/blog + test.html
```
这里把url 拆分为 ==/blog/ + test.html==
替换后的uri 为 ==/root/provenr-local/blogtest.html==， 这是非法的