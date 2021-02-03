* `base64`隐写
```python
 #!/usr/bin/env python
import re
path = './_.txt' ####
b64char = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'
with open(path, 'r')as f:
	cipher = [i.strip() for i in f.readlines()]
plaintext = ''
for i in cipher:
	if i[-2] == '=':  # There are 4-bit hidden info while end with two '='
		bin_message = bin(b64char.index(i[-3]))[2:].zfill(4)
		plaintext += bin_message[-4:]
	elif i[-1] == '=':  # There are 2-bit hidden info while end with one '='
		bin_message = bin(b64char.index(i[-2]))[2:].zfill(2)
		plaintext += bin_message[-2:]
plaintext = re.findall('.{8}', plaintext)  # 8bits/group
plaintext = ''.join([chr(int(i,2)) for i in plaintext])
print(plaintext)
 ```
