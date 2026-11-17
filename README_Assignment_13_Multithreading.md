# Assignment 13: Python Multithreading

## Aim

To write a Python program that creates multiple threads and performs
different tasks concurrently.

## Introduction

A **thread** is a smaller unit of execution within a process. Python's
`threading` module allows a program to run multiple threads. This is
useful when tasks involve waiting, such as reading files, downloading
data, or communicating over a network.

This program creates three threads. Each thread performs a different
task: 1. Prints a counting sequence. 2. Displays the squares of numbers.
3. Displays the cubes of numbers.

The `join()` method makes the main program wait until all three threads
finish.

## Python Program

``` python
import threading
import time

def count_numbers():
    for number in range(1, 6):
        print(f"[Counting Thread] Number: {number}")
        time.sleep(0.5)

def calculate_squares():
    for number in range(1, 6):
        print(f"[Square Thread] {number} squared = {number ** 2}")
        time.sleep(0.5)

def calculate_cubes():
    for number in range(1, 6):
        print(f"[Cube Thread] {number} cubed = {number ** 3}")
        time.sleep(0.5)

# Create thread objects
thread1 = threading.Thread(target=count_numbers, name="CountingThread")
thread2 = threading.Thread(target=calculate_squares, name="SquareThread")
thread3 = threading.Thread(target=calculate_cubes, name="CubeThread")

# Start all threads
thread1.start()
thread2.start()
thread3.start()

# Wait for all threads to complete
thread1.join()
thread2.join()
thread3.join()

print("\\nAll tasks have been completed.")
```

## Sample Output

The exact order of lines may differ each time because the threads are
scheduled independently.

``` text
[Counting Thread] Number: 1
[Square Thread] 1 squared = 1
[Cube Thread] 1 cubed = 1
[Counting Thread] Number: 2
[Square Thread] 2 squared = 4
[Cube Thread] 2 cubed = 8
[Counting Thread] Number: 3
[Square Thread] 3 squared = 9
[Cube Thread] 3 cubed = 27
[Counting Thread] Number: 4
[Square Thread] 4 squared = 16
[Cube Thread] 4 cubed = 64
[Counting Thread] Number: 5
[Square Thread] 5 squared = 25
[Cube Thread] 5 cubed = 125

All tasks have been completed.
```

*Note: This is illustrative output. The interleaving can vary between
executions.*

## Explanation

-   `import threading`: imports Python's threading module.
-   `import time`: provides the `sleep()` function.
-   `threading.Thread(target=...)`: creates a thread and specifies the
    function it will execute.
-   `start()`: starts the thread.
-   `time.sleep(0.5)`: pauses that thread briefly, allowing other
    threads to run.
-   `join()`: waits for a thread to finish before the main program
    continues.

## Result

The program successfully creates three threads and executes different
tasks concurrently. The output demonstrates that the tasks can be
interleaved, and the main program waits for all threads to complete.

## How to Run

Save the code as `assignment13.py` and run:

``` bash
python3 assignment13.py
```
