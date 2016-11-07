# Deadlocks
***
### Result
![dedalocks](http://7xrn7f.com1.z0.glb.clouddn.com/16-11-7/84506596.jpg)

### Conditions for Deadlock
- 互斥条件：一个资源每次只能被一个进程使用
- 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放
- 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺
- 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系

### Explain
 在类A中调用methodA会占用类B，在类B中调用methodB也会占用类A。因此在同时调用methodA与methodB的时候，就会形成死锁。我们看代码：
 
    class Deadlock implements Runnable{
	    A a=new A();
	    B b=new B();
	
        Deadlock(){
    		Thread t = new Thread(this);
    		int count = 20000;
    		
    		t.start();
    		while(count-->0);
    		a.methodA(b);
    	}
    	
    	public void run(){
    		b.methodB(a);
    	}
    	public static void main(String args[]){
    		new Deadlock();
    	}
    }
可以知道，在线程t启动后进入了调度队列，然后主线程等到count迭代20000后，调用methondA，如果此时线程t同时运行methondB，此时a与b同时申请对方资源，且互不相让，产生死锁。
