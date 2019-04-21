# Java���߳�

## Executor��ܣ��̳߳ء� Callable ��Future��
### ʲô��Executor���
�򵥵�˵������һ�������ִ�к͵��ȿ�ܣ��漰��������ͼ��ʾ��
���У������Executor�ӿڣ����Ķ���ܼ򵥣�һ������ִ�������execute������������ʾ��
```
 public interface Executor {
        void execute(Runnable command);
}
```
���⣬���ǻ����Կ���һ��Executors�࣬����һ�������ࣨ�е����Ƽ��Ͽ�ܵ�Collections�ࣩ�����ڴ���ExecutorService��ScheduledExecutorService��ThreadFactory �� Callable����

### �ŵ㣺
������ύ������ִ�й��̽���û�ֻ�趨��������ύ���������ִ�У�ʲôʱ��ִ�в���Ҫ���ģ�

### ���Ͳ��裺
�����������Callable���󣩣������ύ��ExecutorService�����̳߳أ�ȥִ�У��õ�Future����Ȼ�����Future��get�����ȴ�ִ�н�����ɡ�

### ʲô������
ʵ��Callable�ӿڻ�Runnable�ӿڵ��࣬��ʵ���Ϳ��Գ�Ϊһ�������ύ��ExecutorServiceȥִ�У�
����Callable������Է���ִ�н����Runnable�����޷��ؽ����

### ʲô���̳߳�
ͨ��Executors��������Դ����������͵��̳߳أ�����Ϊ���������֣�

  - newCachedThreadPool ����С�����ޣ����߳��ͷ�ʱ�������ø��̣߳�
  - newFixedThreadPool ����С�̶����޿����߳�ʱ��������ȴ���ֱ���п����̣߳�
  - newSingleThreadExecutor ������һ�����̣߳�����ᰴ˳������ִ�У�
  - newScheduledThreadPool������һ�������̳߳أ�֧�ֶ�ʱ������������ִ��
 
### �̳߳ز������
 - �����߳�������1+�߳�IOʱ��/�߳�CPUʱ�䣩* CPU��
 - ���г��ȣ�queueCapacity = (coreSizePool/taskcost) * rt
 - ����߳���������߳��� = �����������-���г��ȣ�/ÿ���߳�ÿ�봦������
maxPoolSize = (max(tasks)- queueCapacity)/(1/taskcost)


### ���䣺���������ִ�з�ʽ
��ʽһ�����ȶ������񼯺ϣ�Ȼ����Future�������ڴ��ִ�н����ִ������������Future���ϻ�ȡ�����
 - �ŵ㣺�������εõ�����Ľ����
 - ȱ�㣺���ܼ�ʱ��ȡ����������ִ�н����

��ʽ�������ȶ������񼯺ϣ�ͨ��CompletionService��װExecutorService��ִ������Ȼ�������take()����ȥȡFuture����
 - �ŵ㣺��ʱ�õ�����������ִ�н��
 - ȱ�㣺�������εõ����

������΢�����£��ڷ�ʽһ�У��Ӽ����б�����ÿ��Future���󲢲�һ���������״̬����ʱ����get()�����ͻᱻ����ס�����Ժ��������ʹ�����Ҳ���ܵõ����������ʽ���У�CompletionService��ʵ����ά��һ������Future�����BlockingQueue��ֻ�е����Future����״̬�ǽ�����ʱ�򣬲Ż���뵽���Queue�У����Ե���take()�ܴ������������õ����µ����������Ľ����

## AbstractQueuedSynchronizer ��AQS��ܣ�
### ʲô��AQS���
AQS�����J.U.C��ʵ������ͬ�����ƵĻ�������ײ���ͨ������ LockSupport .unpark()�� LockSupport .park()ʵ���̵߳������ͻ��ѡ�

AbstractQueuedSynchronizer��һ�������࣬��Ҫ��ά����һ��int���͵�state���Ժ�һ�����������Ƚ��ȳ����̵߳ȴ����У�����state����volatile���εģ���֤�߳�֮��Ŀɼ��ԣ����е���Ӻͳ��Բ�����������������������������CASʵ�֣�����AQS��Ϊ����ģʽ����ռģʽ�͹���ģʽ����ReentrantLock�ǻ��ڶ�ռģʽģʽʵ�ֵģ�CountDownLatch��CyclicBarrier���ǻ��ڹ���ģʽ��
![�˴�����ͼƬ������](images/multi-thread-aqs-structure.jpg)

