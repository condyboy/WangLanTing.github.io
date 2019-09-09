---
title: 7-29日报 python线程
date: 2019-07-29 10:27:29
categories: python
tags: python
---
# 7-29日报 python线程
## 线程与进程
多任务可以由多进程完成，也可以由一个进程内的多线程完成。
我们前面提到了进程是由若干线程组成的，一个进程至少有一个线程。
Python的标准库提供了两个模块：_thread和threading，_thread是低级模块，threading是高级模块，对_thread进行了封装。绝大多数情况下，我们只需要使用threading这个高级模块。
由于任何进程默认就会启动一个线程，我们把该线程称为主线程，主线程又可以启动新的线程，Python的threading模块有个current_thread()函数，它永远返回当前线程的实例。主线程实例的名字叫MainThread，子线程的名字在创建时指定，我们用LoopThread命名子线程。名字仅仅在打印时用来显示，完全没有其他意义，如果不起名字Python就自动给线程命名为Thread-1，Thread-2……
**启动一个线程就是把一个函数传入并创建Thread实例，然后调用start()开始执行：**
```
import threading,time

def loop():
        print("子线程{}开始运行".format(threading.current_thread().name))
        for i in range(5):
                print("子线程{}正在打印{}".format(threading.current_thread().name,i))
                time.sleep(0.5)

if __name__ == "__main__":
    print("thread {} is running..".format(threading.current_thread().name))
    t=threading.Thread(target=loop,name="wltThread")
    t.start()
    t.join()
    print("子线程运行结束")
```
## Lock
多线程和多进程最大的不同在于，多进程中，同一个变量，各自有一份拷贝存在于每个进程中，互不影响，而**多线程中，所有变量都由所有线程共享**，所以，任何一个变量都可以被任何一个线程修改，因此，线程之间共享数据最大的危险在于多个线程同时改一个变量，赋值会出现顺序错误，导致计算结果出现问题，此时要用到锁`threading.Lock()`，保证在修改公共变量时其他线程不能够同时修改。
当多个线程同时执行`lock.acquire()`时，只有一个线程能成功地获取锁，然后继续执行代码，其他线程就继续等待直到获得锁为止。获得锁的线程用完后一定要释放锁，否则那些苦苦等待锁的线程将永远等待下去，成为死线程。所以我们用`try...finally`来确保锁一定会被释放，并且多线程有可能会出现死锁现象
例子：
加锁之前，num运行混乱,采用**global关键字**声明在函数内可以对全局变量进行修改，否则是无法对全局变量进行修改的。
```
import threading,time,random

num=0
lock=threading.Lock()
def go(n):
        global num
        num=num+n
        time.sleep(random.random())
        num=num-n
        print("线程{}执行的结果为{}".format(threading.current_thread().name,num))
def loop(n):
        for temp in range(100):
                go(n)
                
        
if __name__ == "__main__":
    
    print("thread {} is running..".format(threading.current_thread().name))
    t=threading.Thread(target=loop,name="wltThread",args=(5,))
    t1=threading.Thread(target=loop,name="wltThread2",args=(8,))
    t.start()
    t1.start()
    t.join()
    t1.join()
    print("子线程运行结束")
```

加锁之后，num结果均为0
```
import threading,time,random

num=0
lock=threading.Lock()
def go(n):
        global num
        num=num+n
        time.sleep(random.random())
        num=num-n
        print("线程{}执行的结果为{}".format(threading.current_thread().name,num))
def loop(n):
        for temp in range(100):
                lock.acquire()
                try:
                        go(n)
                finally:
                        lock.release()
                
        
if __name__ == "__main__":
    
    print("thread {} is running..".format(threading.current_thread().name))
    t=threading.Thread(target=loop,name="wltThread",args=(5,))
    t1=threading.Thread(target=loop,name="wltThread2",args=(8,))
    t1.start()
    t.start()
    t1.join()
    t.join()
    print("子线程运行结束")
```

## 多核cpu
如果你不幸拥有一个多核CPU，你肯定在想，多核应该可以同时执行多个线程。
如果写一个死循环的话，会出现什么情况呢？
打开Mac OS X的Activity Monitor，或者Windows的Task Manager，都可以监控某个进程的CPU使用率。
我们可以监控到一个死循环线程会100%占用一个CPU。
如果有两个死循环线程，在多核CPU中，可以监控到会占用200%的CPU，也就是占用两个CPU核心。
要想把N核CPU的核心全部跑满，就必须启动N个死循环线程。
启动与CPU核心数量相同的N个线程，在4核CPU上可以监控到CPU占用率仅有102%，也就是仅使用了一核。

但是用C、C++或Java来改写相同的死循环，直接可以把全部核心跑满，4核就跑到400%，8核就跑到800%，为什么Python不行呢？

因为Python的线程虽然是真正的线程，但解释器执行代码时，有一个**GIL锁**：Global Interpreter Lock，任何Python线程执行前，必须先获得GIL锁，然后，每执行100条字节码，解释器就自动释放GIL锁，让别的线程有机会执行。这个GIL全局锁实际上把所有线程的执行代码都给上了锁，所以，多线程在Python中只能交替执行，即使100个线程跑在100核CPU上，也只能用到1个核。

GIL是Python解释器设计的历史遗留问题，通常我们用的解释器是官方实现的CPython，要真正利用多核，除非重写一个不带GIL的解释器。

所以，**在Python中**，**可以使用多线程，但不要指望能有效利用多核**。如果一定要通过多线程利用多核，那只能通过C扩展来实现，不过这样就失去了Python简单易用的特点。

