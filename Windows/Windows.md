# 系统
## 命令
### 跳转（cd）
#### 在同一盘符内跳转
  cd直接跟一个目录名称
```shell
cd Desktop # 相对路径
cd C:\Users\jinlongguo\Desktop # 绝对路径
```

#### 跨盘符跳转
  有两种方法：
* 先切换盘符，再跳转目录
```shell
D:
cd D:\Software
```

* cd后加”/d“选项，一步到位
```shell
cd /d D:\Software
```

# 编程
## 线程同步
|临界区|互斥器|信号量|
|---|---|---|
|CRITICAL_SECTION cs|CreateMutex()|<br>HANDLE hSemaphore;</br>hSemaphore = CreateSemaphore(NULL, 1, 1, NULL)|
|InitializeCriticalSection(&cs)|OpenMutex()|OpenSemaphore()|
|EnterCriticalSection(&cs)|<br>WaitForSingleObject()</br><br>WaitForMultipleObjects()</br><br>MsgWaitForSingleObjects()</br>|<br>WaitForSingleObject(hSemaphore, INFINITE)</br><br>WaitForMultipleObjects()</br><br>MsgWaitForSingleObjects()</br>|
|LeaveCriticalSection(&cs)|ReleaseMutex()|ReleaseSemaphore((hSemaphore, 1, NULL)|
|DeleteCriticalSection(&cs)|CloseHandle()|CloseHandle()|

# 参考
* [windows核心编程-信号量(semaphore)](https://www.bbsmax.com/A/D8547b73JE/)
* [windows下临界区的使用](https://blog.csdn.net/s651665496/article/details/47045485)
* [windows信号量使用](https://blog.csdn.net/wangweitingaabbcc/article/details/6833265)