---
title: "Executing a Python script"
date: 2025-11-06
author: Sarfraaz Ahmed
category: Tutorial
---

Consider this simple Python script: apples.py

```python
count = 5
fruit = "\N{RED APPLE}"
print(fruit * count)
```

The expected output of this simple Python script "apples.py" is

```bash
🍎🍎🍎🍎🍎
```

To execute this Python script on a Unix like shell ( on Mac OS, Ubuntu or other Linux like systems ) one can use this command

```bash
 $ python apples.py
🍎🍎🍎🍎🍎
```

Note: The above command assumes that "apples.py" file is present in the current folder where you are running this command

On Linux ( and other Unix like systems ) there is another way to execute such scripts. We need to give execute permissions to this script and also somehow tell Bash shell to use "python" interpreter to execute this script. 

The second part is done using a "Shebang" statement that is written on the first line starting from the first column of the script as show below:

```python
#!/usr/bin/python3  # <-- This line is called the "Shebang" line
count = 5
fruit = "\N{RED APPLE}"
print(fruit * count)
```

Note: The word "Shebang" is a combination of "sharp" for "#" ( mostly coming from the Music notation ) and "bang" ( short for hash bang ), the name for ! ). But, there is no evident proof for this though, apart from Wikipedia.

The Shebang line contains the path of the interpreter to be used to execute this script. After adding the Shebang we need to give execute permissions to the script and directly execute it as shown below:

```bash
 $ chmod u+x apples.py
 $ ./apples.py 
 🍎🍎🍎🍎🍎
 $
```

# Virtual Environments

So, what if we need to execute this script using the python interpreter in the currently active virtual environment ?

Using the above Shebang would make the shell always use the Python interpreter mentioned in the Shebang, viz., /usr/bin/python3

The trick comes from the old Bash script days of using "env" ( which is sort of a hack that assumes env is mostly present at a specific location in all systems. And this is usually true is most of the modern Unix like systems )

So, the new modified Shebang now looks like

```python
#!/usr/bin/env python3  # <-- Ask env to pick python3 binary
count = 5
fruit = "\N{RED APPLE}"
print(fruit * count)
```

Here, the env command picks the python3 binary available in the existing Shell environment. This makes the script portable and can now be executed on any system or any virtual environment without worrying about the correct path for python3.
