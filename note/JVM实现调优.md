

# JVM自带的调优参数

## 基本命令

### jps - 获取当前操作系统中的所有java相关的进程号

![[ae797dd344035a29554302a5b0049785.png]]

### jmap - 此命令可以用来查看内存信息，实例个数以及占用内存大小

1. jmap -histo 『进程号』 ：查看所有的实例数量信息
   ![[36c4c3fd94a88d91251374f4092cb03f.png]]
   打开文件为下图
   ![[08793d2050f9dd5f84de01472e1f7305.png]]
- num：序号
- instances：实例数量
- bytes：占用空间大小
- class name：类名称，`[C is a char[]，[S is a short[]，[I is a int[]，[B is a byte[]，[[I is a int[][]`

2. jmap - heap 『进程号』 ：查看当前堆信息
   ![[72946f61850dcc44ad6545b9aeaf299b.png]]

这个命令还可以将堆内存的信息输出为当前时间的快照
> jmap -dump:format=b,file=『名称』.hprof 『进程号』
> 其中文件输出格式可以为hprof和dump格式，java的分析工具jvisualvm支持这两种类型的导入
> 也可以在出现内存溢出时自己导出溢出时的内存快照（内存很大时，可能会导不出来）
> 1. -XX:+HeapDumpOnOutOfMemoryError
> 2. -XX:HeapDumpPath=『路径』/『文件名』.dump(hprof)
> 3. 可以用jvisualvm命令工具导入该dump文件分析

```java
// 测试代码
public class OOMTest {  
    public static List<Object> list = new ArrayList<>();
    // JVM设置
    // -Xms10M -Xmx10M -XX:+PrintGCDetails -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=D:\jvm.dump
    public static void main(String[] args) {
        List<Object> list = new ArrayList<>();
        int i = 0;
        int j = 0;
        while (true) {
            list.add(new User(i++, UUID.randomUUID().toString()));
            new User(j--, UUID.randomUUID().toString());
        }
    } 
}
```

### Jstack - 用jstack加进程id查找死锁，见如下示例

```java
// 测试代码，一个简单的死锁程序
/* 输出为：
thread1 begin
main thread end
thread2 begin
可以看到，main主线程已经结束了，但是两个子线程还没有结束，表明已经被死锁了，除非手动结束，否则程序就一直死锁
*/
public class DeadLockTest { 
    private static Object lock1 = new Object();
    private static Object lock2 = new Object();
    public static void main(String[] args) {
        new Thread(() -> {
            synchronized (lock1) {
                try {
                    System.out.println("thread1 begin");
                    Thread.sleep(5000);
                } catch (InterruptedException e) {
                
                } synchronized (lock2) {
                    System.out.println("thread1 end");
                }
            }
        }).start();
        new Thread(() -> {
            synchronized (lock2) {
                try {
                    System.out.println("thread2 begin");
                    Thread.sleep(5000);
                } catch (InterruptedException e) {
                
                } synchronized (lock1) {
                    System.out.println("thread2 end");
                }
            }
        }).start();
        System.out.println("main thread end");
    } 
}
```

使用jstack 『进程号』

![[c072a6bfa5495eeb24549ced4644e881.png]]

"Thread-1" 线程名
prio=5 优先级=5
tid=0x000000001fa9e000 线程id
nid=0x2d64 线程对应的本地线程标识nid
java.lang.Thread.State: BLOCKED 线程状态
**还可以用jvisualvm自动检测死锁**

#### jstack找出占用cpu最高的线程堆栈信息

```java
package com.example.jvm; 
/** * 运行此代码，cpu会飙高 */ 
public class Math { 
    public static final int initData = 666;
    public static User user = new User();
    public int compute() {
        //一个方法对应一块栈帧内存区域
        int a = 1;
        int b = 2;
        int c = (a + b) * 10;
        return c;
    }
    
    
    public static void main(String[] args) {
        Math math = new Math();
        while (true){
            math.compute();
        }
    } 
}
```