不过，也不用过于担心，Python虽然不能利用多线程实现多核任务，但可以通过多进程实现多核任务。多个Python进程有各自独立的GIL锁，互不影响。
## 线程中的局部变量
每个线程调用的函数通过参数传递非常麻烦，所以采用`ThreadLocal`，它可以自动帮助你管理好线程中的局部变量，使用`threading.local()`
```
import threading,time,random

#全局变量的访问需要加锁，所以需要采用线程内的局部变量，threading.local()保证线程内局部变量互不影响
temp=threading.local()
def getName():
        thisname=temp.name
        print("子线程{}中的局部变量的值为{}".format(threading.current_thread().name,thisname))
def initThread(name):
        #通过temp来管理线程内的局部变量
        temp.name=name
        getName()
                
        
if __name__ == "__main__":
    
    print("thread {} is running..".format(threading.current_thread().name))
    t=threading.Thread(target=initThread,name="wltThread1",args=("wlt",))
    t1=threading.Thread(target=initThread,name="wltThread2",args=("zyd",))
    t.start()
    t1.start()
    t.join()
    t1.join()
    print("子线程运行结束")
```
执行结果：

```
thread MainThread is running..
子线程wltThread1中的局部变量的值为wlt
子线程wltThread2中的局部变量的值为zyd
子线程运行结束
```
可以通过构造全局变量temp(名字自己取),并在线程中构造自己需要存储的属性，temp.name（name名自选），并可以存取
一个ThreadLocal变量虽然是全局变量，但每个线程都只能读写自己线程的独立副本，互不干扰。ThreadLocal解决了参数在一个线程中各个函数之间互相传递的问题。
## 进程与线程
实现多任务，通常我们会设计Master-Worker模式，Master负责分配任务，Worker负责执行任务，因此，多任务环境下，通常是一个Master，多个Worker。
如果用多进程实现Master-Worker，主进程就是Master，其他进程就是Worker。
如果用多线程实现Master-Worker，主线程就是Master，其他线程就是Worker。
多进程模式最大的优点就是稳定性高，因为一个子进程崩溃了，不会影响主进程和其他子进程。（当然主进程挂了所有进程就全挂了，但是Master进程只负责分配任务，挂掉的概率低）著名的Apache最早就是采用多进程模式。
多进程模式的缺点是创建进程的代价大，在Unix/Linux系统下，用fork调用还行，在Windows下创建进程开销巨大。另外，操作系统能同时运行的进程数也是有限的，在内存和CPU的限制下，如果有几千个进程同时运行，操作系统连调度都会成问题。
多线程模式通常比多进程快一点，但是也快不到哪去，而且，多线程模式致命的缺点就是任何一个线程挂掉都可能直接造成整个进程崩溃，因为所有线程共享进程的内存。在Windows上，如果一个线程执行的代码出了问题，你经常可以看到这样的提示：“该程序执行了非法操作，即将关闭”，其实往往是某个线程出了问题，但是操作系统会强制结束整个进程。
## 计算密集型与I/O密集型
是否采用多任务的第二个考虑是任务的类型。我们可以把任务分为计算密集型和IO密集型。
计算密集型任务的特点是要进行大量的计算，消耗CPU资源，比如计算圆周率、对视频进行高清解码等等，全靠CPU的运算能力。这种计算密集型任务虽然也可以用多任务完成，但是任务越多，花在任务切换的时间就越多，CPU执行任务的效率就越低，所以，要最高效地利用CPU，计算密集型任务同时进行的数量应当等于CPU的核心数。
计算密集型任务由于主要消耗CPU资源，因此，代码运行效率至关重要。Python这样的脚本语言运行效率很低，完全不适合计算密集型任务。对于计算密集型任务，最好用C语言编写。
第二种任务的类型是IO密集型，涉及到网络、磁盘IO的任务都是IO密集型任务，这类任务的特点是CPU消耗很少，任务的大部分时间都在等待IO操作完成（因为IO的速度远远低于CPU和内存的速度）。对于IO密集型任务，任务越多，CPU效率越高，但也有一个限度。常见的大部分任务都是IO密集型任务，比如Web应用。
IO密集型任务执行期间，99%的时间都花在IO上，花在CPU上的时间很少，因此，用运行速度极快的C语言替换用Python这样运行速度极低的脚本语言，完全无法提升运行效率。对于IO密集型任务，最合适的语言就是开发效率最高（代码量最少）的语言，脚本语言是首选，C语言最差。
## 分布式进程
在Thread和Process中，应当优选Process，因为Process更稳定，而且，Process可以分布到多台机器上，而Thread最多只能分布到同一台机器的多个CPU上。
Python的multiprocessing模块不但支持多进程，其中managers子模块还支持把多进程分布到多台机器上。一个服务进程可以作为调度者，将任务分布到其他多个进程中，依靠网络通信。由于managers模块封装很好，不必了解网络通信的细节，就可以很容易地编写分布式多进程程序。
举个例子：如果我们已经有一个通过Queue通信的多进程程序在同一台机器上运行，现在，由于处理任务的进程任务繁重，希望把发送任务的进程和处理任务的进程分布到两台机器上。怎么用分布式进程实现？
原有的Queue可以继续使用，但是，通过managers模块把Queue通过网络暴露出去，就可以让其他机器的进程访问Queue了。
我们先看服务进程，服务进程负责启动Queue，把Queue注册到网络上，然后往Queue里面写入任务：
参考[这里](https://www.liaoxuefeng.com/wiki/1016959663602400/1017631559645600)