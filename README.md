# msys2上的中文乱码问题
## git中文目录名
msys2的git会将大于0x80的字符（例如中文）转义为类似 `、346\265\213\350\257\225` 的形式。
启用 `core.quotePath` 可解决这个问题：
```
git config --global core.quotePath=false
```
## 运行Windows控制台应用程序
在中文Windows系统上的msys2，*mintty*作为终端模拟器时，直接运行诸如`ping`的命令，会有乱码输出。此时可转用*winpty*运行此类命令。
安装msys2中提供的winpty：
```
pacman -S winpty
```
可以为常用的命令起个别名：
```
alias pingw='winpty -- ping'
```
mintty下的ping输出：
```
# ping cloudflare.com

▒▒▒▒ Ping cloudflare.com [104.17.175.85] ▒▒▒▒ 32 ▒ֽڵ▒▒▒▒▒:
▒▒▒▒ 104.17.175.85 ▒Ļظ▒: ▒ֽ▒=32 ʱ▒▒=149ms TTL=54
▒▒▒▒ 104.17.175.85 ▒Ļظ▒: ▒ֽ▒=32 ʱ▒▒=145ms TTL=54
▒▒▒▒ 104.17.175.85 ▒Ļظ▒: ▒ֽ▒=32 ʱ▒▒=150ms TTL=54
▒▒▒▒ 104.17.175.85 ▒Ļظ▒: ▒ֽ▒=32 ʱ▒▒=150ms TTL=54

104.17.175.85 ▒▒ Ping ͳ▒▒▒▒Ϣ:
    ▒▒▒ݰ▒: ▒ѷ▒▒▒ = 4▒▒▒ѽ▒▒▒ = 4▒▒▒▒ʧ = 0 (0% ▒▒ʧ)▒▒
▒▒▒▒▒г̵Ĺ▒▒▒ʱ▒▒(▒Ժ▒▒▒Ϊ▒▒λ):
    ▒▒▒ = 145ms▒▒▒ = 150ms▒▒ƽ▒▒ = 148ms
```
winpty下的ping输出：
```
# winpty -- ping cloudflare.com

正在 Ping cloudflare.com [104.17.175.85] 具有 32 字节的数据:
来自 104.17.175.85 的回复: 字节=32 时间=148ms TTL=54
来自 104.17.175.85 的回复: 字节=32 时间=148ms TTL=54
来自 104.17.175.85 的回复: 字节=32 时间=150ms TTL=54
来自 104.17.175.85 的回复: 字节=32 时间=150ms TTL=54

104.17.175.85 的 Ping 统计信息:
    数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，
往返行程的估计时间(以毫秒为单位):
    最短 = 148ms，最长 = 150ms，平均 = 149ms
```


另一种办法是[转换文本编码](https://hustlei.github.io/2018/11/msys2-for-win.html#msys2%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%81%E9%97%AE%E9%A2%98)。

使用winpty的好处在于，一些需要交互和输出的应用，会输出消息到Windows控制台，而不是mintty上。和msys2一同使用GnuPGv2时（内置源提供的GnuPG已经很旧了，需要手动安装，[参考](https://hustlei.github.io/2018/11/msys2-for-win.html)），[一些操作需要使用winpty才能正常继续](https://github.com/carlolars/gnupg2-msys2/blob/master/README.md#gpg-and-mintty-needs-winpty)，否则会卡住。