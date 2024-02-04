### Level 6

#### basic recon

![](./pics/1.png)
![](./pics/2.png)

well there's call to `m` and to `n` let's see what's going on there

![](./pics/3.png)

as you can see `n` has call to `system` and `m` has call to `puts`

#### Step by Step
![](./pics/1.png)
![](./pics/2.png)
![](./pics/3.png)
![](./pics/4.png)
![](./pics/5.png)
![](./pics/6.png)
![](./pics/7.png)
![](./pics/8.png)
![](./pics/9.png)
![](./pics/10.png)
![](./pics/11.png)
![](./pics/12.png)

but what's the meaning of the value `0x8048468` mean let's take look there

![](./pics/13.png)

oh well that's the location of the function `m` okay let's continue

![](./pics/14.png)
![](./pics/15.png)
![](./pics/16.png)
![](./pics/17.png)

what is the meaning of the value stored in eax now? it's for some address of the previous stack frame so?
since we are trying to reach something in the previous stack frame maybe the arguments of the programm
let's check

![](./pics/18.png)
![](./pics/19.png)
![](./pics/20.png)
![](./pics/21.png)
![](./pics/22.png)
![](./pics/21.png)
![](./pics/24.png)

here's the interface of `strcpy`

```c
char *strcpy(char *dest, const char *src);
```
![](./pics/25.png)

so what we did now is copy the first param to the location that was returned by the first malloc
and as you can see after that there's call using the value stored at [esp+0x18], which is pointing
to function `m`, let's recap

malloc_1: 0x0804a008
malloc_2: 0x0804a050

and `strcpy` will copy into malloc_1, and call will use value stored in malloc_2, and jmp to it
and malloc_1 < malloc_2, and `strcpy` has no boundary checks, I could input a long input, to fill the
gap between them and then put the location of the function `n` after that but what is the value of this
gap?
gap = malloc_2 - malloc_1 = 72

maybe something like this
A*72 + '0x08048454', 0x08048454: address of `n`

![](./pics/26.png)

#### Payload

```python
python -c "print ('A'*72 + '\x54\x84\x04\x08')" | xargs -I PPPP ./level6 PPPP
```

#### Password for level7
```
f73dcb7a06f60e3ccc608990b0a046359d42a1a0489ffeefd0d9cb2d7c9cb82d
```