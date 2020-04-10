A coroutine switching implement by assembly && ucontext in C-language
----

The coroutine switching implement in C-language called `ccoroutine` this time. it is powerful and lightweight.

The `co_yield()`, `co_yield_from()`, `co_send()`, `co_loop()` in `ccoroutine` are just like the `yield`, `yield from`, `generator.send()`, `asyncio.loop()` in python respectively. 

I have had wrote two blogs to flatter `ccoroutine`, but it‘s core routines always less than 500 lines, this is the reason i still love it.

The potential bottleneck of `ccoroutine` besets me at the same time, so goddess hopes more knowledgeable guys just like you can **continue to improve it**.

### catalogs
`src`, major logic for `ccoroutine`.the `SConscript` in `src/` would build out ccoroutine library. use `lncc = SConscript('../../src/SConscript')` to get the library path by `SConstruct` in experiences/applications.

`context`, coroutine switching supporter, including assembly and ucontext.

`experiences`, namely examples for `ccoroutine`, `co_yield`, `co_send`, `co_yield_from`, `co_loop` included.

`doc`, chinese documents or development-diary for `ccoroutine`.

### running experience
if you owns one running 20000000 simple coroutines such as `_co_yield_from_fn` && `_co_fn` in `loop_e` on a server-computer to experience `ccoroutine`.
10000000 coroutines(_co_fn) switching, other 10000000 coroutines(_co_yield_from_fn) used to sync the former 10000000 coroutines to terminate respectively.

running `scons -Q` to build the target program.
```C
[a@b loop_e]$ scons -Q
...
[a@b loop_e]$
[a@b loop_e]$ ./loop_e 2>o.txt
[a@b loop_e]$ vi o.txt
       1 '_co_yield_from_fn' sync '_co_fn' terminated. '_co_fn' return-value: 012
...
 9999999 '_co_yield_from_fn' sync '_co_fn' terminated. '_co_fn' return-value: 012
10000000 '_co_yield_from_fn' sync '_co_fn' terminated. '_co_fn' return-value: 012
```

start 'top' on another terminal to see the little memory consumption.
```C
top - 16:41:24 up 3 days,  8:02,  3 users,  load average: 0.49, 0.24, 0.15
Tasks:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
%Cpu(s):  3.6 us,  4.5 sy,  0.0 ni, 91.5 id,  0.1 wa,  0.0 hi,  0.3 si,  0.0 st

  PID USER    PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
18036  lxr    20   0    4348    352    276 R  28.2  0.0   0:12.37  loop_e
```
the memory consumption will increase more when the corotines' number and running time growth, of course.
