---
title: "Executing a Python script"
date: 2025-11-07
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

To execute this Python script on a Unix like shell ( on Mac OS, Ubuntu or
other Linux like systems ) one can use this command:

```bash
 $ python3 apples.py
   🍎🍎🍎🍎🍎
```

**Note:** The above command assumes that "apples.py" file is present in the
current folder where you are running this command. Otherwise, we would 
have to specify either its absolute or relative path instead.

## Directly executing Python script as a command

On Linux ( and other Unix like systems ) there is another way to execute 
such scripts. We need to give execute permissions to this script and also 
somehow tell Bash shell to use "python" interpreter to execute this script.

The second part is done using a "Shebang" statement that is written on the
first line of the script starting from the first column as show below:

```python
#!/usr/bin/python3  # <-- This line is called the "Shebang" line
count = 5
fruit = "\N{RED APPLE}"
print(fruit * count)
```

**Note:** The word "Shebang" is a combination of "sharp" for "#" ( mostly 
coming from the Music notation ) and "bang" ( short for hash bang, the 
name for !). But, there is no evident proof for this though, apart from 
Wikipedia.

The Shebang line contains the path of the interpreter to be used to execute
this script. After adding the Shebang we need to give execute permissions 
to the script and directly execute it as shown below:

```bash
 $ chmod u+x apples.py  # <--- Giving execute permissions to the script
 $ ./apples.py          # <--- Executing the script using it's relative path
   🍎🍎🍎🍎🍎
 $
```

## Virtual Environments

What if I share this script with a friend who has python3 present in some
other location ? May be in /usr/share/bin/python3 ?

Or, what if we need to execute this script using the python interpreter 
in the currently active virtual environment ?

Using the above Shebang would make the shell always use the Python 
interpreter mentioned in the Shebang, viz., /usr/bin/python3

The trick comes from the old Bash script days of using "env" command to
pick the correct command from the present environment variables / settings
( which is sort of a hack that assumes env is mostly present at a specific 
location in all systems. But, this is fine since "env" command is usually
present in the same location on most of the modern Unix like systems )

So, the new modified Shebang now looks like

```python
#!/usr/bin/env python3  # <-- Ask env to pick appropriate python3 binary
count = 5
fruit = "\N{RED APPLE}"
print(fruit * count)
```

Here, the env command picks the python3 binary available in the existing 
Shell environment. This makes the script portable and can now be executed 
on any system or any virtual environment without worrying about the 
correct path for python3.

# What about using this with the modern "uv" tool ?

The problem with running a Python script on any (client or customer) machine
is that they need to have Python installed. And that too the correct version
that the script expects ( based on the features that are used in it ). Apart
from this, there is also the problem of dependencies. The system where we 
are planning to execute our Python script needs to have all the dependent 
modules installed. And they also need to be of the correct versions.

"uv" tries to solve this by adding some inline metadata to the script that
mentions the dependencies for the script. This is in accordance with PEP 723
where this support in Python scripts was introduced.

Let's assume that "apples.py" script depends on "numpy" module. To 
mention this dependency in the inline metadata, we need to execute the 
following "uv" command:

```bash
 $ uv add --script apples.py 'numpy'
```

This modifies the "apples.py" script with the inline metadata added at the
start of the script as shown below :

```python
# /// script                    # <--- inline metadata starts here
# requires-python = ">=3.12"
# dependencies = [
#     "numpy",
# ]
# ///                           # <--- inline metadata ends here

count = 5
fruit = "\N{RED APPLE}"
print(fruit * count)
```

And we can now use this "uv" command to execute "apples.py"

```bash
 $ uv run apples.py
   Installed 1 package in 18ms
   🍎🍎🍎🍎🍎
```

The above command takes care of installing the correct version of Python and
the dependent modules in a temporary location on the customer's sytem and 
then execute the Python commands in that script

If we do not want to display the message about installing packages, then 
we can use the "--quiet" switch as shown below:

```bash
 $ uv run --quiet apples.py
   🍎🍎🍎🍎🍎
```

## Combining "uv" with "Shebang"

Running the "uv" command as above lands us into the same problem of asking
customers to run a command ( instead of executing just the script ) to 
trigger the execution of our script. We could solve it using the Shebang 
trick we saw earlier as mentioned [in this
post](https://akrabat.com/using-uv-as-your-shebang-line/)

We can now use the following Shebang to directly launch this script 
using "uv" command:

```python
#!/usr/bin/env -S uv run --script   # <--- Ask env to pick "uv" binary
# /// script                        # <--- inline metadata starts here
# requires-python = ">=3.12"
# dependencies = [
#     "numpy",
# ]
# ///                               # <--- inline meatadata ends here

count = 5
fruit = "\N{RED APPLE}"
print(fruit * count)
```

The "env" command in the Shebang is almost the same as we saw earlier, 
except for the "-S" switch. This switch as mentioned in the [man
pages](https://man7.org/linux/man-pages/man1/env.1.html#SCRIPT_OPTION_HANDLING)
allows us to pass multiple arguments to the Shebang command. In this case,
using the "-S" switch now allows us to properly execute "uv" command with 
"run" and "--script" arguments passed on to the "uv" command as switches 
to it.

**Note:** This "-S" switch might behave differently on a a Mac system which 
uses BSD Unix. The Shebang works on a Mac OS irrespective of providing the
"-S"switch.

And we would execute the "apples.py" script as :

```bash
 $ ./apples.py
   Installed 1 package in 18ms
   🍎🍎🍎🍎🍎
```

And as we saw earlier, if we do not want to display the message about
installing packages, then we could use the "--quiet" switch as shown below:


```python
#!/usr/bin/env -S uv run --script --quiet   # Masks the output of "uv"
# /// script                            # <--- inline metadata starts here
# requires-python = ">=3.12"
# dependencies = [
#     "numpy",
# ]
# ///                                   # <--- inline meatadata ends here

count = 5
fruit = "\N{RED APPLE}"
print(fruit * count)
```

And when we execute this script, we would get the output as:

```bash
 $ ./apples.py
   🍎🍎🍎🍎🍎
```

## Conclusion

This way, we can have a self dependent Python script execute on its own,
irrespetive of whether the customer has the correct Python version 
installed or not.

But, this works only on Unix like machines. I am not sure how something like
this can be done on non-Unix like machines.

Ofcourse, the problem now moves from the customer having "python" 
installed on their systems to them having "uv" installed on their system 
for executing our Python script. But, with the growing popularity of "uv" 
it can be safe to assume that most customers would have "uv" pre-installed 
on their system, as we earlier assumed that they would have "bash" or "env" 
pre-installed on their systems in the right locations.
