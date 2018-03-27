### 同步IO、异步IO
同步IO：在发出一个功能调用时，在没有得到结果之前，该调用不返回。即：CPU在等待该程序执行。
异步IO：在发出一个调用指令后，CPU不等待结果而去做其他事情。调用指令完成后通知CPU，CPU再处理。
#### 读写文件
```python
try:
    f = open('/path/to/file', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```
等同于：
```python
# 读取
with open('/path/to/file', 'r') as f:
    f.read()
# 写(覆盖原文件)
with open('/Users/michael/test.txt', 'w') as f:
    f.write('Hello, world!')
# 追加(不直接覆盖)
with open('/Users/michael/test.txt', '') as f:
    f.write('Hello, world!')
```
读取方式：
- read() 一次性读取全部文件内容
- readline() 每次读取一行内容
- readlines() 一次读取所有内容并按行返回list
```python
for line in readlines():
    print(line.strip())  # 把末尾的'\n'删掉
```
打开方式：
- open('file', 'r') 以文本文件的方式打开（默认utf-8编码）
- open('file', 'rb') 以二进制方式打开文件（如图片、视频等）
- open('file', 'r', encoding='gbk') 读取以其它方式编码的文件
- open('file', 'r', encoding='gbk', errors='ignore') 忽略文件中的非法编码字符
#### StringIO与BytesIO
在内存中读写string或二进制数据
f = StringIO()
f.write('hello world')

f = BytesIO()
f.write('你好'.encode('utf-8'))
### 异步IO
#### 协程Coroutine
协程不是线程或进程，而是类似于子进程，但不返回值。
协程的执行效率极高，因为执行时不需要线程切换，另外，不需要线程锁。
利用多进程+协程，可以充分利用多核，获得极高效率。
python通过generator实现协程:
```python
def consumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[CONSUMER] Consuming %s...' % n)
        r = '200 OK'
        
def produce(c):
    c.send(None)
    n = 0
    while n < 5:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s' % r)
    c.close()

c = consumer()
produce(c)
```
consumer是一个生成器，produce先用send(None)启动生成器，通过send(n)切换到consumer执行，yield把结果传回，下一循环。循环执行完毕，c.close()关闭consumer，结束。
#### asyncio
用asyncio提供的asyncio.coroutine可以将一个generator标记为一个coroutine类型，然后内部用yield from调用另一个coroutine来实现异步操作。
```python
import asyncio


@asyncio.coroutine
def wget(host):
    print('wget %s...' % host)
    connect = asyncio.open_connection(host, 80)
    reader, writer = yield from connect
    header = 'GET / HTTP/1.0\r\nHost: %s\r\n\r\n' % host
    writer.write(header.encode('utf-8'))
    yield from writer.drain()
    while True:
        line = yield from reader.readline()
        if line == b'\r\n':
            break
        print('%s header > %s' % (host, line.decode('utf-8').rstrip()))
    writer.close()

loop = asyncio.get_event_loop()
tasks = [wget(host) for host in ['www.sina.com.cn', 'www.sohu.com', 'www.163.com']]
loop.run_until_complete(asyncio.wait(tasks))
loop.close()
```
##### asynic/await
asynic/await是针对coroutine的新语法：
- 把@asyncio.coroutine替换为async；
- 把yield from替换为await。
```python
import asyncio

@asyncio.coroutine
def hello():
    print("Hello world!")
    # 异步调用asyncio.sleep(1):
    r = yield from asyncio.sleep(1)
    print("Hello again!")

# 获取EventLoop:
loop = asyncio.get_event_loop()
# 执行coroutine
loop.run_until_complete(hello())
loop.close()

替换如下项目：

async def hello():
    print("Hello world!")
    r = await asyncio.sleep(1)
    print("Hello again!")
```
##### aiohttp
aiohttp是基于asynico的HTTP框架。
