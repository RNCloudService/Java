# 七十二、synchronized隐式锁实现线程安全

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


  @Override
  public Integer call() throws Exception {
    for (int i = 1; i <= 100; i++) {
      //同步代码块实现线程同步
//      synchronized (this) {
//        num ++;
//      }
      intPlus();
    }
    return num;
  }

  //同步方法实现线程同步
  public synchronized void intPlus() {
    num++;
  }
```

