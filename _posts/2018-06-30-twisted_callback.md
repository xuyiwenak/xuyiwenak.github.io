---
layout: post
title: 2018-06-30-twisted_callback.md
date: 2018-06-30 18:40:06
tag: 基础
---
# 概念理解
在传统的IO（BIO/阻塞IO）中，所有IO操作都会阻塞当前线程，直到操作完成，所有步骤都是一步一步进行。例如：
```
OutputStream output = socket.getOutputstream();
out.write(data); // IO操作完成后返回
out.flush();
System.out.println("消息发送完成");
```
但是在异步网络编程中，由于IO操作是异步的，也就是一个IO操作不会阻塞去等待操作结果，程序就会继续向下执行。

在MINA、Netty、Twisted中，很多网络IO操作都是异步的，比如向网络的另一端write写数据、客户端连接服务器的connect操作等。

例如twisted的write方法，执行完write语句后并不表示数据已经发送出去，而仅仅是开始发送这个数据，程序并不阻塞等待发送完成，而是继续往下执行。所以如果有某些操作想在write完成后再执行，例如write完成后关闭连接，下面这些写法就有问题了，它可能会在数据write出去之前先关闭连接：
```
Channel ch = ...;
ch.writeAndFlush(message);
ch.close();
```
那么问题就来了，既然上面的写法不正确，那么如何在IO操作完成后再做一些其他操作？

当异步IO操作完成后，无论成功或者失败，都会再通知程序。至于如何处理发送完成的通知，在MINA、Netty、Twisted中，都会有类似回调函数的实现方式。
Twisted
在Twisted中，twisted.internet.defer.Deferred类似于MINA、Netty中的Future，可以用于添加在异步IO完成后的回调函数。但是Twisted并没有在write方法中返回Deffered，虽然write方法也是异步的。下面就用Twisted实现一个简单的TCP客户端来学习Deffered的用法。
```
# -*- coding:utf-8 –*-

from twisted.internet import reactor
from twisted.internet.protocol import Protocol
from twisted.internet.endpoints import TCP4ClientEndpoint, connectProtocol

class ClientProtocol(Protocol):
    def sendMessage(self):
        self.transport.write("Message") # write也是异步的
        self.transport.loseConnection() # loseConnection会等待write完成后再关闭连接

# 连接服务器成功的回调函数
def connectSuccess(p):
    print "connectSuccess"
    p.sendMessage()

# 连接服务器失败的回调函数
def connectFail(failure):
    print "connectFail"

point = TCP4ClientEndpoint(reactor, "localhost", 8080)
d = connectProtocol(point, ClientProtocol()) # 异步连接到服务器，返回Deffered
d.addCallback(connectSuccess) # 设置连接成功后的回调函数
d.addErrback(connectFail) # 设置连接失败后的回调函数
reactor.run()
```
在Twisted中，关闭连接的loseConnection方法会等待异步的write完成后再关闭，类似于MINA中的session.close(false)。Twisted中想要直接关闭连接可以使用abortConnection，类似MINA中的session.close(true)。
