## 题目
[题目链接](https://adworld.xctf.org.cn/media/task/attachments/f590c0f99c014b01a5ab8b611b46c57c.txt)
* 在整个txt文本中，不难发现一些字母是故意大写，不妨先按顺序提取全部的大写字母
* 发现都是`ZERO`和`ONE`,之后又可能是培根，也有可能是转`ASCAll`
* 最后发现转ASCAll即可
* `python`脚本
```python
import re
s = open("f590c0f99c014b01a5ab8b611b46c57c.txt", 'r')
str = s.read()
f = ''
for i in str:
    if i.isupper():
        f += i
print(f)
flag = ''
for i in f:
    if i == 'Z':
        flag += '0'
        continue
    if i == 'N':
        flag += '1'
        continue
    else:
        continue
flag = re.findall(r'.{8}',flag)
res = ''
for i in flag:
    res += chr(int(i,2))
print(res)
# BITSCTF{h1d3_1n_pl41n_5173}
```
