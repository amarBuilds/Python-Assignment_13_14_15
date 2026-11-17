# Assignment 15: Python Automatic Garbage Collection

## Aim

To study Python's automatic garbage collection mechanism and experiment
with manual garbage collection using the `gc` module.

## Introduction

Garbage collection is the process of identifying and freeing memory
occupied by objects that are no longer needed.

Python primarily manages objects using **reference counting**. Each
object keeps track of how many references point to it. When its
reference count reaches zero, the object can generally be released.

Reference counting alone cannot remove certain groups of objects that
refer to one another, even when the group is no longer reachable from
the program. These are called **reference cycles**. Python's cyclic
garbage collector helps detect and collect such unreachable cycles.

The built-in `gc` module provides functions to inspect and control the
cyclic garbage collector.

## Python Program

``` python
import gc

class Demo:
    def __init__(self, name):
        self.name = name
        self.reference = None

    def __del__(self):
        print(f"Object {self.name} is being destroyed.")


print("1. Checking garbage collector status:")
print("Garbage collection enabled:", gc.isenabled())

print("\\n2. Creating a reference cycle:")
object_a = Demo("A")
object_b = Demo("B")

# Create a cycle: A refers to B and B refers to A
object_a.reference = object_b
object_b.reference = object_a

# Remove the program's direct references to the objects
del object_a
del object_b

print("\\n3. Running garbage collection manually:")
collected = gc.collect()
print("Objects collected:", collected)

print("\\n4. Garbage collector statistics:")
print("Garbage collector counts:", gc.get_count())
print("Garbage collector thresholds:", gc.get_threshold())

print("\\n5. Disabling and enabling automatic collection:")
gc.disable()
print("Automatic garbage collection enabled:", gc.isenabled())

gc.enable()
print("Automatic garbage collection enabled:", gc.isenabled())

print("\\nExperiment completed.")
```

## Sample Output

``` text
1. Checking garbage collector status:
Garbage collection enabled: True

2. Creating a reference cycle:

3. Running garbage collection manually:
Object A is being destroyed.
Object B is being destroyed.
Objects collected: 2

4. Garbage collector statistics:
Garbage collector counts: (0, 0, 0)
Garbage collector thresholds: (700, 10, 10)

5. Disabling and enabling automatic collection:
Automatic garbage collection enabled: False
Automatic garbage collection enabled: True

Experiment completed.
```

*Note: Exact collection counts, thresholds, and the order of destruction
messages can vary with Python version and runtime state. The output
above is illustrative.*

## Explanation

-   `import gc`: imports Python's garbage collection module.
-   `gc.isenabled()`: checks whether automatic cyclic garbage collection
    is enabled.
-   `Demo`: defines a class whose objects can refer to one another.
-   `object_a.reference = object_b` and `object_b.reference = object_a`:
    create a reference cycle.
-   `del object_a` and `del object_b`: remove the direct references,
    leaving the cycle unreachable from the program.
-   `gc.collect()`: requests a collection and returns the number of
    unreachable objects collected.
-   `gc.get_count()`: returns the current collection counters for the
    generations.
-   `gc.get_threshold()`: returns the thresholds that influence when
    automatic collection is triggered.
-   `gc.disable()` and `gc.enable()`: disable and re-enable automatic
    cyclic garbage collection.

## Research: How Automatic Garbage Collection Works

Python's memory management combines reference counting with a cyclic
garbage collector.

1.  **Object creation:** When an object is created, Python allocates
    memory for it.
2.  **Reference counting:** References to the object are tracked. When
    references are removed, its reference count decreases.
3.  **Immediate cleanup:** When the reference count reaches zero, the
    object can usually be deallocated immediately.
4.  **Cycle detection:** Objects in a reference cycle may keep one
    another's reference counts above zero. The cyclic garbage collector
    searches for unreachable cycles.
5.  **Memory reuse:** Once objects are collected, their memory can be
    reused by Python. This does not necessarily mean that the process
    immediately returns the memory to the operating system.

Python's cyclic collector uses generations to organise objects by age.
Objects that survive collections may be checked less frequently. The
exact implementation and thresholds can vary between Python versions.

## Important `gc` Module Functions

  Function               Description
  ---------------------- ------------------------------------------------
  `gc.collect()`         Runs a collection manually
  `gc.isenabled()`       Checks whether automatic collection is enabled
  `gc.enable()`          Enables automatic cyclic garbage collection
  `gc.disable()`         Disables automatic cyclic garbage collection
  `gc.get_count()`       Returns current collection counters
  `gc.get_threshold()`   Returns collection thresholds
  `gc.set_threshold()`   Changes collection thresholds

## Result

The experiment demonstrates Python's automatic garbage collection
status, creates an unreachable reference cycle, and uses `gc.collect()`
to request manual collection. It also demonstrates how to inspect,
disable, and re-enable automatic garbage collection.

## How to Run

Save the code as `assignment15.py` and run:

``` bash
python3 assignment15.py
```

## Conclusion

Python automatically manages memory through reference counting and
cyclic garbage collection. The `gc` module allows programmers to inspect
and control the cyclic garbage collector when required. In normal
programs, automatic garbage collection should generally remain enabled
unless there is a specific reason to change it.
