---
title: "Logging and Debugging in Python"
date: 2023-12-29T12:42:27+08:00
draft: false
---

## Rich: a Python library for rich text and beautiful formatting in the terminal

[Github](https://github.com/Textualize/rich), [Documentation](https://rich.readthedocs.io/en/latest/index.html#)

```bash
pip install rich
```

### Printing

- Console

    ```python
    from rich.console import Console
    console = Console()

    # ========================================
    # print auto formated objects
    # ========================================
    console.log("Hello from", console, "!") 

    # ========================================
    # print local variable of current function
    # ========================================
    def func():
        local_val1 = 42
        local_val2 = [4,2]
        console.log(func, log_locals=True)
    func()
    ```

- Inspect: Generate a report on an object:

    ```python
    from rich import inspect

    inspect(inspect) # this also prints the usage of inspect
    ```

### Logging

```python
# Other library may handle logging,
# remember to put this at top level.
import logging
from rich.logging import RichHandler

logging.basicConfig(
    level="NOTSET", # logging level
    format="%(message)s",
    datefmt="[%X]",
    handlers=[RichHandler()]
)
log = logging.getLogger("rich")

# ========================================
# print local variable of current function
# ========================================
log.info("Hello, World!")
```

```python
# logging level
NOTSET=0
DEBUG=10
INFO=20
WARN=30
ERROR=40
CRITICAL=50
```

### Progress Status

```python
from rich.console import Console
console = Console()

# ========================================
# Progress status
# ========================================
tasks = [f"task {n}" for n in range(1, 11)]
with console.status("[bold green]Working on tasks...") as status:
    while tasks:
        task = tasks.pop(0)
        console.log(f"{task} complete")
```

### Progress Bar

```python
from rich.progress import (
    BarColumn,
    MofNCompleteColumn,
    Progress,
    TextColumn,
    TimeElapsedColumn,
    TimeRemainingColumn,
)

# Define custom progress bar
def new_progress_bar():

    return Progress(
        TextColumn("{task.description}: [progress.percentage]{task.percentage:>3.0f}%"),
        BarColumn(),
        MofNCompleteColumn(),
        TextColumn("•"),
        TimeElapsedColumn(),
        TextColumn("• ETA"),
        TimeRemainingColumn(),
        transient=False, # sholdprogress bar disappear after finished
    )

# ========================================
# Progress bar
# ========================================
with new_progress_bar() as p:
    task_length = 1000
    task1 = p.add_task("[bold] Saving dataset1", total=task_length)
    task2 = p.add_task("[bold] Saving dataset2", total=task_length)
    p.start()
    p.console.print("Task started") # This will print message above the progress bars
    for i in p.track(range(0, task_length)):
        
        # Do something here
        p.update(task1, advance=1)
        p.update(task2, advance=1)
```

### Table

```python
from rich.console import Console
from rich.table import Table

table = Table(title="Star Wars Movies")

table.add_column("Released", justify="right", style="cyan", no_wrap=True)
table.add_column("Title", style="magenta")
table.add_column("Box Office", justify="right", style="green")

table.add_row("Dec 20, 2019", "Star Wars: The Rise of Skywalker", "$952,110,690")
table.add_row("May 25, 2018", "Solo: A Star Wars Story", "$393,151,347")
table.add_row("Dec 15, 2017", "Star Wars Ep. V111: The Last Jedi", "$1,332,539,889")
table.add_row("Dec 16, 2016", "Rogue One: A Star Wars Story", "$1,332,439,889")

console = Console()
console.print(table)
```