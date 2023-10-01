+++
author = "IceBlueHalls"
title = "[C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part4: 게임 서버 - 정리"
date = "2023-01-23"
description = "[C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part4: 게임 서버 강의를 듣고 배운점 정리"
tags = [
    "CSharp",
    "Unity",
    "GameServer"
]
categories = [
    "CSharp",
    "Unity",
    "GameServer"
]
series = ["inflearn-gamer-server"]
aliases = ["inflearn-gamer-server"]
slug = "inflearn-gamer-server4"
+++

## 멀티쓰레드
쓰레드는 크게 3개 정도로 사용 가능
* Thread
* ThreadPool
* Task

### Thread
일반적인 쓰레드. 한번 사용하려면 할당을 해야한다.

**코드**
```csharp
void OneThread() {
    while(true) {
        Console.WriteLine("실행중입니다");
    }
}

static void Main(string[] args) {
    Thread t = new Thread(OneThread);
    t.Start();

    Console.WriteLine("Hello World");
}
```

**결과**
```
실행중입니다
실행중입니다
실행중입니다
실행중입니다
Hello World
실행중입니다
```

이것은 쓰레드의 성능에 따라 달라질 수 있다. 어쨌든 병행처리이다.

### ThreadPool
쓰레드풀은 여러 쓰레드의 최대갯수를 지정하는 등의 조절하는 역할을 해준다.  
하지만 변수에 object state를 만들어줘야한다.

```csharp
void OneThread(object state) {
    while(true) {
        Console.WriteLine("실행중입니다");
    }
}

static void Main(string[] args) {
    ThreadPool.SetMaxThreads(4,4);
    ThreadPool.QueueUserWorkItem(OneThread);
    ThreadPool.QueueUserWorkItem(OneThread);
    ThreadPool.QueueUserWorkItem(OneThread);
    ThreadPool.QueueUserWorkItem(OneThread);

    ThreadPool.QueueUserWorkItem((obj) => {
        Console.WriteLine("끝났나요?");
    });

    t.Start();

    Console.WriteLine("Hello World");
}
```

**결과**
```
실행중입니다
실행중입니다
실행중입니다
실행중입니다
실행중입니다
```

이 경우 TheadPool은 최대 4개까지만 가능하므로, 마지막에 삽입한 끝났나요 호출 쓰레드는 작동하지 않는다.

### Task
Thead의 진화판. ThreadPool의 설정을 공유한다.
```csharp
using System.Threading.Tasks;
static void Main(string[] args) {
    ThreadPool.SetMaxThreads(4,4);
    Task t = new Task( () => {while (true) {}});
    t.Start();

    ThreadPool.QueueUserWorkItem((obj) => {
        Console.WriteLine("1");
    });

    ThreadPool.QueueUserWorkItem((obj) => {
        Console.WriteLine("2");
    });

    ThreadPool.QueueUserWorkItem((obj) => {
        Console.WriteLine("3");
    });

    ThreadPool.QueueUserWorkItem((obj) => {
        Console.WriteLine("4");
    });

}
```

**결과**
```
실행중입니다
1
실행중입니다
2
실행중입니다
실행중입니다
3
4
```

## 컴파일러 최적화
Debug 모드에서 Release 모드로 바뀔 때 컴파일러는 자동으로 최적화하여 빌드한다. 그러나 컴퓨터는 정확한 쓰임새를 모르기 떄문에 잘못된 내용으로 코드를 최적화시킬 수 있다.
```csharp
static isStop = false;

void InfinityLoop() {
    while(isStop == false) {
        Console.Write("안멈출거지롱~~");
    }
}

void Start() {
    Task t = new Task(InfinityLoop);
    t.Start();

    isStop = true;
    
    t.Join();
    Console.WriteLine("끝났습니다");
}
```

**Debug Mode**
```
안멈출거지롱~~
안멈출거지롱~~
끝났습니다
```

