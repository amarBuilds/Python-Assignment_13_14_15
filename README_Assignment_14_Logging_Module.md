# Assignment 14: Uses of Python's Logging Module

## Aim

To write a Python program that demonstrates the different levels and
uses of the `logging` module.

## Introduction

The `logging` module is used to record messages about the execution of a
program. These messages help developers understand program behaviour,
identify errors, and debug applications.

Python provides five commonly used logging levels:

  Level        Purpose
  ------------ ---------------------------------------------------
  `DEBUG`      Detailed information useful during debugging
  `INFO`       Confirms that normal operations are working
  `WARNING`    Indicates a possible problem
  `ERROR`      Reports an error that affected an operation
  `CRITICAL`   Reports a serious error that may stop the program

## Python Program

``` python
import logging

# Configure logging to display messages and save them to a file
logging.basicConfig(
    level=logging.DEBUG,
    format="%(asctime)s - %(levelname)s - %(message)s",
    handlers=[
        logging.FileHandler("application.log", mode="w"),
        logging.StreamHandler()
    ],
    force=True
)

def divide_numbers(a, b):
    logging.debug("divide_numbers() was called.")

    if b == 0:
        logging.error("Division by zero is not allowed.")
        return None

    result = a / b
    logging.info("Division completed successfully.")
    return result

logging.debug("Program execution started.")
logging.info("Application is running.")

# Demonstrate normal operation
answer = divide_numbers(10, 2)
print("Result:", answer)

# Demonstrate a warning
temperature = 38
if temperature > 35:
    logging.warning("Temperature is higher than the expected limit.")

# Demonstrate an error
divide_numbers(10, 0)

# Demonstrate a critical message
logging.critical("Demonstration of a critical message.")

logging.info("Program execution finished.")
```

## Sample Output

The timestamp will depend on the date and time when the program is
executed.

``` text
DEBUG - Program execution started.
INFO - Application is running.
DEBUG - divide_numbers() was called.
INFO - Division completed successfully.
Result: 5.0
WARNING - Temperature is higher than the expected limit.
DEBUG - divide_numbers() was called.
ERROR - Division by zero is not allowed.
CRITICAL - Demonstration of a critical message.
INFO - Program execution finished.
```

The actual output also includes timestamps because of the configured
format. Messages are displayed in the terminal and written to
`application.log`.

## Explanation

-   `import logging`: imports the logging module.
-   `logging.basicConfig()`: configures the logging system.
-   `level=logging.DEBUG`: allows all five standard levels to be
    recorded.
-   `format`: specifies how each log message is displayed.
-   `FileHandler`: writes log messages to `application.log`.
-   `StreamHandler`: displays log messages in the terminal.
-   `logging.debug()`, `logging.info()`, `logging.warning()`,
    `logging.error()` and `logging.critical()`: record messages at
    different severity levels.
-   The `divide_numbers()` function demonstrates successful execution
    and error handling.

## Uses of the Logging Module

1.  **Debugging:** helps locate problems in a program.
2.  **Error tracking:** records errors and exceptions.
3.  **Monitoring:** keeps track of important program activities.
4.  **File records:** stores execution details for later inspection.
5.  **Maintenance:** helps developers understand what happened during a
    previous execution.

## Result

The program successfully demonstrates all five standard logging levels.
It displays log messages in the terminal and stores them in a log file
named `application.log`.

## How to Run

Save the code as `assignment14.py` and run:

``` bash
python3 assignment14.py
```

After execution, check the generated `application.log` file in the same
directory.
