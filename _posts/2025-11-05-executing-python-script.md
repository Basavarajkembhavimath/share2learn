---
title: "Executing a Python script"
date: 2025-11-05
author: Sarfraaz Ahmed
category: Tutorial
---

Consider this simple Python script: india.py

```python
count = 5
name = "India"
print(name * count)
```

The expected output of this simple Python script "india.py" is

```bash
IndiaIndiaIndiaIndiaIndia
```

To execute this Python script on a Unix like shell ( on Mac OS, Ubuntu or other Linux like systems ) one can use this command

```bash
 $ python india.py
 IndiaIndiaIndiaIndiaIndia
```

Note: The above command assumes that "india.py" file is present in the current folder where you are running this command

On Linux ( and other Unix like systems ) there is another way to execute such scripts. We need to give execute permissions to this script and also somehow tell Bash shell to use "python" interpreter to execute this script. 

The second part is done using a "Shebang" statement that is written on the first line starting from the first column of the script as show below:

```python
#!/usr/bin/python  # <-- This line is called the "Shebang" line
count = 5
name = "India"
print(name * count)
```