### �򵥾ٸ�����
�ǹ�ƽ����lock������ʵ�֣�
```
    final void lock() {
          //CAS���������StateΪ0����ʾ��ǰû�������߳�ռ�и���������������Ϊ1
          if (compareAndSetState(0, 1))     
              setExclusiveOwnerThread(Thread.currentThread());
          else
              acquire(1);
    }
```
�����ǲ����Ⱥ�˳��ֱ�ӳ��Ի�ȡ�����ǹ�ƽ������)���ɹ��Ļ���ֱ�Ӷ�ռ���ʣ�
�����ȡ��ʧ�ܣ������AQS��acquire�������ڸ÷����ڲ������tryAcquire�����ٴγ��Ի�ȡ���Լ��Ƿ�������жϣ����ʧ�ܣ������ǰ�̲߳����뵽�ȴ����У�
����ɲ鿴ReentrantLock.NonfairSync���AbstractQueuedSynchronizer���Ӧ��Դ�롣

  
## Locks & Condition����������������
�ȿ�һ��Lock�ӿ��ṩ����Ҫ���������£�

 - lock()  **�ȴ���ȡ��**
 - lockInterruptibly()  **���жϵȴ���ȡ����synchronized�޷�ʵ�ֿ��жϵȴ�**
 - tryLock() **���Ի�ȡ������������true��false**
 - tryLock(long time, TimeUnit unit)    **ָ��ʱ���ڵȴ���ȡ��**
 - unlock()      **�ͷ���**
 - newCondition()   **����һ���󶨵��� Lock ʵ���ϵ� Condition ʵ��**

����Lock�ӿڵ�ʵ�֣�������Ҫ�ǹ�ע���������ࣺ

 - ReentrantLock
 - ReentrantReadWriteLock
 
### ReentrantLock
������������ν�Ŀ���������Ҳ�еݹ�������ָһ���̻߳�ȡ�����ٴλ�ȡ����ʱ������Ҫ���µȴ���ȡ��ReentrantLock��Ϊ��ƽ���ͷǹ�ƽ������ƽ��ָ�����ϸ��������ȵõ�˳���Ŷӵȴ�ȥ��ȡ�������ǹ�ƽ��ÿ�λ�ȡ��ʱ������ֱ�ӳ��Ի�ȡ������ȡ�������ٰ��������ȵõ�˳���Ŷӵȴ���

ע�⣺ReentrantLock��synchronized���ǿ���������

### ReentrantReadWriteLock
�������д����ָ����û���߳̽���д����ʱ������߳̿�ͬʱ���ж������������߳̽���д����ʱ��������д����ֻ�ܵȴ���������-���ܹ��棬��-д���ܹ��棬д-д���ܹ��桱��
�ڶ�����д������£���д���ܹ��ṩ�����������õĲ����Ժ���������

### Condition
Condition��������Lock���󴴽��ģ�һ��Lock������Դ������Condition����ʵLock��Condition���ǻ���AQSʵ�ֵġ�
Condition������Ҫ�����̵߳ĵȴ��ͻ��ѣ���JDK 5֮ǰ���̵߳ĵȴ���������Object�����wait/notify/notifyAll����ʵ�ֵģ�ʹ���������Ǻܷ��㣻
��JDK5֮��J.U.C���ṩ��Condition�����У�

 - Condition.await��Ӧ��Object.wait��
 - Condition.signal ��Ӧ�� Object.notify��
 - Condition.signalAll ��Ӧ�� Object.notifyAll��

ʹ��Condition������һ���Ƚ����Եĺô���һ�������Դ������Condition�������ǿ��Ը�ĳ���̷߳���һ��Condition��Ȼ��Ϳ��Ի����ض�����̡߳�
 
## Synchronizers��ͬ������
 J.U.C�е�ͬ������Ҫ����Э���߳�ͬ�������������֣�
 
 - ���� CountDownLatch
 - դ�� CyclicBarrier
 - �ź��� Semaphore
 - ������ Exchanger

### ���� CountDownLatch
������Ҫ������һ�����̵߳ȴ�һ���¼����������ִ�У�������¼���ʵ����ָCountDownLatch�����countDown������ע�������̵߳�����countDown�������ǻ����ִ�еģ���������ͼ��ʾ��

