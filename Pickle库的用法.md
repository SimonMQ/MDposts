# Pickle标注库的用法
- Pickle可以将纯python对象存储到一个文件中，并稍后取回。
#### 便于存储
序列化过程是将文本信息（如字符串、列表、字典等）转变为二进制流，保存在硬盘的文件中。
#### 便于运输
当两个进程远程通信时，彼此可以发送各种类型的数据。无论何种类型的数据都可以转换成二进制在网上传输。
### 序列化
pickle.dump()
该方法是将序列化后的对象obj以二进制的形式写进文件中，进行保存。
```python
import pickle
shoplist = ['a', 'b', 'c']
with open('shopfile.data', 'wb') as f:
    pickle.dump(shoplist, f)
    
```
Pickler(file, protocol).dump(obj)
### 反序列化
pickle.load()
该方法是将文件中序列化的对象以二进制读取出来。
```python
import pickle
with open('shopfile.data, 'rb') as f:
    storedlist = pickle.load(f)
```
Unpickler(file).load()
