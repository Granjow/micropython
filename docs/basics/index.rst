MicroPython Basics
==================

What is the difference between MicroPython and Python?
---

Python is a programming language. This is what you write in `.py` files.
A `.py` file on its own is just a text file until it is executed by a Python interpreter. This is a program which can translate Python to machine code and then, for example, write files or send data over the network.

**CPython** is one such interpreter, it is also what you get when you download Python from python.org. It runs on most Linux, Windows, and Mac OS platforms, but not on microcontrollers.

**MicroPython** is another Python interpreter, which is designed specially for microcontrollers. This means e.g.:

* MicroPython is small enough to fit on a Microcontroller
* MicroPython supports various Microcontroller instruction sets


Can I run any Python program on the Microcontroller?
---

Maybe.

Most Python libraries run on MicroPython. The same holds for your Python program.

It will not run if …

* … the program uses unsupported language features. MicroPython does not support all Python language features. If you use a language feature that is not supported, it will not run.
  See https://docs.micropython.org/en/latest/genrst/index.html
* … the program accesses host specific features like a Linux service or the Windows registry.
* … the program uses a lot of resources (e.g. gigabytes of RAM) which the microcontroller simply does not have.

But I have to run it!
---

There are options.

If you use unsupported language features, avoid or mock them. For example, MicroPython does not support `ABC`, but it can be mocked with a small class
so the same code runs on both CPython (with type checking) and MicroPython (without type checking).

```python
class ABC:
    def __init__(self):
        pass
```

Then make sure that MicroPython also finds the mock by modifying the path list where MicroPython looks for the .py files.

```python
import sys

sys.path.append('.')
```

If you use host specific features or too much resources, the program should probably remain on the host.

So, can I run MicroPython programs on my PC?
---

Again, maybe.

It will not run if …

* … the program uses platform specific features like a microcontroller’s IO or SPI bus.

But I want to test my Microcontroller code on my PC!
---

Fear not! Patterns like dependency inversion are here for the rescue.

Assume code which reads a value from a UART device to decide whether to turn a lamp on or off.

```python
class AlarmClock:
  def __init__(self):
    uart = UART(1)
    io = machine.Pin(10, machine.Pin.OUTPUT)
if uart.read() == "sleeping":
  io.write(True)
```


* Performance
* mpy vs cpy
* basics repl, flash storage, main.py
* mocking (host + µc)
* how to build
