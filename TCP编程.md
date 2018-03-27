### TCP/IP协议
TCP协议和IP协议简称TCP/IP协议。
IP协议负责把数据从一台计算机通过网络发送到另一台计算机，按块发送，途经多个路由，不保证到达或顺序到达。IP地址：IPv4和IPv6。
TCP协议建立在IP协议之上，连接可靠，顺序到达。一个TCP报文除了包含要传输的数据外，还包含源IP地址和目标IP地址，源端口和目标端口。
### TCP编程
Socket表示“打开一个网络链接”。
- 客户端
```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(('127.0.0.1', 9999))
print(s.recv(1024).decode('utf-8'))
for data in [b'Michael', b'Tracy', b'Sarah']:
	s.send(data)
	print(s.recv(1024).decode('utf-8'))
s.send(b'exit')
s.close
```
- 服务器
```python
import socket
import threading
import time

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 监听端口
s.bind(('127.0.0.1', 9999))
s.listen(5)  # 等待连接的最大数量
print('Waiting for connection...')


def tcplink(sock, addr):
    print('Accept new connection from %s:%s...' % addr)
    sock.send(b'Welcome!')
    while True:
        data = sock.recv(1024)
        time.sleep(1)
        if not data or data.decode('utf-8') == 'exit':
            break
        sock.send(('Hello, %s!' % data.decode('utf-8')).encode('utf-8'))
    sock.close()
    print('Connection from %s:%s closed.' % addr)


while True:
    sock, addr = s.accept()
    t = threading.Thread(target=tcplink, args=(sock, addr))
    t.start()
```
客户端主动连接服务器的IP和指定端口，服务器首先监听指定端口，对每一个新连接，创建一个线程或者进程来处理。并且，服务器总是一直运行下去。
