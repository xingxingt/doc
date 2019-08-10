

#### 开启线程的三种方式？
    继承Thread: 定义一个继承Thead类的子类，子类重写Thread的run方法，然后实例化子类创建出线程对象，然后调用线程对象的start()开启线程;    
    实现Runnable: 定义一个实现Runnable的实现类，实现类重写Runnable的run方法，然后实例出子类即线程对象，然后调用线程对象的start()开启线程;    
    使用Callable和Future: 定义一个实现Runnable的实现类，实现类重写Callable的call()方法,call()方法可以有返回值,创建FutureTask对象，  
    实例出实现类，用FutureTask对象包装实例出的实现类，FutureTask封装了Callable的call()的返回值;通过new Thread(future).start的方式启动线程;
   

#### 线程和进程的区别？
    1,进程是系统进行资源分配和调度的一个独立单元；  
    2,而线程是进程的实体，是cpu资源分配和调度的最小单元，线程本身不拥有资源，它只拥有一些运行时所需要的资源（寄存器，栈，程序计数器等）,  
      它可与同属同一个进程的其他线程共享进程所拥有的所有资源;  
    3,进程和线程的主要区别在于它们的操作系统资源管理方式不同，进程有独立的地址空间，一个进程死掉后在保护模式下不会影响其他进程，   
      而线程只是进程的不同执行路径;  
    4,线程有自己独立的堆栈和局部变量，没有单独的地址空间，一个线程死掉后就等于整个进程死掉，所以多进程的程序  
    比多线程的程序健壮，但是进程之间的切换，耗费的资源更大效率要差，而且要同时对共享变量操作是只能用线程，不能用进程；   
    简述：  
    1，一个程序至少要有一个进程，而一个进程至少要有一个线程;  
    2,线程的划分尺度要小于进程，使得多线程程序的并发性高；  
    3，进程在执行过程中有独立的内存单元，而多个线程共享内存，从而极大的提高了程序的运行效率;  
    4,每个独立的线程有程序运行的入口，顺序执行的序列和程序运行的出口,但是线程不能独立运行必须依赖于应用程序，由应用程序提供多个线程的执行控制;  
    5,从逻辑的角度来看，多线程的意义在于在一个应用程序中有多个执行部分可以同时执行，但是操作系统并没有将多个线程看做是多个独立运行的应用，来
       实现进程的的调度管理和资源分配(进程和线程的重要区别);  
    ref:https://blog.csdn.net/mxsgoden/article/details/8821936

#### 为什么要有线程，而不是仅仅用进程？
    什么是进程？   
    应用程序不能直接执行，它需要装载到内存中，系统为它分配资源，才能运行，而这种执行的程序称为进程；  
    程序和进程的区别？  
    程序是运行指令的集合，他是进程运行的静态描述文件，而进程是程序的一次执行活动，是动态概念;   
    进程的缺点:  
    1,进程在一时间只能做一件事，如果想同时多多件事就无能为力了；  
    2，进程在执行过程中如果遇到阻塞，例如输入等待，一旦阻塞，那么其他操作也无法进行，导致整个进程都要挂起;  
    线程的优点:  
    1，线程可以提高进程的并发度，可以同时执行多个任务；  
    2，线程可以很好的利用计算机多处理器和多核的特性，从而提高进程的运行效率；  
    ref:https://blog.csdn.net/tongxinhaonan/article/details/42558561

#### run()和start()方法区别
    start():start方法是用来启动线程的，实现多线程的运行的，start()方法执行后，无需等待run()方法里面的代码体执行完毕而直接运行下面的代码，  
            通过Thread的start()方法启动一个线程，这时线程并没有运行，而是处于一个可运行状态，一旦得到cpu的时间片，就开始运行run方法里面  
            的代码体，run()方法包含了这个线程要运行的内容，run()方法执行完毕，这个线程也就终止了;    
    run():run()方法只是一个普通的方法而已，如果直接运行run()方法，那么还是只有主线程一个线程而已，程序要顺序执行，还是要等run()方法运行  
          完毕后在执行下面的代码，不过这样没达到多线程的意义;  
    ref:https://blog.csdn.net/dada360778512/article/details/6965790

#### 如何控制某个方法允许并发访问线程的个数？  
     使用semaphore(信号量来控制并发访问线程的个数),semaphore可以协调各个线程，以保证它们可以正确，合理的使用公共资源;
     ref code:https://github.com/xingxingt/concurrent/tree/master/src/main/java/com/concurrent/concurrent/aqs