![�˴�����ͼƬ������](images/multi-thread-count-down-latch.webp)

��CountDownLatch�ڲ�������һ����������һ��ʼ��ʼ��Ϊһ���������¼�������������һ���¼��󣬵���countDown��������������1��await���ڵȴ�������Ϊ0�����ִ�е�ǰ�̣߳�
����ͼ��TA���̻߳�һֱ�ȴ���ֱ������cnt=0,�ż���ִ�У�

### դ�� CyclicBarrier
դ����Ҫ���ڵȴ������̣߳��һ������Լ���ǰ�̣߳������̱߳���ͬʱ����դ��λ�ú󣬲��ܼ���ִ�У����������̵߳���դ���������Դ���ִ������һ��Ԥ�����õ��̣߳���������ͼ��ʾ��

![�˴�����ͼƬ������](images/multi-thread-cyclic-barrier.png)

����ͼ�У�T1��T2��T3ÿ����һ��await�������������������ǵ���await������ʱ�����������Ϊ0���������Լ����̣߳�
���⣬TA�̻߳��������̵߳���դ����������Ϊ0����ʱ�򣬲ſ�ʼִ�У�

### �ź���Semaphore
�ź�����Ҫ���ڿ��Ʒ�����Դ���̸߳�������������ʵ����Դ�أ������ݿ����ӳأ��̳߳�...
��Semaphore�У�acquire�������ڻ�ȡ��Դ���еĻ�������ִ�У�ʹ�ý����󣬼ǵ��ͷ���Դ����û����Դ�Ļ�������ֱ���������̵߳���release�����ͷ���Դ��

![�˴�����ͼƬ������](images/multi-thread-semaphore.png)

### ������ Exchanger
��������Ҫ�����߳�֮��������ݽ�����
�������̶߳����ﹲͬ��ͬ���㣨��ִ�е�exchanger.exchange��ʱ�̣�ʱ���������ݽ����������ȴ�ֱ�������̵߳��

![�˴�����ͼƬ������](images/multi-thread-exchanger.webp)

## Atomic Variables��ԭ�ӱ�����
ԭ�ӱ�����Ҫ�Ƿ������Ա�ڶ��̻߳����£������Ľ���ԭ�Ӳ�����

ԭ�����ǻ���Unsafeʵ�ֵİ�װ�࣬���Ĳ�����CASԭ�Ӳ�������ν��CAS��������compare and swap��ָ���ǽ�Ԥ��ֵ�뵱ǰ������ֵ�Ƚ�(compare)����������ʹ����ֵ�滻(swap)��ǰ���������������������ǿ���ժȡһ��AtomicInteger��Դ�룬���£�

```
    public final boolean compareAndSet(int expect, int update) {
        return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
    }
```
    
��compareAndSwapInt�����У�valueOffset���ڴ��ַ��expect��Ԥ��ֵ��update�Ǹ���ֵ�����valueOffset��ַ����ֵ��Ԥ��ֵ��ȣ���valueOffset��ַ����ֵ����Ϊupdateֵ��PS:�ִ�CPU�ѹ㷺֧��CASָ�

��Java�У�������ԭ�Ӹ��·�ʽ�����£�

 - ԭ�ӷ�ʽ���»������ͣ� AtomicInteger �� AtomicLong ��
 - ԭ�ӷ�ʽ�������飻 AtomicIntegerArray�� AtomicLongArray��
 - ԭ�ӷ�ʽ�������ã� AtomicReference�� AtomicReferenceFieldUpdater��
 - ԭ�ӷ�ʽ�����ֶΣ� AtomicIntegerFieldUpdater�� AtomicStampedReference(���CAS��ABA����)

### ABA����
`AtomicStampedReference`�� `AtomicMarkableReference`��ͨ���汾�ţ�ʱ����������ABA����ģ�����Ҳ����ʹ�ð汾�ţ�verison�������ABA��
���ֹ���ÿ����ִ�����ݵ��޸Ĳ���ʱ���������һ���汾�ţ�һ���汾�ź����ݵİ汾��һ�¾Ϳ���ִ���޸Ĳ������԰汾��ִ��+1�����������ִ��ʧ�ܡ�

