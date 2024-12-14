Executor Service: Defog Tech (https://www.youtube.com/watch?v=5wgZYyvIVJk )
=================
It is about having theadpool having setof tasks stored in blocking queue. These tasks pick from blcoking queue 
and performs a task. We can use callab;e where we will get the return value. we can use runnable also.

ForkJoin pool:
===============
Big Task break into multiple subtasks , each one is computed individually parally , then 
and resultes are aggregated to make final result.
 1. Forking and 2. Joining 2 operations

It has per thread queueing & work stealing:
-----------------------------------------
 
![image](https://github.com/user-attachments/assets/f3b46378-cbb2-490f-9a6f-5837b7aafa8c)
Here each thread has Deque, in short it is called double ended queue.
each subtasks stored in its local of its therad,
you are touching u r own queue, no synchronization problem.

In this scenatio 1 thread 1 is busy, other thread 0 already executed, so inthis caethread 0 will pick the tasks from other end, this is called work staling

<img width="344" alt="image" src="https://github.com/user-attachments/assets/d0d5deab-87d5-45e8-bc32-fa47b38578ae" />

How to submit this tasks:
<img width="440" alt="image" src="https://github.com/user-attachments/assets/1dfc5767-8c02-4fcf-8792-85ff5e3b866c" />


## ForkJoinTask Class Built internally
  
fork(), JOin()