**Release Mode**
```
안멈출거지롱~~
안멈출거지롱~~
안멈출거지롱~~
안멈출거지롱~~
```

이러한 이유는 컴파일러가 Release로 최적화를 하면사 다음과 같이 로직을 변경했기 떄문이다.

```csharp
void InfinityLoop() {
    if(isStop == false) {
        while(true) {
            Console.Write("안멈출거지롱~~");
        }
    }
}
```

이러한 값의 변경을 막고자 할때에는 **volatile** bool isStop = false;를 통해 volatile 키워드를 붙여서 컴퓨터 멋대로 바꾸지 못하게 당부해야한다.

## 캐시
CPU 코어에서는 더 빠르게 데이터를 처리하기 위해 캐시라는 개념을 사용한다.  
캐시는 자주쓰는 데이터를 메인 저장공간에 저장하고 꺼내쓰는 식이 아닌, cpu가 메인까지 가지 않고 소지하여 계속 사용하면서 메인 메모리까지의 보내는 것을 단축시키는 역할을 한다.  
강의 예시에서는 테이블의 주문이 있을 때 주문을 받고 바로 주방에 알리는 식이 아닌 테이블을 돌아다니며 주문을 받아 적고 한번에 주방장에게 주는 방식이다.  
다만 직원이 많을 경우에는 각자 가지고 있는 주문표가 겹칠 수도 있으니 꼬일 수도 있다.

캐시는 두가지 철학이 있다.
* 시간적 캐싱(TEMPORAL LOCALITY) : 해당 주소를 사용한 직후 해당 주소값의 데이터를 계속 사용할 수 있으므로 캐싱한다.
* 공간적 캐싱(SAPCIAL LOCALITY) : 해당 주소를 사용한 주변 주소를 또 사용할 가능성이 있으므로 해당 주소를 캐싱한다.(배열등 인접한 주소 사용시 유용)

## 메모리 배리어
하드웨어에서도 메모리 최적화 작업이 일어나면서 자기 멋대로 코드를 바꾼다.

```csharp
static volatile int x = 0;
static volatile int y = 0;
static volatile int r1 = 0;
static volatile int r2 = 0;

static void Thread1() {
    y = 1;
    r1 = x;
}

static void Thread2() {
    x = 1;
    r2 = y;
}

static void Main(string[] args) {
    while(true) {
        x = y= r1 = r2 = 0;

        Task t1 = new Task(Thread1);
        Task t2 = new Task(Thread2);
        t1.Start();
        t2.Start();

        Task.WaitAll(t1, t2);

        if(r1 == 0 && r2 == 0) {
            break;
        }
    }

    Console.WriteLine("빠져나왔습니다");
}
```

위 코드는 'x, y 값이 1이 되었고 이 값을 대입했으니 r1이든 r2이든 1이 되어서 무한루프가 돌 것이다' 라고 생각이 되지만  
하드웨어상에서 돌릴 때 y하고 r1하고는 상관이 없네? 그럼 r1 = x 먼저하고 y = 1해도 되겠다 싶어서 순서를 바꾸기도 한다.
그렇게 되면 각 쓰레드에서는 0값이 먼저 대입되고 그 다음에 x,y에 1이 대입되므로 순서가 꼬인다.

이 것을 꼬이지 않게 하려면? "메모리 배리어"를 사용하면 된다.
```csharp
static void Thread1() {
    y = 1;
    Thread.MemoryBarrier();
    r1 = x;
}

static void Thread2() {
    x = 1;
    Thread.MemoryBarrier();
    r2 = y;
}
```

Thread.MemoryBarrier()는 캐싱되어 있는 메모리를 동기화시키는 작업으로, 식당으로 치면 기존에 가지고 있던 자신이 받은 주문 쪽지를 주방과 동기화 시키고 다시 오는 것이다.
그럼 다른 쓰레드(직원)이 이를 알아채고 동기화가 되면서 순차적으로 호출이 된다.

