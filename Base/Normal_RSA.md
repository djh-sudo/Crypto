## 题目
题目给了[两个文件](https://adworld.xctf.org.cn/media/task/attachments/547de1d50b95473184cd5bf59b019ae8.rar)
* 一个是flag.enc,一个是pubkey.pem
这里其实需要我们从公钥中破解得到私钥。
* 1.可以用现有的工具RsaCtfTool
```python3 RsaCtfTool.py --publickey pubkey.pem  --uncipherfile flag.enc```

![image](https://github.com/djh-sudo/Crypto/blob/main/RSA.png)

* 2.另外一种方法会详细一点，是第一种的展开版本
我们这里就以它为例，首先公钥包含了```(e,n)```两个参数，RSA的安全性基于大整数```n```难分解，如果```n```被分解了，私钥```d```自然就容易求，这里我们可以自己尝试分解```n```
* ```PEM```格式公钥提取两个参数```(e,n)```
利用openssl提取
```openssl rsa -pubin -in pubkey.pem -noout -text```
提取结果可以得到
```
e = 65537
n = 87924348264132406875276140514499937145050893665602592992418171647042491658461
```
这里看到```n```并不大，很容易分解
* 分解方法1 [在线分解](http://factordb.com/)
* 分解方法2 利用```yahu```f分解
分解可以得到
```
p = 275127860351348928173285174381581152299
q = 319576316814478949870590164193048041239
```
这里私钥```d```其实可以求出来了，之后的解密做法，既可以是恢复出私钥的pem文件，然后用```openssl```解密，也可以把```flag.enc```二进制打开，然后解密
这里我们选择了后者
* 具体```python```脚本如下
```
import gmpy2
from Crypto.Util.number import *
e = 65537
n = 87924348264132406875276140514499937145050893665602592992418171647042491658461
p = 275127860351348928173285174381581152299
q = 319576316814478949870590164193048041239
d = gmpy2.invert(e, (p-1)*(q-1))
c = 0x6D3EB7DF23EEE1D38710BEBA78A0878E0E9C65BD3D08496DDA64924199110C79
m = pow(c, d, n)
print(long_to_bytes(m))
# 可以看到打印的字符和第一种截图一样
# PCTF{256b_i5_m3dium}
```