1. 使用命令top -p ，显示你的java进程的内存情况，pid是你的java进程号，比如19663![[clipboard[6].png]]
2. 按H，获取每个线程的内存情况![[clipboard (1)[1].png]]
3. 找到内存和cpu占用最高的线程tid，比如19664
4. 转为十六进制得到 0x4cd0，此为线程id的十六进制表示
5. 执行 jstack 19663|grep -A 10 4cd0，得到线程堆栈信息中 4cd0 这个线程所在行的后面10行，从堆栈中可以发现导致cpu飙高的调用方法![[clipboard[7].png]]
6. 查看对应的堆栈信息找出可能存在问题的代码

### Jinfo - 查看正在运行的Java应用程序的扩展参数

查看正在运行的Java应用程序的扩展参数
查看jvm的参数
![[clipboard (1)[2].png]]

查看java系统参数
![[clipboard[8] 1.png]]

### Jstat - jstat命令可以查看堆内存各部分的使用量，以及加载类的数量。

命令的格式如下：`jstat [-命令选项] [vmid] [间隔时间(毫秒)] [查询次数]`
注意：使用的jdk版本是jdk8

#### 垃圾回收统计

**jstat -gc pid** 最常用，可以评估程序内存使用及GC压力整体情况
![[clipboard[9].png]]
- S0C：第一个幸存区的大小，单位KB
- S1C：第二个幸存区的大小
- S0U：第一个幸存区的使用大小
- S1U：第二个幸存区的使用大小
- EC：伊甸园区的大小
- EU：伊甸园区的使用大小
- OC：老年代大小
- OU：老年代使用大小
- MC：方法区大小(元空间)
- MU：方法区使用大小
- CCSC:压缩类空间大小
- CCSU:压缩类空间使用大小
- YGC：年轻代垃圾回收次数
- YGCT：年轻代垃圾回收消耗时间，单位s
- FGC：老年代垃圾回收次数
- FGCT：老年代垃圾回收消耗时间，单位s
- GCT：垃圾回收消耗总时间，单位s

#### 堆内存统计

jstat -gccapacity 『进程ID』
![[clipboard[10].png]]
- NGCMN：新生代最小容量
- NGCMX：新生代最大容量
- NGC：当前新生代容量
- S0C：第一个幸存区大小
- S1C：第二个幸存区的大小
- EC：伊甸园区的大小
- OGCMN：老年代最小容量
- OGCMX：老年代最大容量
- OGC：当前老年代大小
- OC:当前老年代大小
- MCMN:最小元数据容量
- MCMX：最大元数据容量
- MC：当前元数据空间大小
- CCSMN：最小压缩类空间大小
- CCSMX：最大压缩类空间大小
- CCSC：当前压缩类空间大小
- YGC：年轻代gc次数
- FGC：老年代GC次数

#### 新生代垃圾回收统计

jstat -gcnew 『pid』
![[clipboard (1)[3].png]]
- S0C：第一个幸存区的大小
- S1C：第二个幸存区的大小
- S0U：第一个幸存区的使用大小
- S1U：第二个幸存区的使用大小
- TT:对象在新生代存活的次数
- MTT:对象在新生代存活的最大次数
- DSS:期望的幸存区大小
- EC：伊甸园区的大小
- EU：伊甸园区的使用大小
- YGC：年轻代垃圾回收次数
- YGCT：年轻代垃圾回收消耗时间

#### 新生代内存统计

jstat -gcnewcapacity 『pid』
![[clipboard (2)[1].png]]
- NGCMN：新生代最小容量
- NGCMX：新生代最大容量
- NGC：当前新生代容量
- S0CMX：最大幸存1区大小
- S0C：当前幸存1区大小
- S1CMX：最大幸存2区大小
- S1C：当前幸存2区大小
- ECMX：最大伊甸园区大小
- EC：当前伊甸园区大小
- YGC：年轻代垃圾回收次数
- FGC：老年代回收次数

#### 老年代垃圾回收统计

