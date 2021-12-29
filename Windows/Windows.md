
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