# 简介
  Linux下的多线程接口称为pthread，遵循POSIX标准，使用pthread编写多线程程序时，需要包含pthread.h，链接时需要调用libpthread.a(eg. gcc -lpthread)。

# API
## 数据类型
* **pthread_t**
  线程句柄，使用pthread_self获取线程自身ID，使用pthread_equal()比较两个线程的ID。
  在"/usr/include/bits/pthreadtypes.h"中定义：
```C
typedef unsigned int pthread_t;
```

* **pthread_attr_t**
  线程属性，无法直接修改值，只能使用线程属性管理函数修改。
  在"/usr/include/bits/pthreadtypes.h"中定义：
```C
union pthread_attr_t
{
  char __size[__SIZEOF_PTHREAD_ATTR_T];
  long int __align;
};
#ifndef __have_pthread_attr_t
typedef union pthread_attr_t pthread_attr_t;
# define __have_pthread_attr_t 1
#endif
```

|属性|说明|
|-|-|
|detachstate|表示线程是否与进程中其他线程脱离同步：<br>PTHREAD_CREATE_JOINABLE：可以使用pthread_join()来与其他线程同步</br><br>PTHREAD_CREATE_DETACHED：新线程不能使用pthread_join()来与其他线程同步，无法切换到PTHREAD_CREATE_JOINABLE状态。</br><br>运行时可以通过pthread_attr_setdetachstate()来设置，默认值为PTHREAD_CREATE_JOINABLE。</br>|
|guardsize||
|sched_param|和线程调度相关的参数结构体，目前有且仅有一个sched_priority整形变量表示线程的运行优先级，仅在调度策略为实时时有效。<br>运行时可以通过pthread_attr_setschedparam()来改变，缺省值为0，表示与父进程相同的优先级。</br>|
|schedpolicy|线程调度策略：<br>SCHED_OTHER：正常（非实时）</br><br>SCHED_RR：轮转（实时）</br><br>SCHED_FIFO：先入先出（实时）</br><br>运行时可以通过pthread_attr_setschedpolicy()来改变，缺省值为SCHED_OTHER，后两种调度策略仅对超级用户有效。|
|inheritsched|PTHREAD_EXPLICIT_SCHED：使用显示制定调度策略和调度参数；</br><br>PTHREAD_INHERIT_SCHED：继承调用者线程的值。</br><br>缺省值为PTHREAD_EXPLICIT_SCHED。</br>|
|scope|绑定类型，表示线程间竞争CPU的范围：<br>PTHREAD_SCOPE_SYSTEM：与系统中所有线程一起竞争CPU时间（绑定）；</br><br>PTHREAD_SCOPE_PROCESS：有同进程中的线程竞争CPU时间（非绑定）。</br><br>通过pthread_attr_setscope()设置。</br>|
|stackaddr|堆栈地址|
|stacksize|堆栈大小，默认值为1M。|

## 函数
### 线程管理
* **pthread_create**
  创建新的线程。
  在"/usr/include/pthread.h"中声明：
```C
extern int pthread_create (pthread_t *__restrict __newthread,
			   const pthread_attr_t *__restrict __attr,
			   void *(*__start_routine) (void *),
			   void *__restrict __arg) __THROWNL __nonnull ((1, 3));
```
|参数|说明|
|-|-|
|__newthread|线程ID指针|
|__attr|线程属性指针，为空（NULL）时使用默认属性|
|__start_routine|线程运行函数指针|
|__arg|线程运行函数的参数指针，为空（NULL）时表示没有参数|
|int|返回值： <br>0：线程创建成功</br><br>EAGAIN：系统限制创建新的线程，如线程过多</br><br>EINVAL：线程属性非法</br>|

* **pthread_join**
  线程阻塞函数，当前线程阻塞到被等待的线程结束为止。
  在"/usr/include/pthread.h"中声明：
```C
extern int pthread_join (pthread_t __th, void **__thread_return);
```
|参数|说明|
|-|-|
|__th|被等待线程的ID|
|__thread_return|被等待线程的返回值|

* **pthread_exit**
  结束当前线程。
  在"/usr/include/pthread.h"中声明：
```C
extern void pthread_exit (void *__retval) __attribute__ ((__noreturn__));
```
|参数|说明|
|-|-|
|__retval|指定的线程返回值的指针|

### 线程属性
  在"/usr/include/pthread.h"中声明：