�ײ���ͨ��ʵ�ʵ�CAS�����Ƚϵ��ǵ�ǰ��pair������½���pair����pair�����װ�����ú�ʱ�����Ϣ��

## BlockingQueue���������У�
���������ṩ�˿���������Ӻͳ��Բ���������������ˣ���Ӳ���������ֱ���пռ���ã�������п��ˣ����Ӳ���������ֱ����Ԫ�ؿ��ã�

![�˴�����ͼƬ������](images/multi-thread-blocking-queue.jpg)

 ��Java�У���Ҫ���������͵��������У�

 - ArrayBlockingQueue ��һ��������ṹ��ɵ��н��������С�
 - LinkedBlockingQueue ��һ��������ṹ��ɵ��н��������С�
 - PriorityBlockingQueue ��һ��֧�����ȼ�������޽��������С�
 - DelayQueue��һ��֧����ʱ��ȡԪ�ص��޽��������С�
 - SynchronousQueue��һ�����洢Ԫ�ص��������С�
 - LinkedTransferQueue��һ��������ṹ��ɵ��޽��������С�
 - LinkedBlockingDeque��һ��������ṹ��ɵ�˫���������С�

����������һ���Ƚϵ��͵�Ӧ�ó����ǽ��������-����������

## Concurrent Collections������������
����������������һ�¹����бȽϳ�����һ�����ݣ�����������
˵���������������ò���ͬ����������JDK5֮ǰ��Ϊ���̰߳�ȫ������һ�㶼��ʹ��ͬ��������ͬ��������Ҫ������ȱ�㣺

 - ͬ����������������״̬�ķ��ʶ����л������ؽ����˲����ԣ�
 - ĳЩ���ϲ�������Ȼ��Ҫ����������
 - �����ڼ䣬�������̲߳����޸ĸ����������׳�ConcurrentModificationException�쳣��������ʧ�ܻ���

���ڸ��ϲ��������ǿ��Ծٸ�����, ��Ϊ�Ƚ����ױ����ӣ����´��룺

```
    public static  Integer getLast(Vector<Integer> list){
        int lastIndex = list.size() - 1;
        if(lastIndex < 0) return null;
        return list.get(lastIndex);
    }
```

�����ϴ����У���Ȼlist������Vector���ͣ����÷�����Ȼ����ԭ�Ӳ�������Ϊ��list.size()��list.get(lastIndex)֮�䣬�����Ѿ������˺ܶ��¡�
��ô����JDK 5֮������Щ���������أ�������Ҫ˵���֣����£�

 - ConcurrentHashMap
 - CopyOnWriteArrayList/Set

### ConcurrentHashMap
ConcurrentHashMap�ǲ��÷�������������ͬ�������У���һ������һ����������ConcurrentHashMap�У��Ὣhash������鲿�ֳַ����ɶΣ�ÿ��ά��һ��������Щ�ο��Բ����Ľ���д�������Դﵽ��Ч�Ĳ������ʣ�����ͼʾ����
![�˴�����ͼƬ������](images/multi-thread-concurrenthashmap.png)

### CopyOnWriteArrayList/Set
Ҳ�п���������ָ����д���ݵ�ʱ�����¿���һ�ݽ���д��������ɺ��ٽ�ԭ����������ָ���µĿ���������

***�����������������ԶԶ����д������ʱ�򣬿���������������ϡ�***

## Fork/Join���м�����
�����������JDK7������ģ����˾����൱ţ�ƣ����Է������ö��ƽ̨�ļ����������򻯲��г���ı�д��������Ա�����ע��λ������������м�����

fork/join��ܵĺ�����ForkJoinPool�࣬ʵ���˹�����ȡ�㷨������Щ����������������̣߳���������߳���ȡ����ִ�У������ܹ�ִ�� ForkJoinTask����

���ó������������ܱ��ݹ��ֳɶ���������Ӧ�ã�
���Բο���ͼ��������⣬λ��ͼ�ϲ��� Task ������λ�����µ� Task ��ִ�У�ֻ�е����е����������֮�󣬵����߲��ܻ�� Task 0 �ķ��ؽ������ʵ����һ�ֶַ���֮��˼�룺
![�˴�����ͼƬ������](images/multi-thread-fork-join.webp)
��ʵ����ʹ��fork/join��ܵĿ�����Ա��˵����Ҫ�������������񻮷֣����Բο�����α���룺
```
    if (�����㹻С) {
      ֱ��ִ�и�����;
    } else {
      �������ֳɶ��������;
      ִ����Щ�����񲢵ȴ����;
    }
```


