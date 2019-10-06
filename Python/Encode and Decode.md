# Encode and Decode

**编码格式**

计算机中，存储的信息都有一定的编码格式，比如汉字‘我’，在计算机眼里，存的并>不是‘我’，而是二进制的一串数字，而且不同编码，使用的二进制数和位长都不相同。

常用的编码格式有UTF8、GBK、GB2312等，一般设置为UTF8，支持绝大部分的中文字符。

**Python2中的类型——str与unicode**

前面我们说，字符有多种编码格式，比如UTF8和GBK。但不同编码格式之间，如何相互转换呢？这就需要一个转换的桥梁，需要一个翻译官的角色为双方翻译。

unicode就是这个翻译官，它能转换成任意编码格式的str。

在Python中，GBK转换成UTF8，需要先将GBK编码格式的str类型转换成unicode类型，>然后将unicode类型再转换成UTF8编码格式的str类型。

**Python2**

在python2中，字符串有两种类型：str和unicode，程序中默认使用str类型。

**赋值**

```python
s = '你好'
u = u'你好'
```

**str与unicode转换**

decode: str -> unicode

encode: unicode -> str

```python
ss = s.decode('utf-8') # ss是unicode类型
uu = u.encode('utf-8') # uu是str类型
```

**Python3**

python3的字符序列有byte和str两种类型，字符串默认是unicode编码。

字符串统一使用unicode编码，不需要再像python2那样麻烦的转换编码。