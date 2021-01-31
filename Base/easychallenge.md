## 题目
题目给了一个[pyc文件](https://adworld.xctf.org.cn/media/task/attachments/42aa1a89e3ae48c38e8b713051557020.pyc)

* pyc文件是python的可执行文件，目的是节省python解析器的翻译时间，可以提高运行效率.
* 但由于pyc是字节码，其实是二进制文件，可以被逆向出源码(当然不是自己逆向)，这里可以使用专门的工具  
(在目录里面找 Easy Python Decompiler v1.3.2.7z)。
* 下面是逆向后的代码
```
def encode1(ans):
    s = ''
    for i in ans:
        x = ord(i) ^ 36
        x = x + 25
        s += chr(x)

    return s

def encode2(ans):
    s = ''
    for i in ans:
        x = ord(i) + 36
        x = x ^ 36
        s += chr(x)

    return s

def encode3(ans):
    return base64.b32encode(ans)

flag = ' '
print 'Please Input your flag:'
flag = raw_input()
final = 'UC7KOWVXWVNKNIC2XCXKHKK2W5NLBKNOUOSK3LNNVWW3E==='
if encode3(encode2(encode1(flag))) == final:
    print 'correct'
else:
    print 'wrong'
```
* 这里简单写一些逆函数即可
```
def decode1(ans):
    s = ''
    for i in ans:
        i = chr(ord(i) - 25)
        x = ord(i) ^ 36
        s += chr(x)

    return s

def decode2(ans):
    s = ''
    for i in ans:
        i = chr(ord(i) ^ 36)
        x = ord(i) - 36
        s += chr(x)

    return s

s = 'UC7KOWVXWVNKNIC2XCXKHKK2W5NLBKNOUOSK3LNNVWW3E==='
s = base64.b32decode(s)
# 解码后的结果是
s = '\xa0\xbe\xa7Z\xb7\xb5Z\xa6\xa0Z\xb8\xae\xa3\xa9Z\xb7Z\xb0\xa9\xae\xa3\xa4\xad\xad\xad\xad\xad\xb2'
s = decode1(decode2(s))
print(s)
# cyberpeace{interestinghhhhh}
```
最后的结果就是```cyberpeace{interestinghhhhh}```
