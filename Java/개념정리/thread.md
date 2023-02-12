

## 스레드

### What is Thread In java? 

```
A thread is a thread of execution in a program. The Java Virtual Machine allows an application to have multiple threads of execution running concurrently.
```

여러 개의 스레드를 동시에 실행시킬 수 있게 해준다.

### 스레드 상태 정리


|개념 | 의미 |
|----|----|
|NEW | 스레드가 Start() 메소드를 호출하기 전 상태|
|RUNABLE| 작업준비를 마쳤지만 다른 thread 가 우선적으로 실행되고 있는 상태를 의미 |
|RUNNING| 특정 스레드가 실행되고 있는 상태 (스레드1 과 스레드2가 동시에 실행중이지만 스레드2가 작업중일 경우 스레드2의 상태를 일컫는다.)
|BLOCK| 외부의 데이터베이스나, 어떤 입력을 위해 대기하고 있거나, 실행이 완료되지 않은 다른 thread 로부터 데이터를 입력받아야 하는 상황을 말한다.|
|TERMINATED/DEAD| 모두 실행이 완료되었을 경우를 의미한다.|

### 스레드 기본 코드

- 스레드의 처리를 개발자가 직접 처리해줘야 한다.

```java

class Task1 extends Thread {
    public void run() {
        System.out.println("task1 Start");
        for (int i = 100; i <= 199; i++) {
            System.out.print(i + " ");
        }
        System.out.println("\n task1 done");
    }
}

class Task2 extends Thread {
    public void run() {
        System.out.println("task2 Start");
        for (int i = 200; i <= 299; i++) {
            System.out.print(i + " ");
        }
        System.out.println("\n task2 done");
    }
}

public class ThreadExample1 {
    public static void main(String[] args) throws InterruptedException {

        // task1
        Task1 task1 = new Task1();
        task1.start(); // 스레드가 실행된 상태

        task1.setPriority(1); // 스레드의 우선순위 설정 (추천사항 반드시 우선순위대로 실행되지 않는다.)
        Task2 task2 = new Task2();
        task2.start(); 


        task1.join(); // 해당 스레드가 끝날 떄 까지 기달린다. 

        System.out.println("task3 Start");
        for (int i = 300; i <= 399; i++) {
            System.out.print(i + " ");
        }
        System.out.println("\n task3 done");
    }
}


```

##  Executorservice 


***개발자가 스레드의 시작과  스레드 이후의 메소드 실행 시점을 직접 코드로 작성하지 안해도된다.*** <br/>
|메소드명| 의미| 
|---|---|
|shutdown | ExcutorService 에서 실행되는 스레드를 종료  |
|execute  | 스레드를 실행 시킨다.|
|newFixedThreadPool |  스레드풀을 생성하여 동시에 스레드가 몇개 실행될 수 있는지 컨트롤할 수 있게 해준다.  |
| submit | callable 구현체이 call Method 의 반환 객체를 **future** 객체로 반환받는다.|

**shutdown()** : ExcutorService 에서 실행되는 스레드를 종료 시켜버린다. <br/>

**execute()** : 스레드를 실행 시킨다.  <br/>

**newFixedThreadPool()**  : 스레드풀을 생성하여 동시에 스레드가 몇개 실행될 수 있는지 컨트롤할 수 있게 해준다.  <br/>


```java

/**
 * Executor Service
 * 스레드의 상태를 알 수 잇다.
 * 스레드의 실행을 컨트롤 할 수 있다.
 */
public class ThreadExample2 {

    static class task extends Thread {
        private final int number;

        public task(int number) {
            this.number = number;
        }

        public void run() {
            System.out.println("\n Start thread" + this.number);
            for (int i = this.number * 100 ; i < this.number * 100 +  100 ; i++) {
                System.out.print(" " + i);
            }
            System.out.println("\n end thread" + this.number);
        }
    }

    public static void main(String[] args) throws InterruptedException {

        /**
         *  ExecutorService
         *  - An Executor that provides
         *    methods to manage termination
         *    and methods that can produce a Future for tracking progress of  one or more asynchronous tasks.
         */
        ExecutorService executorService = Executors.newFixedThreadPool(3);
        executorService.execute(new task(1));
        executorService.execute(new task(2));
        executorService.execute(new task(3));
        executorService.execute(new task(4));
        executorService.execute(new task(5));
        executorService.execute(new task(6));
        
        executorService.shutdown();
    }
}


```

## Callable


A task that returns a result and may throw an exception. Implementors define a single method with no arguments called call. <br/>

The Callable interface is similar to Runnable, in that both are designed for classes
whose instances are potentially executed by another thread. A Runnable, however, does not return a result
and cannot throw a checked exception. <br/>

다른 스레드에서 실행 되도록 도와준다? (멀티 스레드를 도와주는 개념) <br/> <br/>


***예시 코드*** 

```java

class MultiTask implements Callable<String> {
    private final String name;

    public MultiTask(String name) {
        this.name = name;
    }

    @Override
    public String call() throws Exception {
        // 스레드를 실행하고 나서 다양한 과정이 포함되기 떄문에 대기 를 시켜줘야한다.
        Thread.sleep(1000);
        System.out.println("task Start " + this.name);
        return "Hello " + this.name;
    }
}


public class MultiCallable {
    public static void main(String[] args) throws InterruptedException, ExecutionException {
        // 스레드를 관리해주는 ExecutorService 를 생성 
        // 실행 시점과 스레드의 종료시점을 컨트롤 할 수 있다.
        ExecutorService executorService = Executors.newFixedThreadPool(1);
        // 여러 개의 Callable생성
        List<MultiTask> callableTasks = List.of(new MultiTask("yushin"), new MultiTask("hansung"), new MultiTask("min"));
        List<Future<String>> futures = executorService.invokeAll(callableTasks);

        for (Future<String> item : futures) {
            System.out.println("item -> " + item.get());
        }

        // 스레드 종료 
        executorService.shutdown();
    }
```