### ����(deadlock)
�������̶��ڵȴ��Է�ִ����ϲ��ܼ�������ִ�е�ʱ��ͷ�������������������������̶����������޵ĵȴ��С�
����������
```
    public class DieLockDemo {
        public static void main(String[] args) {
            DieLock dl1 = new DieLock(true);
            DieLock dl2 = new DieLock(false);
            
            dl1.start();
            dl2.start();
        }
    }
    
    public class DieLock extends Thread {
        private boolean flag;
        public DieLock(boolean flag) {
            this.flag = flag;
        }
        @Override
        public void run() {
            if (flag) {
                synchronized (MyLock.objA) {
                    System.out.println("if objA");
                    synchronized (MyLock.objB) {
                        System.out.println("if objB");
                    }
                }
            } else {
                synchronized (MyLock.objB) {
                    System.out.println("else objB");
                    synchronized (MyLock.objA) {
                        System.out.println("else objA");
                    }
                }
            }
        }
    }
```

 - ����״̬��dl1�߳�Ϊtrue��ifִ���ȴ��"if objA"Ȼ���ٽ��Ŵ��"if objB"֮���ͷ�A��B��������֮��dl2�߳�ִ��else�����"else objB"��"else objA"��
 - ������״̬��dl1�ȴ��"if objA"��֮���߳�dl2ִ�д��"else objB"��Ȼ��1��2�̵߳�������A��B�����ڱ�����״̬�������߳���������������������

## �߳�ͬ����ͨѶ
### �߳�ͬ��
��ʹ�ö���߳�������ͬһ������ʱ���ǳ����׳����̰߳�ȫ����(�������̶߳��ڲ���ͬһ���ݵ������ݲ�һ��),����������ͬ�������������Щ���⡣

ʵ��ͬ������������������

 - ͬ������飺
    synchronized(ͬһ������){} ͬһ�����ݣ�����N���߳�ͬʱ����һ�����ݡ�
 - ͬ��������
    public synchronized ���ݷ������� ������(){}����ʹ�� synchronized ������ĳ����������÷�����Ϊͬ������������ͬ���������ԣ�������ʾָ��ͬ����������ͬ��������ͬ���������� this Ҳ���Ǹö���ı�������ָ�Ķ������е㺬������ʵ���ǵ��ø�ͬ�������Ķ���ͨ��ʹ��ͬ���������ɷǳ�����Ľ�ĳ�����̰߳�ȫ����

### �߳�ͨѶ
��ʹ��synchronized ������ĳ��������Դʱ(��ͬ��������ͬ���������������,��ĳ���̻߳�ù�����Դ������Ϳ���ִ����Ӧ�Ĵ���Σ�ֱ�����߳�������ô���κ���ͷŶԸ� ������Դ�������������߳��л���ִ�жԸù�����Դ���޸ġ���ĳ���߳�ռ��ĳ��������Դ����ʱ���������һ���߳�Ҳ������������о���Ҫʹ��wait() ��notify()/notifyAll()�����������߳�ͨѶ�ˡ�

## interrupt(),interrupted()��isInterrupted()������

 - ### interrupt()
 ���ܿ���ȥ�Ǹ÷����������ж��̵߳ģ�����ʵ���ϣ�����ʹ��Ч��������ֹͣһ���������е��̣߳� 
���������߳��д���һ��ֹͣ�ı�Ƕ��ѣ���Ҫ����һ���жϲ�ʵ��ֹͣ�̵߳Ĺ��ܡ�
```
    /*
 - ����interrupt()����
 */
public class Test_interrupt implements Runnable {

    @Override
    public void run() {
        for(int i=0; i<500000; i++){
            System.out.println("i=: "+i);
        }
    }

    public static void main(String[] args) {
        try {
            Test_interrupt r = new Test_interrupt();
            Thread t = new Thread(r,"test");
            t.start();
            Thread.sleep(1000);
            t.interrupt();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }  
    }
}
//������߳�һֱ��ӡ����δ��ֹ
```

 - ### interrupted()
 ��⵱ǰ�߳��Ƿ��жϣ������ǰ�̴߳����ж�״̬�����״ε��ø÷���ʱ�᷵��true����ʶ��ǰ�߳��Ѿ��жϣ� 
�÷���������ִ�к�**���״̬��ʶ**�Ĺ��ܣ�������������ε��ø÷��������һ�λ᷵��true���ڶ��λ᷵��false��
```
/*
 * ����interrupted()����
 */
public class Test_interrupted implements Runnable {

    @Override
    public void run() {
        for(int i=0; i<500000; i++){
            System.out.println("i=: "+i);
        }
    }

    public static void main(String[] args) {

        Test_interrupted r = new Test_interrupted();
        Thread t = new Thread(r,"test");
        t.start();     
        Thread.currentThread().interrupt();    //�жϵ�ǰmain�߳�, ����Thread.interrupted()�ķ���ֵ

        System.out.println("�Ƿ�ֹͣ1?: "+Thread.interrupted());
        System.out.println("�Ƿ�ֹͣ2?: "+Thread.interrupted());        
    }
}

//�����
> �Ƿ�ֹͣ1?: true
> �Ƿ�ֹͣ2?: false
```

 - ### isInterrupted()
 �����߳��Ƿ��жϣ��̵߳��ж�״̬�����ܵ��÷�����Ӱ�죬���������ø÷�������ִ�к�**�������״̬��ʶ**��
```
    /*
 * ����interrupt()����
 */
public class Test_interrupted implements Runnable {

    @Override
    public void run() {
        for(int i=0; i<500000; i++){
            System.out.println("i=: "+i);
        }
    }

    public static void main(String[] args) {

        Test_interrupted r = new Test_interrupted();
        Thread t = new Thread(r,"test");
        t.start();
        t.interrupt();    //�ж��߳�t, ����t.isInterrupted()�ķ���ֵ

        System.out.println("�Ƿ�ֹͣ1?: "+t.isInterrupted());
        System.out.println("�Ƿ�ֹͣ2?: "+t.isInterrupted());
    }
}
//�����
> �Ƿ�ֹͣ1?: true
> �Ƿ�ֹͣ2?: true
```
## synchronizedԭ��
### 3��ʹ�÷���

- ����ʵ�������������ڵ�ǰʵ������������ͬ������ǰҪ��õ�ǰʵ������
- ���ξ�̬�����������ڵ�ǰ��������������ͬ������ǰҪ��õ�ǰ��������
- ���δ���飬ָ���������󣬶Ը����������������ͬ�������ǰҪ��ø����������

### �����ڴ沼��
![�˴�����ͼƬ������](images/multi-thread-object-head-structure.png)

 - ʵ������������������������Ϣ�����������������Ϣ������������ʵ�����ֻ���������ĳ��ȣ��ⲿ���ڴ水4�ֽڶ��롣

 - ������ݣ����������Ҫ�������ʼ��ַ������8�ֽڵ���������������ݲ��Ǳ�����ڵģ�������Ϊ���ֽڶ��룬����˽⼴�ɡ�

### ����ͷ�ṹ
�����λ��	 | ͷ����ṹ | ˵��
:-: | :-: | :-:
32/64bit | Mark Word | �洢�����hashCode������Ϣ��ִ������GC��־����Ϣ
32/64bit | Class Metadata Address| ����ָ��ָ��������Ԫ���ݣ�JVMͨ�����ָ��ȷ���ö������ĸ����ʵ��
 
 ���ڶ���ͷ��������ʶָ��ľ���monitor�������ʼ��ַ�����ÿ��������һ��monitor�����������synchronized��ʵ�ʾ���ͨ���̻߳�ȡ�����monitorʵ�ֵġ�
 
### synchronized�����ײ�ԭ��
 ���Դӷ�������ֽ����п�֪ͬ�������ʵ��ʹ�õ���`monitorenter` �� `monitorexit` ָ�����monitorenterָ��ָ��ͬ�������Ŀ�ʼλ�ã�monitorexitָ����ָ��ͬ�������Ľ���λ��
 
### synchronized�����ײ�ԭ��
 ��������ͬ������ʽ��������ͨ���ֽ���ָ�������Ƶģ���ʵ���ڷ������úͷ��ز���֮�С�JVM���Դӷ����������еķ�����ṹ(method_info Structure) �е� `ACC_SYNCHRONIZED` ���ʱ�־����һ�������Ƿ�ͬ������������������ʱ������ָ��� ��鷽���� ACC_SYNCHRONIZED ���ʱ�־�Ƿ����ã���������ˣ�ִ���߳̽��ȳ���monitor��������淶���õ��ǹܳ�һ�ʣ��� Ȼ����ִ�з���������ٷ������(������������ɻ��Ƿ��������)ʱ�ͷ�monitor��
 
### ���ķ���
 
 **ƫ����**
 ƫ�����ĺ���˼���ǣ����һ���̻߳����������ô���ͽ���ƫ��ģʽ����ʱMark Word �ĽṹҲ��Ϊƫ�����ṹ��������߳��ٴ�������ʱ�����������κ�ͬ������������ȡ���Ĺ��̣�������ʡȥ�˴����й�������Ĳ������Ӷ�Ҳ����߳�������ܡ�
 
 **������**
 ���������ܹ������������ܵ������ǡ��Ծ��󲿷ֵ�����������ͬ�������ڶ������ھ�������ע�����Ǿ������ݡ���Ҫ�˽���ǣ�������������Ӧ�ĳ������߳̽���ִ��ͬ����ĳ��ϣ��������ͬһʱ�����ͬһ���ĳ��ϣ��ͻᵼ��������������Ϊ����������
 
 **������**
 �����ڲ��ý�������ǰ���߳̿��Ի�����������������õ�ǰ��Ҫ��ȡ�����߳���������ѭ��(��Ҳ�ǳ�Ϊ������ԭ��)��һ�㲻��̫�ã�������50��ѭ����100ѭ�����ھ������ɴ�ѭ��������õ�������˳�������ٽ�������������ܻ�������ǾͻὫ�߳��ڲ���ϵͳ���������������������Ż���ʽ

### �ȴ����ѻ�����synchronized

 - notify/notifyAll��wait��������ʹ����3������ʱ�����봦��synchronized��������synchronized�����У�����ͻ��׳�IllegalMonitorStateException�쳣��������Ϊ�����⼸������ǰ�����õ���ǰ����ļ�����monitor����Ҳ����˵notify/notifyAll��wait����������monitor����
 - ��sleep������ͬ����wait����������ɺ��߳̽�����ͣ����wait���������ͷŵ�ǰ���еļ�������(monitor)��ֱ�����̵߳���notify/notifyAll�������ܼ���ִ�У���sleep����ֻ���߳����߲����ͷ�����
 - ͬʱnotify/notifyAll�������ú󣬲����������ͷż�����������������Ӧ��synchronized(){}/synchronized����ִ�н�������Զ��ͷ�����

## Lock�ĵײ�ʵ��ԭ��
��������Lock��Ҫ��ͨ������������ʵ�ֵķֱ���CAS��AQS(AbstractQueuedSynchronizer)��

 1. ��ȡ��ʾ��״̬�ı���
 2. �����ʾ״̬�ı�����ֵΪ0����ô��ǰ�̳߳��Խ�����ֵ����Ϊ1��ͨ��CAS������ɣ���������߳�ͬʱ����ʾ״̬�ı���ֵ��0���ó�1ʱ����һ���߳��ܳɹ��������̶߳���ʧ�ܡ�ʧ�ܺ�����������ת��������ǰ�̡߳�
 3. �����ʾ״̬�ı�����ֵΪ1����ô����ǰ�̷߳���ȴ������У�Ȼ����������

## ���̼��ͨ�ŷ�ʽ

 - �ܵ�( pipe )���ܵ���һ�ְ�˫����ͨ�ŷ�ʽ������ֻ�ܵ�������������ֻ���ھ�����Ե��ϵ�Ľ��̼�ʹ�á����̵���Ե��ϵͨ����ָ���ӽ��̹�ϵ��
 - �����ܵ� (namedpipe) �� �����ܵ�Ҳ�ǰ�˫����ͨ�ŷ�ʽ����������������Ե��ϵ���̼��ͨ�š�
 - �ź���(semophore ) ���ź�����һ���������������������ƶ�����̶Թ�����Դ�ķ��ʡ�������Ϊһ�������ƣ���ֹĳ�������ڷ��ʹ�����Դʱ����������Ҳ���ʸ���Դ����ˣ���Ҫ��Ϊ���̼��Լ�ͬһ�����ڲ�ͬ�߳�֮���ͬ���ֶΡ�
 - ��Ϣ����( messagequeue ) �� 
��Ϣ����������Ϣ������������ں��в�����Ϣ���б�ʶ����ʶ����Ϣ���п˷����źŴ�����Ϣ�١��ܵ�ֻ�ܳ����޸�ʽ�ֽ����Լ���������С���޵�ȱ�㡣
 - �ź� (signal ) �� �ź���һ�ֱȽϸ��ӵ�ͨ�ŷ�ʽ������֪ͨ���ս���ĳ���¼��Ѿ�������
 - �����ڴ�(shared memory )�������ڴ����ӳ��һ���ܱ��������������ʵ��ڴ棬��ι����ڴ���һ�����̴�������������̶����Է��ʡ������ڴ������� IPC��ʽ����������������̼�ͨ�ŷ�ʽ����Ч�ʵͶ�ר����Ƶġ�������������ͨ�Ż��ƣ����ź��������ʹ�ã���ʵ�ֽ��̼��ͬ����ͨ�š�
 - �׽���(socket ) �� �׽��Ҳ��һ�ֽ��̼�ͨ�Ż��ƣ�������ͨ�Ż��Ʋ�ͬ���ǣ��������ڲ�ͬ�����Ľ���ͨ�š�

## �̼߳��ͨ�ŷ�ʽ��ͬ����

 - �����ƣ�������������������������д��

  - �������ṩ����������ʽ��ֹ���ݽṹ�������޸ĵķ�����
  - ��д���������߳�ͬʱ���������ݣ�����д�����ǻ���ġ�
  - ��������������ԭ�ӵķ�ʽ�������̣�ֱ��ĳ���ض�����Ϊ��Ϊֹ���������Ĳ������ڻ������ı����½��еġ���������ʼ���뻥����һ��ʹ�á�while+if+volatile����
 - �ź�������(Semaphore)�����������߳��ź����������߳��ź���
 - �źŻ���(Signal)�����ƽ��̼���źŴ���
  
## �̵߳�״̬
![�˴�����ͼƬ������](images/multi-thread-thread-life-circle.png)


## Q&A
 ### sleep() �� wait() ��ʲô����?
sleep���߳��ࣨThread���ķ��������´��߳���ִͣ��ָ��ʱ�䣬��ִ�л���������̣߳����Ǽ��״̬��Ȼ���֣���ʱ����Զ��ָ�������sleep�����ͷŶ�������
wait��Object��ķ������Դ˶������wait�������±��̷߳���������������ȴ��˶���ĵȴ������أ�ֻ����Դ˶��󷢳�notify��������notifyAll�����̲߳Ž������������׼����ö�������������״̬��
 ### ʵ�ֵ���ģʽ�ļ��ַ���
```
//˫�ؼ����
public class Singleton() {
        private Singleton(){};
        private volatile static Singleton instance;
        public static Singleton getInstance() {
          if (instance == null)
          {
            synchronized(Singleton.class) {  //1
              if (instance == null)          //2
                instance = new Singleton();  //3
            }
          }
          return instance;
        }
    }
    
//��̬�ڲ���
public class Singleton {

    private Singleton() {}

    private static class SingletonInstance {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonInstance.INSTANCE;
    }
}

//ö��
 public enum Singleton {
        INSTANCE;
        public void whateverMethod() {
    
        }
}
```

### �߳�start()��run()����������
```
    //start()����
    public static void main(String args[]) {
        Thread t = new Thread() {
            public void run() {
                pong();
            }
        };
        t.start();
        System.out.print("ping");
    }
 
    static void pong() {
        System.out.print("pong");
    }
    //���:pingpong
```

```
    //run()����
    public static void main(String args[]) {
        Thread t = new Thread() {
            public void run() {
                pong();
            }
        };
        t.run();
        System.out.print("ping");
    }
 
    static void pong() {
        System.out.print("pong");
    }
    //���:pongping
```

### ��α�������

 - ȷ������˳��
 - ���ü�����ʱ
 - ������⣺ÿ��һ���̻߳�������������̺߳�����ص����ݽṹ�У�map��graph�ȵȣ�������¡�����֮�⣬ÿ�����߳���������Ҳ��Ҫ��¼��������ݽṹ�С���һ���߳�������ʧ��ʱ������߳̿��Ա������Ĺ�ϵͼ�����Ƿ�������������