#### 在Java中wait,yield,sleep和join方法的不同；
    sleep:需要指定等待的时间,sleep方法可以让当前正在执行的线程暂停执行，进入阻塞状态，该方法可以让与该线程同优先级或者高优先级的线程  
          得到执行机会，也可以让低优先级的线程得到执行机会;但是sleep不会释放锁标志，如果多个线程同时访问synchronized同步代码块，那么  
          其他线程仍然无法访问共享数据;
    wait:wait()方法和notify(),notifyall()方法一起使用，这三个方法协调多线程对共享数据的存取，所以这三个方法要在synchronized代码块中使用,    
         也就是说调用wait()方法和notify(),notifyall()之前，线程必须获取锁,这三个方法是Object内的方法，不是Thread中的方法;    
         wait()和sleep()的区别在于该方法释放对象的锁标志,当某个对象调用wait方法后会暂停当前线程的执行，并将当前线程放入对象等待池中，  
         直到调用notify()方法后才将对象等待池中任意一个线程移出并放入锁标志等待池中，锁标志等待池中的线程随时准备竞争锁的拥有权，当对象  
         调用notifyAll()方法时，会将对象等待池中的所有对象都移入锁标志等待池中； 
         wait()，notify()及notifyAll()只能在synchronized语句中使用;      
    yield: yield和sleep相似,暂停线程之后不会释放锁标志，yield只会让当前线程重新回到可执行的状态，所以通过yield()方法，线程回到可执行状态后  
           又有可能进入可执行的状态后马上执行，所以说yield()是不可靠的；另外yeild()只能使同优先级或者高优先级的线程得到执行机会;  
    join: 会使当前线程等待调用join()方法的线程执行完毕后在执行,插队;  
       
    ref:https://blog.csdn.net/xiangwanpeng/article/details/54972952
        https://www.jianshu.com/p/25e959037eed
    
#### 谈谈wait/notify关键字的理解
    可以通过配合调用Object对象的wait（）方法和notify（）方法或notifyAll（）方法来实现线程间的通信;   
    具体看上题; 
    
#### 什么导致线程阻塞？
     线程阻塞的特点: 线程阻塞后该线程会让出cpu的使用权，暂停运行，只能等待阻塞的原因消除恢复正常执行，或者是该线程被中断，  
                   抛出InterruptedException异常，该线程也会退出阻塞状态;  
     导致线程阻塞的原因:   
                   1,该线程执行了Thread.sleep(time)方法,使该线程进入休眠并放弃cpu的使用权，等休眠时间到了再恢复cpu的使用权;  
                   2,线程执行同步代码块，未获取到相关同步锁，从而使该线程处于等待阻塞状态，等到获取到同步锁后，再恢复执行；  
                   3,线程执行了一个对象的wait()方法，使该线程处于阻塞状态，等到其他线程执行了notify()后者notifyAll()方法后才能恢复执行;  
                   4,线程执行一些IO操作，因为等待某些资源而进入等待阻塞状态，例如等待客户端的输入...  
      ref:https://blog.csdn.net/sinat_22013331/article/details/45740641
      
#### 线程如何关闭？
     1，使用标志位,例如定义一个用volatile修饰的状态为(为了保证线程之间的可见性)，然后在线程启动后定期检查这个状态位，如果这个状态位为True了，则  
        马上结束线程；这个方法的弊端就是如果线程处于阻塞状态，那么这个线程无法进行状态位的检查，从而无法停止线程;  
     2,使用java提供的中断机制,interupte(),isInterrupted(),interrupted();对线程调用interrupt()方法，不会真正中断正在运行的线程，  
       只是发出一个请求，由线程在合适时候结束自己。     
     3,使用Executor框架,ExecutorService扩展了Executor，ExecutorService.submit()之后返回一个future，可以使用future.cancle()来中断线程; 
     ref:https://www.jianshu.com/p/536b0df1fd55  
     code:https://github.com/xingxingt/concurrent/tree/master/src/main/java/com/concurrent/concurrent/thread
        
#### 讲一下java中的同步的方法
     为什么要同步?    
     java允许多个线程并发控制，当多个线程对一个共享变量修改时会使共享资源变量不准确，所以通过同步锁的方法，使一个线程对共享变量进行操作的时候  
     禁止其他线程操作，从而保证了该变量的唯一性和准确性；  
     同步方法：  
     1，使用synchronized关键字修饰的方法；  
     2,使用synchronized关键字修饰代码块；  
     3,使用volatile修饰的变量实现线程同步，但是volatile修饰的变量不具备原子性和线程安全性;  
     4,使用锁机制来实现线程同步，例如：ReentrantLock  
     5,使用局部变量ThreadLocal实现线程同步；   
     6,使用阻塞队列实现线程同步LinkedBlockingQueue;  
     7,使用原子变量实现线程同步,util.concurrent.atomic包下的原子变量;  
     ref:https://www.cnblogs.com/XHJT/p/3897440.html
    
