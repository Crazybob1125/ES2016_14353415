##Deadlock 死锁

在上学期的操作系统课就对死锁有所了解，死锁就是指在两个或者以上的进程在执行过程中，因为互相请求资源而造成的互相等待的现象，若无相应的处理方法，这些进程将会停滞。

#####死锁产生条件：
1.互斥条件：一个资源只能被一个进程使用。（一旦一个进程获得了，就是它独有的）

2.请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。（别的进程来抢我的资源，不给）

3.不剥夺条件：进程已获得的资源，在未完成使用完之前，不能强行剥夺。（资源没用完，不给）

4.循环等待条件：若干进程之间形成的一种头尾相接的循环的等待的资源关系。（多条请求资源的链形成死循环）

#####现在具体来看看实验代码：


    class A{
    	// 线程访问对象的一个synchronized代码块时，
    	//其他线程对这对象中其他synchronized代码块的访问会被阻塞 
    	synchronized void methodA(B b){
    		b.last();
    	}	
    	synchronized void last(){
    		System.out.println("Inside A.last()");
    	}
    }
    
    class B{
    	synchronized void methodB(A a){
    		a.last();
    	}	
    	synchronized void last(){
    		System.out.println("Inside B.last()");
    	}
    }
定义class A class B。
methodA()会调用b.last()，即访问B类的函数，执行内容是打印字符串。

这里用synchronized修饰，说明某线程访问这个代码块时，其他线程将不能访问此代码块（满足了不剥夺条件）。


    class Deadlock implements Runnable{
    	A a = new A();
    	B b = new B();
    	
    	Deadlock(){
    		Thread t = new Thread(this);
    		int count = 20000;
    		//t开始运行之后，先调用a.methodA 
    		t.start();
    		while(count-- > 0);
    		a.methodA(b);
    	}
    	//调度到Runnable线程的时候，会执行这里 
    	public void run(){
    		b.methodB(a);
    	}
    	public static void main(String args[]){
    		new Deadlock();
    	}
    }

Runnable是一个在后台运行的线程，每次调度到它的时候，就会执行run()里面的语句。

implements关键字说明这是对Runnable接口的实现。

整个函数重复执行100次：

![](http://i.imgur.com/wruCdFN.png)


每次Main函数new一个Deadlock类，然后new出了a, b两个对象。

然后创建一个线程t，固定的隔20000个时间单位后，执行一次a.methoadA(b);

而由于run()是当调度到Runnable线程的时候才会执行，所以时间不一定。

由于a.methodA(b)的执行时间间隔是固定的，而b.methodB(a)的执行时间间隔是不固定的，所以到了一定的时间，这两条语句将会同时执行。

而这时a试图访问b的一个方法，b试图访问a的一个方法，但是谁都不愿放掉自己的资源，于是这就形成了死锁。

![](http://i.imgur.com/J5kPrnQ.png)
大概就是这么个情况，两边都想要对方的资源（调用对面的方法），但是两边都不想放开自己的资源，就形成了僵局。

#####最后来说说解决死锁的方法：
######（1）死锁检测和恢复
① 发现死锁时，撤销一些处于死锁状态的进程。

②挂起一些处于死锁状态的进程，并释放他们的资源。

######（2）死锁预防
①每个进程都必须一次性地请求它所需的所有资源，若不能满足，则不执行。这是一个资源预分配的方法，破坏了请求与保持的条件。

②一个已占有资源的进程若要再申请新资源，则需先释放已占有的资源。若随后还需要，则再申请。就是说一个进程在使用某资源过程中可以放弃该资源，这破坏了不剥夺的条件。

③将系统中所有资源顺序编号，规定进程只能依次申请资源，这就是说，一个进程只有在前面的申请满足后，才能提出对其后面序号的资源的请求。这是一种有序使用资源法，它破坏了产生死锁的第四个必要条件。

######（3）死锁避免
如银行家算法。