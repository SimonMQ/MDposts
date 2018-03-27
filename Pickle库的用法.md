# Pickle标注库的用法
- Pickle可以将纯python对象存储到一个文件中，并稍后取回。此方法适用于python语言内部。
#### 便于存储
序列化过程是将文本信息（如字符串、列表、字典等）转变为二进制流，保存在硬盘的文件中。
#### 便于运输
当两个进程远程通信时，彼此可以发送各种类型的数据。无论何种类型的数据都可以转换成二进制在网上传输。
### 序列化pickling
- pickle.dump()
该方法是将对象obj序列化，直接以二进制的形式写进文件中，进行保存。
- pickle.dumps()
该方法是将对象序列化成一个bytes，以便下一步的保存。
```python
import pickle
shoplist = ['a', 'b', 'c']
with open('shopfile.data', 'wb') as f:
    pickle.dump(shoplist, f)
    
```
Pickler(file, protocol).dump(obj)
### 反序列化unpickling
- pickle.load()
该方法是直接将文件中对象以二进制数据的形式反序列化出来。
- pickle.loads()
我们将一个文件读到一个bytes后，用该方法反序列化出对象。
```python
import pickle
with open('shopfile.data, 'rb') as f:
    storedlist = pickle.load(f)
```
Unpickler(file).load()
# JSON
如果在不同语言间传递对象，就必须把对象序列化为标准格式，如XML、JSON等。
JSON表示的对象是标准的JavaScript语言的对象。
python的json模块可以将一个对象转换为JSON格式。
```python
>>> import json
>>> d = dict(name='Bob', age=20, score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```
注意：dumps()方法返回一个str。类似的有json.dump()方法直接写入file。
如果将一个class的实例转换为JSON对象，那么需要在CLASS中定义一个方法，将其属性先转换为dict类型(可序列化的JSON对象)，然后再写入文件。
json.dumps(s, default=stu2dict)
json.dumps提供一个ensure_ascii参数，当其为True时返回的数据类型为ascii码；当其为False时，返回的就不是强制的ascii码。
