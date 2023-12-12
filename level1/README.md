### Level 1


#### Basic Recon
![](./pics/1.png)

as you can see all relocation read-only is disabled, stack canary is disable, NX also, to me it seems maybe there's stack
overflow

![](./pics/2.png)

well there's call to gets, which is good, but even if there's gets is there, and we could jump to some place, a place? well run is intersting to me, we could check that

![](./pics/3.png)

here's another good sign there's call to system, which may give us a shell let's try it

let's check in gdb

![](./pics/4.png)
![](./pics/5.png)
![](./pics/6.png)
![](./pics/7.png)
![](./pics/8.png)
![](./pics/9.png)

well at the top of the stack there's location that points to
another location in the stack,
so when gets get called it will use the value on the top of
the stack as pointer and write to it what we input, and now
the availbe size is 0x50 - 0x10 + 0x8 = 0x44 = 72 why, well - 0x10 because
of the pointer that will be used by gets is esp+0x10, and why add 0x8 because that allocated when we did that and operation.

so what should we now, easy we just fill the stack with 68 bytes of junk, and fill the other 4 bytes, that was occupiad by the old ebp value and after that we put the address of the run function? why well by convition the ret instruction will just pop and what is stored there it will jump to it but we are far way pop would take the wrong value well no that's becaue
of the leave instruction that will clear the current stack
frame like this

![](./pics/10.png)

leave just copies the value of ebp to esp, and pop from there
to recover the old ebp

#### Payload
```
python -c print('B'*76 + '\x08\x04\x84\x44'[::-1])
```

![](./pics/11.png)