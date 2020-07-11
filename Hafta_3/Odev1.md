
### This article describes the definition and uses of Task And Thread:
+ What is Task?
+ What is Thread?
+ Why do we need Task?
+ Why do we need Thread?
+ How to implement Task
+ How to implement Thread
+ Differences between Task And Thread

## What is Task in C#?
 
.NET framework provides Threading.Tasks class to let you create tasks and run them asynchronously. A task is an object that represents some work that should be done. The task can tell you if the work is completed and if the operation returns a result, the task gives you the result.

![image001](https://user-images.githubusercontent.com/61011022/85726303-9ff7ff80-b6fe-11ea-81a7-2ae0eb11cb36.gif)


## What is Thread?
 
.NET Framework has thread-associated classes in System.Threading namespace.  A Thread is a small set of executable instructions.

![image002](https://user-images.githubusercontent.com/61011022/85726416-bbfba100-b6fe-11ea-8e5a-24d4c6a57674.gif)


## Why we need Tasks
 
It can be used whenever you want to execute something in parallel. Asynchronous implementation is easy in a task, using’ async’ and ‘await’ keywords.

## Why we need a Thread

When the time comes when the application is required to perform few tasks at the same time.


## *How to create a Task*


```
static void Main(string[] args) {  
    Task < string > obTask = Task.Run(() => (  
        return“ Hello”));  
    Console.WriteLine(obTask.result);  
} 
```
## *How to create a Thread*


```
static void Main(string[] args) {  
    Thread thread = new Thread(new ThreadStart(getMyName));  
    thread.Start();  
}  
public void getMyName() {} 
```

## Differences Between Task And Thread
 
Here are some differences between a task and a thread.

1. The Thread class is used for creating and manipulating a thread in Windows. A Task represents some asynchronous operation and is part of the Task Parallel Library, a set of APIs for running tasks asynchronously and in parallel.
2. The task can return a result. There is no direct mechanism to return the result from a thread.
3. Task supports cancellation through the use of cancellation tokens. But Thread doesn't.
4. A task can have multiple processes happening at the same time. Threads can only have one task running at a time.
5. We can easily implement Asynchronous using ’async’ and ‘await’ keywords.
6. A new Thread()is not dealing with Thread pool thread, whereas Task does use thread pool thread.
7. A Task is a higher level concept than Thread.

*Differences Between Task And Thread* -->[https://www.linkedin.com/learning/threading-in-c-sharp/tasks-vs-threads]


## Program vs. Process vs. Thread vs. Task

These are four very similar terms yet very different. Lets start understanding the difference between them.

As everybody says its better to visualize than just blabber. So I have put all my thoughts about these 4 terms in the form of a diagram as shown below.

![blog4](https://user-images.githubusercontent.com/61011022/85732034-b48ac680-b703-11ea-892d-118695b7776d.png)

### *Now here comes the textual part:*

A **program** in Simple terms can be described as any executable file. Basically it contains certain set of instructions written with the intent of carrying out a specific operation. It resides in Memory & is a passive entity which doesn’t go away when system reboots.

Any running instance of a program is called as **process** or it can also be described as a program under execution. **1 program** can have **N processes**. Process resides in main memory & hence disappears whenever machine reboots. Multiple processes can be run in parallel on a multiprocessor system.

A **Thread** is commonly described as a lightweight process. **1 process** can have **N threads**. All threads which are associated with a common process share same memory as of process.The essential difference between a thread and a process is the work that each one is used to accomplish. Threads are being used for small & compact tasks, whereas processes are being used for more heavy tasks.

One major difference between a thread and a process is that threads within the same process consume the same address space, whereas different processes do not. This allows threads to read from and write to the common shared and data structures and variables, and also increases ease of communication between threads. Communication between two or more processes – also known as Inter-Process Communication i.e. IPC – is quite difficult and uses intensive resources.

**Tasks** are very much similar to threads, the difference is that they generally do not interact directly with OS. Like a Thread Pool, a task does not create its own OS thread. A task may or may not have more than one thread internally.

If you want to know when to use Task and when to use Thread: Task is simpler to use and more efficient that creating your own Threads. But sometimes, you need more control than what is offered by Task. In those cases, it makes more sense to use Thread directly.

The bottom line is that Task is almost always the best option; it provides a much more powerful API and avoids wasting OS threads.The only reasons to explicitly create your own Threads in modern code are setting per-thread options, or maintaining a persistent thread that needs to maintain its own identity.



## Kaynakça

+ https://www.c-sharpcorner.com/article/task-and-thread-in-c-sharp/
+ https://neharustagiblog.wordpress.com/2014/09/26/program-vs-process-vs-thread-vs-task/
+ https://stackoverflow.com/questions/13429129/task-vs-thread-differences
+ https://tallyfy.com/task-vs-project-vs-process-management/

 
