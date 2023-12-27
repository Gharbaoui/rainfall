### Level 5

#### Basic Recon

![](./pics/1.png)
![](./pics/2.png)
![](./pics/3.png)


![](./pics/4.png)

it seems bad there's no call to `o` and we are using `fgets` so no overflow is possible, and even if possible we can not do it because we are exiting before we reach `ret` instruction,

[](../explained/plt_got.md)

well now you can see that we can do some tricks maybe if we filled that table with fake addresses like maybe the address of exit start to point to system directly!