```C
/* Thread attribute handling.  */

/* Initialize thread attribute *ATTR with default attributes
   (detachstate is PTHREAD_JOINABLE, scheduling policy is SCHED_OTHER,
    no user-provided stack).  */
extern int pthread_attr_init (pthread_attr_t *__attr) __THROW __nonnull ((1));

/* Destroy thread attribute *ATTR.  */
extern int pthread_attr_destroy (pthread_attr_t *__attr)
     __THROW __nonnull ((1));

/* Get detach state attribute.  */
extern int pthread_attr_getdetachstate (const pthread_attr_t *__attr,
					int *__detachstate)
     __THROW __nonnull ((1, 2));

/* Set detach state attribute.  */
extern int pthread_attr_setdetachstate (pthread_attr_t *__attr,
					int __detachstate)
     __THROW __nonnull ((1));


/* Get the size of the guard area created for stack overflow protection.  */
extern int pthread_attr_getguardsize (const pthread_attr_t *__attr,
				      size_t *__guardsize)
     __THROW __nonnull ((1, 2));

/* Set the size of the guard area created for stack overflow protection.  */
extern int pthread_attr_setguardsize (pthread_attr_t *__attr,
				      size_t __guardsize)
     __THROW __nonnull ((1));


/* Return in *PARAM the scheduling parameters of *ATTR.  */
extern int pthread_attr_getschedparam (const pthread_attr_t *__restrict __attr,
				       struct sched_param *__restrict __param)
     __THROW __nonnull ((1, 2));

/* Set scheduling parameters (priority, etc) in *ATTR according to PARAM.  */
extern int pthread_attr_setschedparam (pthread_attr_t *__restrict __attr,
				       const struct sched_param *__restrict
				       __param) __THROW __nonnull ((1, 2));

/* Return in *POLICY the scheduling policy of *ATTR.  */
extern int pthread_attr_getschedpolicy (const pthread_attr_t *__restrict
					__attr, int *__restrict __policy)
     __THROW __nonnull ((1, 2));

/* Set scheduling policy in *ATTR according to POLICY.  */
extern int pthread_attr_setschedpolicy (pthread_attr_t *__attr, int __policy)
     __THROW __nonnull ((1));

/* Return in *INHERIT the scheduling inheritance mode of *ATTR.  */
extern int pthread_attr_getinheritsched (const pthread_attr_t *__restrict
					 __attr, int *__restrict __inherit)
     __THROW __nonnull ((1, 2));

/* Set scheduling inheritance mode in *ATTR according to INHERIT.  */
extern int pthread_attr_setinheritsched (pthread_attr_t *__attr,
					 int __inherit)
     __THROW __nonnull ((1));


/* Return in *SCOPE the scheduling contention scope of *ATTR.  */
extern int pthread_attr_getscope (const pthread_attr_t *__restrict __attr,
				  int *__restrict __scope)
     __THROW __nonnull ((1, 2));

/* Set scheduling contention scope in *ATTR according to SCOPE.  */
extern int pthread_attr_setscope (pthread_attr_t *__attr, int __scope)
     __THROW __nonnull ((1));

/* Return the previously set address for the stack.  */
extern int pthread_attr_getstackaddr (const pthread_attr_t *__restrict
				      __attr, void **__restrict __stackaddr)
     __THROW __nonnull ((1, 2)) __attribute_deprecated__;

/* Set the starting address of the stack of the thread to be created.
   Depending on whether the stack grows up or down the value must either
   be higher or lower than all the address in the memory block.  The
   minimal size of the block must be PTHREAD_STACK_MIN.  */
extern int pthread_attr_setstackaddr (pthread_attr_t *__attr,
				      void *__stackaddr)
     __THROW __nonnull ((1)) __attribute_deprecated__;

/* Return the currently used minimal stack size.  */
extern int pthread_attr_getstacksize (const pthread_attr_t *__restrict
				      __attr, size_t *__restrict __stacksize)
     __THROW __nonnull ((1, 2));

/* Add information about the minimum stack size needed for the thread
   to be started.  This size must never be less than PTHREAD_STACK_MIN
   and must also not exceed the system limits.  */
extern int pthread_attr_setstacksize (pthread_attr_t *__attr,
				      size_t __stacksize)
     __THROW __nonnull ((1));

#ifdef __USE_XOPEN2K
/* Return the previously set address for the stack.  */
extern int pthread_attr_getstack (const pthread_attr_t *__restrict __attr,
				  void **__restrict __stackaddr,
				  size_t *__restrict __stacksize)
     __THROW __nonnull ((1, 2, 3));

/* The following two interfaces are intended to replace the last two.  They
   require setting the address as well as the size since only setting the
   address will make the implementation on some architectures impossible.  */
extern int pthread_attr_setstack (pthread_attr_t *__attr, void *__stackaddr,
				  size_t __stacksize) __THROW __nonnull ((1));
#endif

#ifdef __USE_GNU
/* Thread created with attribute ATTR will be limited to run only on
   the processors represented in CPUSET.  */
extern int pthread_attr_setaffinity_np (pthread_attr_t *__attr,
					size_t __cpusetsize,
					const cpu_set_t *__cpuset)
     __THROW __nonnull ((1, 3));

/* Get bit set in CPUSET representing the processors threads created with
   ATTR can run on.  */
extern int pthread_attr_getaffinity_np (const pthread_attr_t *__attr,
					size_t __cpusetsize,
					cpu_set_t *__cpuset)
     __THROW __nonnull ((1, 3));

/* Get the default attributes used by pthread_create in this process.  */
extern int pthread_getattr_default_np (pthread_attr_t *__attr)
     __THROW __nonnull ((1));

/* Set the default attributes to be used by pthread_create in this
   process.  */
extern int pthread_setattr_default_np (const pthread_attr_t *__attr)
     __THROW __nonnull ((1));

/* Initialize thread attribute *ATTR with attributes corresponding to the
   already running thread TH.  It shall be called on uninitialized ATTR
   and destroyed with pthread_attr_destroy when no longer needed.  */
extern int pthread_getattr_np (pthread_t __th, pthread_attr_t *__attr)
     __THROW __nonnull ((2));
#endif
```

# 参考
* [linux的<pthread.h>](https://www.cnblogs.com/x_wukong/p/5671137.html)
* [谈标准Linux操作系统实时性的制约因素](https://developer.aliyun.com/article/599856)
* [Linux内核针对实时性的调优](http://howar.cn/2020/03/15/embedded-linux-real-time/)