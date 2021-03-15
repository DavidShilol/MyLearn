# MIME协议

在早期的消息传输中，仅仅是互相传输文本信息，但后来人们不再满足于此，希望消息中能传输图片、声音、附件等二进制信息。随着邮件的数据内容越来越丰富，就需要一个标准用以描述不同的数据——MIME协议（Multiplepurpose Internet MailExtension）。

MIME协议<u>**定义了复杂邮件体的格式**</u>。一条消息如果应用了MIME协议，那就是一条MIME类型的消息。

正如*HTTP/HTTP报文.md*中的HTTP报文示例中描述的：

```
Content-Disposition: form-data; name="detect_type"

1
```

```
Content-Disposition: form-data; name="image" filename="a.jpg"
Content-Type: image/jpeg

# 省略二进制字节流
```

POST表单中的每一项数据都有一些**实体头**去描述资源信息，这些头字段都在MIME协议中做了定义。

### MIME消息的头字段

1. Content-Type：定义内容的类型
2. Content-Disposition：指定处理数据内容的方式
3. Content-Transfer-Encoding：指定消息体内容所采用的编码方式

*Tips*

*Content-Disposition说的是数据的表示结构。如form-data的实质就是键值对；inline表示一个value；attachment表示value为一个附件，如果浏览器接收到的报文是Content-Disposition: attachment，就会直接提示保存或下载。*

*Content-Transfer-Encoding说的是传输这个消息体所使用的编码方式，类似于Python3向文本文件写入信息：`open(path, 'w', encoding='utf-8')`。*

### MIME编码方式

根据[MIME协议详解By淹死的鱼pp][1]一文归纳总结

传统的SMTP协议是基于ASCII设计的，因此消息传输不能出现中文和二进制文件，在传统SMTP协议中传输的消息体<u>内容不需要经过任何编码</u>，就叫**7bit编码**。(ASCII的一个字符占8bit，最高位永远为0，有效数据只有7bit)

但消息中传递二进制文件和中文是普遍的需求，后来人们找到一个方案，将一组连续的字节数据按6bit分为一组，每一组6bit数据都用一个ASCII字符表示。6bit的数据需要$2^6=64$个字符表示，它们是`ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/`，像这样<u>需要把消息体内容中字节数据进行编码</u>的方式，称为**BASE64**。还有一种常用的字节数据编码的方式叫做**Quted-printable**。

后来还出现了扩展的SMTP协议，允许消息体直接放入非ASCII字符而无须编码，像这样<u>无须对消息体内容中字节数据进行编码的方式</u>叫做**8bit**。

[1]: https://blog.csdn.net/qq_34227896/article/details/80326121

