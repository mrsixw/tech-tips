# 1 - Python inlines
Python can run scripts passed from the command line. 

Consider

```bash
python3 -c 'print("hello world")'
```

Here the `-c` flag passes the Python interpreter a command to run. 
For this simple example, it's prints out obligatory "hello world", and indeed 
running this we see 

```bash
$ python3 -c 'print("hello world")'
hello world
```

Just simply lovely!
