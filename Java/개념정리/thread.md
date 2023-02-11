

## 스레드

### What is Thread In java? 

```
A thread is a thread of execution in a program. The Java Virtual Machine allows an application to have multiple threads of execution running concurrently.
```

여러 개의 스레드를 동시에 실행시킬 수 있게 해준다.

### 스레드 상태


|개념 | 의미 |
|----|----|
|NEW | 스레드가 Start() 메소드를 호출하기 전 상태|
|RUNABLE| 작업준비를 마쳤지만 다른 thread 가 우선적으로 실행되고 있는 상태를 의미 |
|RUNNING| 특정 스레드가 실행되고 있는 상태 (스레드1 과 스레드2가 동시에 실행중이지만 스레드2가 작업중일 경우 스레드2의 상태를 일컫는다.)
|BLOCK| 외부의 데이터베이스나, 어떤 입력을 위해 대기하고 있거나, 실행이 완료되지 않은 다른 thread 로부터 데이터를 입력받아야 하는 상황을 말한다.|
|TERMINATED/DEAD| 모두 실행이 완료되었을 경우를 의미한다.|

### 스레드 예시 코드

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