#### 数据一致性如何保证？
     1，CAS；  
     2，Final修饰不可变;  
     3,synchronized，修改代码块或者方法，通过线程互斥的方式实现;  
     4,volatile修饰,但是不能保证原子性;
     5,concurrent,lock,atomic包下的工具类来实现；
     ref:https://www.cnblogs.com/jiumao/p/7136631.html

#### 如何保证线程安全？
    1，互斥同步： 同步表示共享变量在被多个线程访问时，保证共享变量只被一个线程使用，互斥是方法，同步是目标，例如synchronized等锁机制；  
    2,非阻塞同步，非阻塞同步是先进行操作，如果没有其他线程争用共享数据，那操作就成功；如果数据有争用，产生了冲突，那就采取其他的补偿措施。  
    3，无同步方案；  对于一个方法本来就不涉及共享数据，那就自然无须同步措施来保证正确性。  
    ref:https://www.jianshu.com/p/fe7ed5b50933

#### 如何实现线程同步？
    1,互斥同步：同步是保证在多线程环境下并发的访问同一个共享变量时，同一时刻只有一个线程能够操作共享变量;  互斥是方法，同步是目的;  
      例如使用:synchronized关键字...   
    2,非阻塞同步： 互斥的话会带来线程间的阻塞和唤醒从而影响性能，非阻塞同步是真先进性操作共享数据，如果没有其他线程竞争共享资源，那就操作  
      成功，如果有线程竞争共享资源，产生了冲突，那么久采用其他的补救措施；  
    3，无同步方案：对于一段代码不涉及到共享资源，那么也就无需采用同步措施来保证正确性;     
    4,使用重入锁实现线程同步（ReenreantLock）；  
    5,使用局部变量实现线程同步 使用ThreadLocal管理变量;  
    6,使用特殊域变量(volatile)实现线程同步,但它是非原子性的；  
    7,使用阻塞队列实现线程同步,LinkedBlockingQueue;    
    8,使用原子变量实现线程同步util.concurrent.atomic包下的;  
    ref:https://www.cnblogs.com/XHJT/p/3897440.html

#### 两个进程同时要求写或者读，能不能实现？如何防止进程的同步？
    1,允许多个线程同时对文件进行读操作;  
    2,只允许一个写线程往文件中写操作;  
    3,任一写者在完成写操作之前不允许其他读者或写者工作;  
    4,写者执行写操作前，应让已有的读者和写者全部退出。   

#### 线程间操作List 
    多线层操作list会出现如下情况:  
    如果一个线程正在遍历list，而恰好另一个线程对该list做了remove操作，那么此时遍历list过程会出现ConcurrentModificationException；    
    解决办法:   
    1,使用vector，并在遍历该list的时候使用synchronized(list)来加锁，使在遍历list的时候不让remove操作执行；  
    2,使用CopyOnWriteArrayList,copyOnWrite也就是在写时进行copy，当对CopyOnWriteArrayList进行修改操作时，它首先会copy一份新的list  
      并在新的list上做修改，最后将原list的引用指向新的list；所以如果一个线程在遍历list的时候，另一个线程对该list做修改操作，其实是在新的list  
      上做的修改操作，并不会应该list的遍历操作;  
    3,使用线程安全的list.forEach，java8的新特性，例如vector的forEach方法，是在该forEach上加了synchronized关键字来控制线程安全的遍历;  
    ref:https://blog.csdn.net/xiao__gui/article/details/51050793 

#### java对象的生命周期  
    1,创建阶段，一旦某个对象被创建后，并分配给某个变量赋值，则这个对象就进入引用阶段；  
    2,应用阶段，对象至少被一个强引用持有着;  
    3,不可见阶段,程序本身不对该对象持有任何强引用，虽然该对象的这些引用还存在着，即程序的执行已经超出了该对象的作用域(这种情况编辑器就能检查的出来);  
    4,不可达阶段，指该对象不会被任何强引用所持有;  
    5,收集阶段,当对象已经进入了不可达阶段，并且垃圾收集器已经为该对象重新分配的内存空间，则该对象进入收集阶段，此阶段可以通过finalize()逃逸,
      从新获得引用;
    6,回收阶段,当对象仍然处于不可达阶段，并且已经执行了finalized()方法，则该对象进入回收阶段;  
    7,对象空间重新分配,垃圾收集器对该对象的内存空间进行回收或者重新分配，该对象彻底消失; 
    ref:https://blog.csdn.net/sodino/article/details/38387049
