rExecutor Service: Defog Tech (https://www.youtube.com/watch?v=5wgZYyvIVJk )
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