jstat -gcold 『p』
![[clipboard (3) 1.png]]
- MC：方法区大小
- MU：方法区使用大小
- CCSC:压缩类空间大小
- CCSU:压缩类空间使用大小
- OC：老年代大小
- OU：老年代使用大小
- YGC：年轻代垃圾回收次数
- FGC：老年代垃圾回收次数
- FGCT：老年代垃圾回收消耗时间
- GCT：垃圾回收消耗总时间

#### 老年代内存统计

jstat -gcoldcapacity 『pid』
![[clipboard[11].png]]
- OGCMN：老年代最小容量
- OGCMX：老年代最大容量
- OGC：当前老年代大小
- OC：老年代大小
- YGC：年轻代垃圾回收次数
- FGC：老年代垃圾回收次数
- FGCT：老年代垃圾回收消耗时间
- GCT：垃圾回收消耗总时间

#### 元数据空间统计

jstat -gcmetacapacity 『pid』
![[clipboard (2)[2].png]]
- MCMN:最小元数据容量
- MCMX：最大元数据容量
- MC：当前元数据空间大小
- CCSMN：最小压缩类空间大小
- CCSMX：最大压缩类空间大小
- CCSC：当前压缩类空间大小
- YGC：年轻代垃圾回收次数
- FGC：老年代垃圾回收次数
- FGCT：老年代垃圾回收消耗时间
- GCT：垃圾回收消耗总时间

#### 总结垃圾回收统计

jstat -gcutil 『pid』
![[clipboard (1)[4].png]]
- S0：幸存1区当前使用比例
- S1：幸存2区当前使用比例
- E：伊甸园区使用比例
- O：老年代使用比例
- M：元数据区使用比例
- CCS：压缩使用比例
- YGC：年轻代垃圾回收次数
- FGC：老年代垃圾回收次数
- FGCT：老年代垃圾回收消耗时间
- GCT：垃圾回收消耗总时间

## JVM运行情况预估

用 jstat gc -pid 命令可以计算出如下一些关键数据，有了这些数据就可以采用之前介绍过的优化思路，先给自己的系统设置一些初始性的JVM参数，比如堆内存大小，年轻代大小，Eden和Survivor的比例，老年代的大小，大对象的阈值，大龄对象进入老年代的阈值等。

**年轻代对象增长的速率**
可以执行命令 jstat -gc pid 1000 10 (每隔1秒执行1次命令，共执行10次)，通过观察EU(eden区的使用)来估算每秒eden大概新增多少对象，如果系统负载不高，可以把频率1秒换成1分钟，甚至10分钟来观察整体情况。注意，一般系统可能有高峰期和日常期，所以需要在不同的时间分别估算不同情况下对象增长速率。

**Young GC的触发频率和每次耗时**
知道年轻代对象增长速率我们就能推根据eden区的大小推算出Young GC大概多久触发一次，Young GC的平均耗时可以通过 YGCT/YGC 公式算出，根据结果我们大概就能知道系统大概多久会因为Young GC的执行而卡顿多久。

**每次Young GC后有多少对象存活和进入老年代**
这个因为之前已经大概知道Young GC的频率，假设是每5分钟一次，那么可以执行命令 jstat -gc pid 300000 10 ，观察每次结果eden，survivor和老年代使用的变化情况，在每次gc后eden区使用一般会大幅减少，survivor和老年代都有可能增长，这些增长的对象就是每次Young GC后存活的对象，同时还可以看出每次Young GC后进去老年代大概多少对象，从而可以推算出老年代对象增长速率。

**Full GC的触发频率和每次耗时**
知道了老年代对象的增长速率就可以推算出Full GC的触发频率了，Full GC的每次耗时可以用公式 FGCT/FGC 计算得出。

**优化思路**其实简单来说就是尽量让每次Young GC后的存活对象小于Survivor区域的50%，都留存在年轻代里。尽量别让对象进入老年代。尽量减少Full GC的频率，避免频繁Full GC对JVM性能的影响。
