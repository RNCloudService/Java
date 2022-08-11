# 七十三、Lock锁显示锁实现线程安全

```java
public class ThreadTest {

  //初始化线程池，创建方式为指定线程池数量创建
  public static ThreadPoolExecutor pool = 
      new ThreadPoolExecutor(
        //核心线程池的数量
        5,
        //最大线程能开到多少
        100,
        //线程空闲后，多开的线程多长时间休眠
        10,
        //线程休眠的时间单位
        TimeUnit.SECONDS,
        //工作/阻塞队列，一旦核心线程放满后，多余的任务会放进该队列，采用链表阻塞队列
        new LinkedBlockingDeque<>(100),
        //默认工厂模式
        Executors.defaultThreadFactory(),
        //阻塞队列满后，采用拒绝策略拒绝服务
        new ThreadPoolExecutor.AbortPolicy()
      );

  public static void main(String[] args) throws InterruptedException, ExecutionException {
    

    CallableTest call = new CallableTest();
    List<Future<Integer>> fts = new ArrayList<>();
    for (int i = 1; i <= 100; i++) {
      Future<Integer> ft = pool.submit(call);
      fts.add(ft);
    }
    int num = -1;
    for (Future<Integer> future : fts) {
      num = future.get() > num ? future.get() : num;
    }
    System.out.println(num);
    pool.shutdown();

  }
}

class CallableTest implements Callable<Integer> {


  private int num = 0;

  //该类实现于Lock接口，是Lock接口的实现类，一般使用该类进行开锁及关锁操作
  ReentrantLock lock = new ReentrantLock();

  @Override
  public Integer call() throws Exception {
    for (int i = 1; i <= 100; i++) {
      lock.lock();
      //为什么要加上try/finally呢？
      //因为在开锁之后有可能会发生异常导致当前线程一直占用资源不释放，不加的话后面的代码不会被执行
      //加上try/finally后不管有没有异常都能正常执行关锁操作，不会导致一个线程一直占用资源发生死锁
      try {
        num++;
      } finally {
        lock.unlock();
      }
      //同步代码块实现线程同步
//      synchronized (this) {
//        num ++;
//      }
//      intPlus();
    }
    return num;
  }

  //同步方法实现线程同步
//  public synchronized void intPlus() {
//    num++;
//  }
```