![](https://ws4.sinaimg.cn/large/006tKfTcgy1g14bvg072ij30uo0eu43j.jpg)    

#### java对象的内存布局
    在HotSpot中对象的内存分布分为3个区域，对象头，实例数据和对齐填充;  
    对象头:有分为两部分存储: 
       1,一部分存储对象自身的运行时数据，包括：hash码，GC分代年龄,锁状态标志，线程持有的锁，偏向线层id，偏向时间戳等,这部分数据称为"Mark Word";  
       2,另一部分存储对象的类型指针,该指针指向它的类元数据的指针，虚拟机通过这个指针来确定这个对象是哪个类的实例;    
    实例数据:存储的是对象真是的有效信息，也就是对象的代码中定义的各种类型的字段内容;   
    对齐填充:JVM要求对象大小必须是8的倍数，所以当对象的实例数据没有对齐时，就需要对齐填充来补全；  

#### 什么是线程的上下文切换
    上下文切换的过程:  
    CPU是能够处理多任务的，多任务是指cpu能同时处理两个或者两个以上的程序；在多任务的处理系统中，cpu需要处理所有程序的操作，当CPU来回切换任务  
    时需要记录这些任务执行到哪里了，这就是上下文的切换过程，允许cpu记录并恢复各种程序的运行状态，使它能够完成切换状态;  
    上下文切换:  
    操作系统通过时间片轮转的方法，cpu给每个任务都服务一定的时间，然后把当前任务的状态保存下来，然后加载下一个任务的状态，继续为下一个任务服务，  
    任务的保存和加载的过程就是线程的上下文切换
    ref:https://juejin.im/post/5b10e53b6fb9a01e5b10e9be

#### java的乐观锁和悲观锁
    乐观锁：乐观锁是一种乐观思想，即认为读多写少，遇到并发的可能性低，每次拿数据的时候都任务别人不会修改，所以不会上锁，但是更新的时候会判断  
           一下数据是否被别人修改过，即读取出当前的版本号，然后加锁，再对比旧的版本号，如果版本号一直则认为该数据没被修改然后更新，如果不  
           一致则重复读取-比较-更新;  java的乐观锁是通过CAS来实现的；
    悲观锁: 悲观锁则是一种悲观思想，总认为写多，遇到高并发的可能性高，每次去拿数据的时候总任务别人会修改数据，所以每次都会上锁，这样别人  
           去拿数据的时候总会被阻塞一直等到获取到锁，java中的悲观锁是Synchronized； AQS下的锁先用乐观锁CAS尝试获取锁，如果获取不到则转为  
           悲观锁，如ReetranLock;

#### 偏向锁，自旋锁，轻量级锁，重量级锁
    偏向锁:  
         偏向锁的使用场景是在无多线程竞争锁的情况；即在运行中只有一个线程运行无并发情况，就会给线程添加一个偏向锁; 如果在运行中遇到了其他线程  
         抢占锁，则持有偏向锁的线程会被挂起，jvm会把此偏向锁消除，转换为轻量级锁；   
         偏向锁的获取过程：  
         1，先判断对象的对象头部的Mark Word区域的锁标志是否为1，偏向锁标志是否为01;  
         2,如果开启了偏向锁，则判断对象头部记录的线程id是否是当前线程，如果是则进入同步代码块中，如果不是则进入下一步;  
         3,如果当前线程未获取到锁资源，则通过CAS竞争锁，如果获取成功则将Mark Work中的线程ID修改为当前线程的线程ID，然后执行同步代码块，  
           如果获取失败，则进入下一步;  
         4,如果通过CAS获取锁失败，则表示有线程竞争当前锁，当线程进入安全点(safePoint)的时候挂起当前线程，偏向锁升级为轻量级锁，然后被阻塞在  
           安全点的线程继续执行下面的同步代码块(因为这个时候肯定是已经获取到了锁在执行同步代码块的时候出现了其他线程竞争锁；注意:撤销偏向锁的
           时候会触发Stop the World）;  
                    
    自旋锁:
         如果获取锁的线程在短时间内可以释放锁资源，那么其他等待获取锁资源的线程就没必要进入阻塞状态，而是通过自旋的方式等待获取锁的锁资源，从而  
         避免了内核态和用户态的来回切换的消耗；自旋锁是需要消耗cpu的，利用cpu做无用功的方式来实现自旋；如果自旋时间超过了设定的最大时间，则停止  
         自旋进入阻塞状态(jdk1.7自旋锁的最大时间由JVM控制)；    
   
    轻量级锁:  
         轻量级锁也是在单线程环境下无锁竞争的情况下使用;  
         轻量级锁是由偏向锁转换而来的，当持有偏向锁的线程遇到有其他线程竞争锁资源的时,则会将偏向锁升级为轻量级锁;  
         加锁过程:  
         1,在线程进入同步代码块的时候，先判断对象头的锁状态，如果此时的锁状态位是01,是否为偏向锁状态为0，则在此线程的栈帧中建一个锁记录  
         (Lock Record)空间用于存储对象头的Mark Word；  
         2,将对象头的Mark Word拷贝到线程栈的Lock Record中；  
         3,copy成功后，使用CAS操作将对象的Mark Word指向Lock Record的指针，并将Lock Record中的Owner指向Object的Mark Word；  
         4,如果第三步更新成功了，则表示当前线程获取的带对象的锁，并将该对象的Mark Word中的锁标志置位00代表当前是轻量级锁；  
         5,如果第三部更新失败，则先检查对象的Mark Word指向的指针是否是当前线程栈，如果是则表明当前线程已经获取到了锁资源直接进入同步代码块即可；  
           如果不是当前线程则表示有多个线程在竞争锁资源，轻量级锁要膨胀为重量级锁，对象头的锁标志变为10(表示为重量级锁)，后面的线程则处于阻塞等  
           待状态，当前线程则自旋获取锁；  
         
     重量级锁：见下面：Synchronized👇    
     ref:https://blog.csdn.net/zqz_zqz/article/details/70233767  
         https://blog.csdn.net/u012465296/article/details/53022317  
![](https://ws3.sinaimg.cn/large/006tKfTcgy1g162tj8p6rj30xe0ke75v.jpg)         

#### Synchronized用法
     Synchronied保证了在多线程的情况下方法或者代码块在同一时间只有一个线程能够进入(互斥性),同时它还能保证共享变量的可见性;  
     java的每个方法都可以作为锁：  
         1,作用于普通方法，锁是当前实例对象；  
         2,作用于静态方法，锁是当前类的class对象;当作用于静态方法时，锁住的是Class实例，又因为Class的相关数据存储在永久带PermGen（   
                       jdk1.8则是metaspace），永久带是全局共享的，因此静态方法锁相当于类的一个全局锁，会锁所有调用该方法的线程；      
         3,同步代码块，锁是括号中的对象;  
     ref:http://www.51gjie.com/java/727.html    
     refcode:https://github.com/xingxingt/concurrent/tree/master/src/main/java/com/concurrent/concurrent/sync      

#### synchronize的原理
     首先Synchronized是重量级锁;    
     1,检测对象头中的Mark Word中存放的线程id是否为本线程，如果是，则表明该线程获取了偏向锁;  
     2,如果不是,则使用CAS将Mark Word中线程ID替换为本线程的ID,如果CAS操作成功则获取了偏向锁，进入同步代码块,否则下一步;  
     3,如果该线程的CAS操作失败，则表示有线程竞争该锁，则需要撤销偏向锁，升级为轻量级锁;  
     4,当前线程把Mark Word替换为锁记录指针，如果替换成功则当前线程获取锁;  
     5,如果获取失败则表示有其他线程竞争锁，则通过自旋的方式获取锁；  
     6，如果通过自旋获取锁成功则该线程依然持有轻量级锁；  
     7，如果自旋失败则升级为重量级锁;  
     这些操作都是由JVM内部实现，当使用Synchronized时，JVM会根据启用锁和当前线程的竞争情况来决定如何执行同步操作;    
     在所有的锁都启用的情况下线程进入临界区时会先去获取偏向锁，如果已经存在偏向锁了，则会尝试获取轻量级锁，启用自旋锁，如果自旋也没有获取到锁，
     则使用重量级锁，没有获取到锁的线程阻塞挂起，直到持有锁的线程执行完同步块唤醒他们；  
     ref:https://blog.csdn.net/zqz_zqz/article/details/70233767   
         https://www.jianshu.com/p/19f861ab749e  

#### 谈谈对Synchronized关键字，类锁，方法锁，重入锁的理解
    重入锁：  
    Synchronized是强制原子性的内置锁机制，当一个线程获取该对象锁之后再次获取该锁的时候可以再次得到该锁，不需要去竞争锁；也就是说一个synchronied  
    方法/块在调用被类内部的其他Synchronized方法时是永远可以拿到锁;  
    重入锁的实现原理:   
    在线程栈中有一个数据结构(monitor)用于存储锁的持有者(owner)以及锁的重入次数(Nest),当一个线程已经获取到了该锁那么Nest则被置为1,当该线程  
    再次想要获取该锁的时候，会判断该锁的持有者是否是该线程，如果是则将Nest+1，进入同步代码块；当线程退出同步代码块时，计数器会递减，如果计数器为0，
    则释放该锁； 
    重入锁ref:https://blog.csdn.net/aigoogle/article/details/29893667?utm_source=tuicool&utm_medium=referral  
             https://www.jianshu.com/p/19f861ab749e  
    
    refCode:https://github.com/xingxingt/concurrent/tree/master/src/main/java/com/concurrent/concurrent
            /sync/SynchronizedMethodAndCodes.java  
    ref:https://blog.csdn.net/le_le_name/article/details/52348314    

#### static synchronized 方法的多线程访问和作用
    synchronizd是对类的当前实例进行加锁，而static synchronized是对类的所有实例进行加锁；
    看代码例子:  
     refCode:https://github.com/xingxingt/concurrent/tree/master/src/main/java/com/concurrent/concurrent
            /sync/SynchronizedMethodAndCodes.java  
     ref:https://blog.csdn.net/wangtaomtk/article/details/52318634  
        
#### 同一个类里面两个synchronized方法，两个线程同时访问的问题
     ref:https://blog.csdn.net/aiyawalie/article/details/53261823
     refCode:https://github.com/xingxingt/concurrent/tree/master/src/main/java/com/concurrent/concurrent
            /sync/SynchronizedMethodAndCodes.java  

#### java的内存模型
    引入:
    原子性：是指cpu的一个操作，要么执行完，要么不执行，不可中途暂停然后再调度;    
    可见性：指多个线程访问同一个变量，一个线程修改了该变量，其他线程应该立即能看的到修改的值;  
    有序性:指程序的执行顺序按照代码的顺序先后执行;  
    缓存一致性问题其实就是可见性问题。而处理器优化是可以导致原子性问题的。指令重排即会导致有序性问题。 
    为了保障共享内存的正确性(原子性,可见性,有序性),内存模型定义共享内存系统中多线程读写操作的规范； 
    java(JMM)内存模型:  
    java内存模型是一种规范，它规定了一个线程如何和核实被其他线程修改过的共享变量的值,以及如何同步的访问共享变量;    
    要求本地变量和对应栈存放在线程栈上，对象存放在堆上，线程之间的通信必须经过主内存；(Java内存模型规定了所有的     
    变量都存储在主内存中，每条线程还有自己的工作内存，线程的工作内存中保存了该线程中是用到的变量的主内存副本拷贝，  
    线程对变量的所有操作都必须在工作内存中进行，而不能直接读写主内存。不同的线程之间也无法直接访问对方工作内存中  
    的变量，线程间变量的传递均需要自己的工作内存和主存之间进行数据同步进行;)
    ref:https://www.hollischuang.com/archives/2550
![](https://ws2.sinaimg.cn/large/006tKfTcgy1g0szp0qn1vj30u40hsgn9.jpg)    
    

#### java内存屏障
    内存屏障(Memory Barrier)又称内存栅栏，是一个cpu指令，特点:  
    1,保证特定操作的执行顺序；  
    2，影响某些数据的可见性；  
    编译器和cup能够重排序指令，保证最后相同的结果，提高性能，但是如果插入一条Memory Barrier会告诉编译器和cpu  
    不管什么指令都不能和这条Memory Barrier进行重排序;  
    内存屏障还有一个重要的特点是可以强制刷出cpu的各种cache(主存)，比如读取屏障的操作，将会强制从cpu的cache总读取(主存);
    volatile的实现就是基于内存屏障来实现的;
    
#### volatile的原理
    volatile是通过加入内存屏障和禁止指令重排序优化来实现的；    
    volatile在写的操作，在写操作之后加入一个store屏障指令，将本地内存中的共享变量值刷新到主内存中;  
    volatile在读的操作，在读操作之前加一个load屏障指令，从主内存中读取共享变量;  
![](https://ws3.sinaimg.cn/large/006tKfTcgy1g0sb3h62iij31180u077f.jpg)

#### happens-before
    如果线程A满足线程B的happens-before原则，那么线程A的执行动作的结果对线程B是可见的，如果两个线程未按照happens-before原则  
    ，则JVM会对他们的执行顺序重新排序;

#### 什么是CAS
    CAS即compare and swap(比较和交换);  
    CAS的思想: CAS会传入三个值，当前内存中的值V，旧的期望值A，即将要更新的值B,当且仅当期望值A和内存中的值V相同时，将内存值修改为B，
    并返回true，否则什么都不做，返回false;  
    CAS通过java的本地方法Unsafe来实现的，java方法无法操作底层内存中的数据，需要通过本地方法来访问,而Unsafe可以直接操作指定内存中数据;
    CAS的缺点:有ABA的问题，可以通过AtomicStampedReference来解决;
    ref:https://www.jianshu.com/p/fb6e91b013cc
    
#### 谈谈volatile关键字的用法
    volatile保证了不同线程对该变量操作的可见性;   
    禁止指令重排序;    
    所以volatile关键字只是保证了JMM中的可见性，有序性，并不能保证原子性；  
    ref:https://blog.csdn.net/hang1995/article/details/79417477
   
#### 谈谈volatile关键字的作用
    被volatile修饰的变量，在每次读取的时候都是读取主存中的变量值，例如多线程环境下，如果每个线程都会copy一份共享变量到线程栈中，如果  
    这个共享变量被volatile修饰，那么线程每次都会从主存中读取变量值，每次修改过后都会将修改后的变量值写回主存中去;  
    ref:https://www.cnblogs.com/aigongsi/archive/2012/04/01/2429166.html


#### transient关键字的作用
    1）一旦变量被transient修饰，变量将不再是对象持久化的一部分，该变量内容在序列化后无法获得访问。  
    2）transient关键字只能修饰变量，而不能修饰方法和类。注意，本地变量是不能被transient关键字修饰的。变量如果是用户自定义类变量，  
        则该类需要实现Serializable接口。  
    3）被transient关键字修饰的变量不再能被序列化，一个静态变量不管是否被transient修饰，均不能被序列化。

#### synchronized 和volatile 关键字的区别
    synchronized主要作用于执行控制，该线程获取了锁其他线程也要获取该锁的话就会被阻塞，这样就保护了被synchronized修饰的代码块不被其他线程访问，  
    同时Synchronized会建立一个内存屏障，内存屏障指令保证了cpu的所有操作结果都会刷新到主存中，这样就保证了共享资源的可见性；   
    volatile主要的作用是内存的可见行，它保证了通过volatile修饰的变量读写都会刷新到主存中；  
    区别:  
    1,被volatile修饰的变量会告诉jvm该值存在于线程的工作内存中是不可靠的，需要从主存中读取，而Synchronized主要是锁住当前变量，只有当前线程  
      可以访问，其他线程被阻塞；  
    2,volatile只能修饰变量，而Synchronized可以修饰变量，方法，类；  
    3,volatile修饰的变量只能保证多线程的可见性，而Synchronized修饰的可以保证多线程的可见性和原子性；  
    4,volatile不会造成线程阻塞，而Synchronized会造成线程阻塞;   
    5,volatile修饰的变量不会被编译器优化(禁止指令重排序)，而Synchronized会被编译器优化；  
    ref:https://blog.csdn.net/suifeng3051/article/details/52611233

#### synchronized与Lock的区别
    ref:http://www.cnblogs.com/dolphin0520/p/3923167.html
        https://blog.csdn.net/u012403290/article/details/64910926?locationNum=11&fps=1
![](https://ws4.sinaimg.cn/large/006tKfTcgy1g17b75r93vj31hk0l875j.jpg)


#### ReentrantLock 、synchronized和volatile比较
     synchronized:是内置锁，锁的管理交给底层的JVM，又称为互斥锁，即操作互斥;  
     ReentrantLock:可重入锁,需要自己去管理锁，特点如下：  
             1,ReentrantLock有tryLock()方法，允许尝试获取锁，如果获取到锁则返回true，否则返回false；  
             2,ReentrantLock有公平锁和非公平锁功能，在创建的时候可以指定，默认是非公平锁,可抢占;    
             3,ReentrantReadWriteLock,可实现读写分离锁,可用于读多写少的情景，读的时候不加锁，写的时候加锁从而大大提高性能；  
     volatile:修饰于变量,主要是内存可见性和禁止指令重排序，但是非线程安全的，也不是原子性的；         
     ref:https://www.cnblogs.com/dennyzhangdd/p/6020566.html  
     
#### ReentrantLock的内部实现
    ReentrantLock是可重入锁，实现主要有两种，公平锁和非公平锁，公平锁就是多个线程竞争锁的时候，每个线程按照抢占锁的顺序依次获取锁，  
    而非公平锁如果线程A获取了锁，线程B处于锁等待被挂起状态，当A执行完代码释放锁后，B开始尝试获取锁，在B尝试获取锁的时候C线程也来尝试  
    获取锁并且获取锁成功，那B线程则继续处于等待状态，这就是非公平锁的抢占机制;   
    ReentrantLock内部有一个计数器state，当线程A获取了锁，那么state的值原子性的+1,而在线程A获取锁的时候，线程A又尝试获取锁，此时  
    线程A无需排队等待，只需将计数器state的值原子性的+1即可,然后访问同步代码块(+1操作是用CAS来完成的);  
    另外还有条件变量Condition:条件变量很大一个程度上是为了解决Object.wait/notify/notifyAll难以使用的问题. 在Synchronized中  
    所有线程都在一个object条件队列中等待，而ReentrantLock一个lock可以有多个condition条件队列;   
    ref:https://blog.csdn.net/yanyan19880509/article/details/52345422   
        https://www.jianshu.com/p/4358b1466ec9  

#### lock原理
    底层AQS分析  
    https://blog.csdn.net/endlu/article/details/51249156  
    https://www.jianshu.com/p/d8eeb31bee5c  

#### 死锁的四个必要条件:  
    1,互斥条件：一个资源每次只能被一个进程使用。  
    2,请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。  
    3,不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。   
    4,循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。  
        
#### 怎么避免死锁？
     1,线程按照一定的顺序加锁;   
     2,给线程添加加锁时限，当一个线程请求获取锁被阻塞时,会判断等待的时长，如果超过设定的时限，则自动放弃请求锁；  
     3，死锁检测;  
     ref:https://blog.csdn.net/ls5718/article/details/51896159
     
##### 对象锁和类锁是否会互相影响？
    对象锁和类锁是不同的锁，对象锁类的实例锁，而类锁则是一个类的class对象锁,java类可能有很多个对象，但只有一个class类；  
    对于对象锁来说，各自对象持有各自的对象锁，互补干扰，不存在竞争锁的情况；但是对于类锁则不同，例如该类的多个对象都持有类锁，  
    那么这些对象所持有的是同一把锁，所以会出现锁竞争的情况；  
    1个线程访问静态synchronized的时候，允许另一个线程访问对象的实例synchronized方法。反过来也是成立的，因为他们需要的锁是不同的。  
    ref:https://blog.csdn.net/codeharvest/article/details/70649375  
        https://github.com/xingxingt/concurrent/blob/master/src/main/java/com/concurrent/concurrent/
        sync/SynchronizedMethodAndCodes.java

#### 什么是线程池，如何使用?
    线程池：
        存放线程的对象池,池中线程的执行调度由池管理器来管理，当执行任务时从池中获取一条线程，任务执行完毕后再放回池中，这样可以
        避免反复创建对象带来的性能开销，节省系统资源；  
    线程池的作用:  
        1,线程的创建和销毁都伴随着系统的开销，过于频繁的创建和销毁线程会造成严重的影响执行效率；  
        2,可运用线程池来控制线程的并发数，从而可以避免线程任务过多而系统资源不足导致任务阻塞的情况;  
        3,对线程的管理,比如对线程定时循环执行或者延迟执行；  
    ref:https://www.jianshu.com/p/210eab345423  
    ref:https://github.com/xingxingt/concurrent/tree/master/src/main/java/com/concurrent/concurrent/threadpool


#### 线程池的拒绝策略  
    线程池的拒绝策略:   
    1，当线程数量为达到corePoolSize数量时则新建一个核心线程去执行；   
    2，线程数量达到了corePools，则将任务移入队列等待；  
    3，队列已满，新建线程(非核心线程)执行任务；    
    4，队列已满，总线程数又达到了maximumPoolSize，就会由线程池的拒绝策略处理；     

    java线程池的拒绝策略的实现:  
    1,ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。  
    2,ThreadPoolExecutor.DiscardPolicy：也是丢弃任务，但是不抛出异常。  
    3,ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）  
    4,ThreadPoolExecutor.CallerRunsPolicy：由调用线程处理该任务   
    ref:https://www.jianshu.com/p/210eab345423    
    ref:ref:https://github.com/xingxingt/concurrent/tree/master/src/main/java/com/concurrent/concurrent/
    threadpool/ThreadPoolRejectedDemo.java
    
![](https://ws4.sinaimg.cn/large/006tNc79gy1g5a4ksff6rj31580nudj9.jpg)    
    

#### 线程池获取返回值
    线程池提交任务的方法:  
    1.public void execute(Runnable command);  该方法没有返回值，而且只能提交Runnable；   
    2.public Future<?> submit(Runnable task);   
      public <T> Future<T> submit(Runnable task, T result)    
      public <T> Future<T> submit(Callable<T> task)     
      这个方法接收两种参数，Callable和Runnable。返回值是Future。   
    使用submit方法有一个特点就是，他的异常可以在主线程中catch到。而使用execute方法执行任务是捕捉不到异常的。  
    ref:ref:https://blog.csdn.net/qq_25806863/article/details/71214033

#### java的BlockQueue
    ref:http://www.importnew.com/28053.html

#### Java的并发、多线程、线程模型
    ref:http://ifeve.com/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E6%A8%A1%E5%9E%8B/
  
#### threadlocal
    threadlocal原理:   
    threadlocal其实是在每个线程中维护着一个ThreadLocalMap，该map是以当前线程为key，以set的value为value；     
    threadlocal的get或者set方法就是通过线程内部的ThreadlocalMap来进行对象的存取;     
    threadlocal本身并不存放值，他只是作为key能让线程从TheadlocalMap中获取value；   
    所以threadlocal能够实现线程间的数据隔离，获取当前线程的局部变量值，不受其他线程影响;    
    设计理念:   
    ThreadLocal的设计目的是实现在当前线程中有自己的变量，并不是为了解决高并发和共享变量的问题;  
    ref:https://juejin.im/post/5ac2eb52518825555e5e06ee#comment
  
  
* 谈谈对多线程的理解
* 多线程有什么要注意的问题？
* 谈谈你对并发编程的理解并举例说明
* 谈谈你对多线程同步机制的理解？
* 如何保证多线程读写文件的安全？
* 多线程断点续传原理
* 断点续传的实现
* 谈谈NIO的理解
