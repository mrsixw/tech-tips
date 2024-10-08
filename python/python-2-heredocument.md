# 2 - Python here-document

We saw before you can use `python3 -c` to run small scripts. What if you wanted something slightly 
longer with say a function? Well `here-document` is here to save the day. Consider

```bash
python3 <<EOF

def add(a, b):
    return a + b

print(f"The result is {add(1,2)}")
EOF
```

In this example `here-document` is the `<<EOF ... EOF` construct. When the shell sees `<<EOF` it knows it needs 
to wait until it sees `EOF` to have a complete "document". In our case here, the document is our simple Python adder upper script. 

When we run this, we see

```Bash
$ python3 <<EOF

def add(a, b):
    return a + b

print(f"The result is {add(1,2)}")
EOF
The result is 3
```

Awesome!