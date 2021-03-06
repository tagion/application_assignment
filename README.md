## Simple distribute sort algorithm



Test assignment for a core team.

The task is to write a program in D which can sort a list of numbers using via tasks/threads intercommunication.

The program should be implemented as command line only program and it should only use the D standard library (phobos and druntime) and it should be able to be compiled with the **dmd** or the **ldc2** compiler.

The program should be able to be executed in the flowing manner.

```bash
./dsort input.json output.json -n 7
```

The program should take 3 arguments an input and output json file and a parameter n. 

The main task M should start the n sorting tasks (use the std.concurrency) and wait for the task to be ready (Using  concurrency send/read to synchronize the task).

Hint: Use an enum as a to signal (send/receive) between.

```D
enum Control {
    STOP, // Send to a task to stop it
    END   // Send from a task when the task ends
        ...
}
```

All tasks must be connected as show in the figure the arrows shows the message connections between the tasks. 

[Task commications figure](sort_tasks.svg)

Hint: The functions locate and register (in std.concurrency  [See](https://dlang.org/phobos/std_concurrency.html)) can be use to set the name of tasks and to locate the task id 'Tid'.

Hint: When a task is spawned from the main task M. The task function can be called with a immutable parameter.

When all n tasks has been started from the M task should send the numbers from the list one by one to each tasks randomly.

The sorting tasks are only allowed to communicate with previous task and the next tasks as show in with the arrows in the figure.

Hint: A sorting task could keep track of how many numbers the previous task and the next task holds.

When a tasks finished it should write a json file with numbers contained in the task the files should be called output_#.json where # is the task number.

The sorted numbers which is hold by the task should be send back to the main task M when the sorting task ends.

The main task should write all the collected number to the output.json file in order.

Hint: If the main task M send a STOP signal to each sorting task from 0 to n-1 and between each STOP it wait for the results from the sorting, this makes it simple for the main task to collect the result from all sorting tasks in order.

**Example**

```bash
./dsort input.json output.json -n 7
```

input.json

```json
[51, 99, 19, 80, 2, 4, 8, 0, 60, 23, 38, 29, 85, 58, 61, 60, 14, 77, 83, 4]
```
output.json
```json
[0, 2, 4, 4, 8, 14, 19, 23, 29, 38, 51, 58, 60, 60, 61, 77, 80, 83, 85, 99]
```
output_0.json
```json
[0, 2]
```
output_1.json
```json
[4, 4, 8, 14]
```
output_2.json
```json
[19, 23, 29]
```
output_3.json
```json
[23, 29, 38, 51, 58]
```
output_4.json
```json
[60, 60, 61, 77]
```
output_5.json
```json
[80, 83]
```
output_6.json
```json
[85, 99]
```

Recommend to use the D modules std.getopt, std.json, std.stdio, std.path, std.concurrency, std.random and other D standard libraries.

Note. 

A D program can compile/linked like.

```bash
dmd dsort.d
# or
ldc2 dsort.d
```

To search for information about D, use the keyword  **dlang**.

The dmd compiler can be found here [dmd][https://dlang.org/download.html]. and the **ldc2** can usually be install with. (**Ubuntu**)

```bash
sudo apt install ldc
```





