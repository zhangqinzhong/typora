# NIO基础

non-blocking io 非阻塞IO

NIO支持**面向缓冲区的、基于通道的IO操作**。NIO将以更加高效的方式进行文件的读写操作。

## 1.1 IO与NIO的区别

| IO                      | NIO                         |
| ----------------------- | --------------------------- |
| 面向流(Stream Oriented) | 面向缓冲区(Buffer Oriented) |
| 阻塞IO(Blocking IO)     | 非阻塞IO(NonBlocking IO)    |
|                         | 选择器(Selectors)           |

常见的Channel有

* FileChannel    （处理文件）
* DatagramChannel  （处理UDP）
* SocketChannel      （TCP数据传输通道）（专用服务器）
* ServerSocketChannel     （TCP数据传输通道）（服务器客户端都能用）

buffer 则是用来缓冲读写数据的临时存放内存空间，常见的有

* byteBuffer
* * MappedByteBuffer
  * DirectByteBuffer
  * HeapByteBuffer
* ShortBuffer
* IntBuffer
* LongBuffer
* FloatBuffer
* DoubleBuffer
* CharBuffer

## 1.2 Selector