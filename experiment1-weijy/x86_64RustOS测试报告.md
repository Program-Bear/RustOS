#x86_64RustOS测试报告
复现了王润基的相关工作，在qemu模拟器上测试原ucore上的相关测试样例，获得了如下测试结果：

##针对waitkill的测试（通过测试）：
wait child 1.
child 2.
child 1.
kill parent ok.
kill child1 ok.

##针对sleep的测试（通过测试）：
sleep 1 x 100 slices.
sleep 2 x 100 slices.
sleep 3 x 100 slices.
sleep 4 x 100 slices.
sleep 5 x 100 slices.
sleep 6 x 100 slices.
sleep 7 x 100 slices.
sleep 8 x 100 slices.
sleep 9 x 100 slices.
sleep 10 x 100 slices.
use 1001 msecs.
sleep pass.

##针对spin的测试（通过测试）：
I am the parent. Forking the child...
I am the parent. Running the child...
I am the child. spinning ...
I am the parent.  Killing the child...
kill returns 0
wait returns 0
spin may pass.

##针对sh的测试（未通过测试）：
user sh is running!!!$ [ERROR] unknown syscall id: 0x66, args: [0, b00fff47, 1, b00fff28, 80013e, 0]
[ERROR] Process 2 error:
TrapFrame {
    r15: 0x0,
    r14: 0x0,
    r13: 0x0,
    r12: 0x0,
    rbp: 0xb00ffef0,
    rbx: 0x1,
    r11: 0x0,
    r10: 0x0,
    r9: 0x0,
    r8: 0x0,
    rsi: 0x80013e,
    rdi: 0xb00fff28,
    rdx: 0x0,
    rcx: 0xb00fff47,
    rax: 0x66,
    trap_num: 0x80,
    error_code: 0x0,
    rip: 0x8004ff,
    cs: 0x23,
    rflags: 0x202,
    rsp: 0xb00ffec4,
    ss: 0x2b
}

##针对forktest的测试（通过测试）：
I am child 4
I am child 3
I am child 2
I am child 1
I am child 0
I am child 9
I am child 8
I am child 7
I am child 6
I am child 5
I am child 13
I am child 12
I am child 11
I am child 10
I am child 18
I am child 17
I am child 16
I am child 15
I am child 14
I am child 23
I am child 22
I am child 21
I am child 20
I am child 19
I am child 28
I am child 27
I am child 26
I am child 25
I am child 24
I am child 31
I am child 30
I am child 29
forktest pass.

##针对faultread的测试（未通过测试）：
[ERROR] 
EXCEPTION: Page Fault @ 0x0, code: 0x4
[ERROR] 
EXCEPTION: Page Fault @ 0xffffff8000000000, code: 0x0
[ERROR] Process 2 error:
TrapFrame {
    r15: 0xffffff0000850268,
    r14: 0x0,
    r13: 0xffffff0000042880,
    r12: 0xffffff00001b7e50,
    rbp: 0xb00fff88,
    rbx: 0xffffff8000000000,
    r11: 0x0,
    r10: 0xffffff00001b7a38,
    r9: 0x1,
    r8: 0x4,
    rsi: 0x0,
    rdi: 0xffffff8000000000,
    rdx: 0xffffff0000850268,
    rcx: 0x0,
    rax: 0xffffff8000000000,
    trap_num: 0xe,
    error_code: 0x0,
    rip: 0xffffff0000027c80,
    cs: 0x8,
    rflags: 0x86,
    rsp: 0xffffff00001b6db8,
    ss: 0x0
}

##针对forktree的测试（未通过测试）：
forktree process will sleep 400 ticks
0002: I am ''
>> 0004: I am '1'
0005: I am '11'
0007: I am '111'
0009: I am '1111'
0008: I am '1110'
0006: I am '110'
000b: I am '1101'
000a: I am '1100'
0003: I am '0'
000d: I am '01'
000f: I am '011'
0011: I am '0111'
0010: I am '0110'
000e: I am '010'
0013: I am '0101'
0012: I am '0100'
000c: I am '00'
0015: I am '001'
0017: I am '0011'
0016: I am '0010'
0014: I am '000'
0019: I am '0001'
0018: I am '0000'
0002: I am '10'
001a: I am '100'
001c: I am '1001'
001b: I am '1000'
001d: I am '101'
001f: I am '1011'
001e: I am '1010'
[ERROR] 
PANIC in /Users/victor/RustOS/RustOS/crate/process/src/scheduler.rs at line 136
    attempt to add with overflow

##针对divzero的测试（未通过测试）：
[ERROR] Process 2 error:
TrapFrame {
    r15: 0x0,
    r14: 0x0,
    r13: 0x0,
    r12: 0x0,
    rbp: 0xb00fff88,
    rbx: 0x0,
    r11: 0x0,
    r10: 0x0,
    r9: 0x0,
    r8: 0x0,
    rsi: 0x0,
    rdi: 0x0,
    rdx: 0x0,
    rcx: 0x0,
    rax: 0x1,
    trap_num: 0x0,
    error_code: 0x0,
    rip: 0x8015b9,
    cs: 0x23,
    rflags: 0x282,
    rsp: 0xb00fff80,
    ss: 0x2b
}

##针对yield的测试（通过测试）：
Hello, I am process 2.
Back in process 2, iteration 0.
Back in process 2, iteration 1.
Back in process 2, iteration 2.
Back in process 2, iteration 3.
Back in process 2, iteration 4.
All done in process 2.
yield pass.

##针对faultreadkeral的测试（未通过测试）：
[ERROR] 
EXCEPTION: Page Fault @ 0xfac00000, code: 0x4
[ERROR] 
EXCEPTION: Page Fault @ 0xffffff80007d6000, code: 0x0
[ERROR] 
EXCEPTION: Page Fault @ 0xffffffffc0003eb0, code: 0x0
[ERROR] Process 2 error:
TrapFrame {
    r15: 0xffffff0000850268,
    r14: 0xffffff80007d6000,
    r13: 0xffffff0000042880,
    r12: 0xffffff00000c6c00,
    rbp: 0xb00fff88,
    rbx: 0xffffffffc0003eb0,
    r11: 0x0,
    r10: 0xffffff00000c67e8,
    r9: 0x1,
    r8: 0x4,
    rsi: 0x7fffffc0003eb0,
    rdi: 0xffffffffc0003eb0,
    rdx: 0xffffff0000850268,
    rcx: 0x7fc0003eb0,
    rax: 0xffffffffc0003eb0,
    trap_num: 0xe,
    error_code: 0x0,
    rip: 0xffffff0000027c80,
    cs: 0x8,
    rflags: 0x82,
    rsp: 0xffffff00000c5b68,
    ss: 0x0
}

##针对exit的测试（通过测试）：
I am the parent. Forking the child...
I am parent, fork a child pid 3
I am the parent, waiting now..
I am the child.
waitpid 3 ok.
exit pass.

##针对softint的测试（未通过测试）：
[ERROR] Process 2 error:
TrapFrame {
    r15: 0x0,
    r14: 0x0,
    r13: 0x0,
    r12: 0x0,
    rbp: 0xb00fff88,
    rbx: 0x0,
    r11: 0x0,
    r10: 0x0,
    r9: 0x0,
    r8: 0x0,
    rsi: 0x0,
    rdi: 0x0,
    rdx: 0x8016b4,
    rcx: 0xb00fffa0,
    rax: 0x6e657269,
    trap_num: 0xd,
    error_code: 0xe2,
    rip: 0x8015ad,
    cs: 0x23,
    rflags: 0x282,
    rsp: 0xb00fff80,
    ss: 0x2b
}

##针对badsegment的测试（通过测试）：
user panic at user/badsegment.c:9:
    FAIL: T.T

##针对hello的测试（通过测试）：
Hello world!!.
I am process 2.
hello pass.

##针对ls的测试（通过测试）：
无输出，由于没有文件系统这样的测试没有意义

##针对priority测试（通过测试）：
priority process will sleep 400 ticks
child pid 7, acc 4000, time 17346
child pid 6, acc 4000, time 17347
child pid 5, acc 4000, time 17348
child pid 4, acc 4000, time 17349
child pid 3, acc 4000, time 17349
main: fork ok,now need to wait pids.
main: pid 3, acc 4000, time 17350
main: pid 4, acc 4000, time 17351
main: pid 5, acc 4000, time 17351
main: pid 6, acc 4000, time 17352
main: pid 7, acc 4000, time 17352
main: wait pids over
stride sched correct result: 1 1 1 1 1

##针对badarg测试（未通过测试）：
[ERROR] 

PANIC in /Users/victor/.rustup/toolchains/nightly-2018-09-18-x86_64-apple-darwin/lib/rustlib/src/rust/src/libcore/option.rs at line 345
    called `Option::unwrap()` on a `None` value

##针对testbss测试（通过测试）：
Making sure bss works right...
user panic at user/testbss.c:14:
    bigarray[14248] isn't cleared!

##针对pgdir测试（未通过测试）：
I am 2, print pgdir.
[ERROR] unknown syscall id: 0x1f, args: [b00fff78, 80082f, 801a80, 2, b00fff88, 0]
[ERROR] Process 2 error:
TrapFrame {
    r15: 0x0,
    r14: 0x0,
    r13: 0x0,
    r12: 0x0,
    rbp: 0xb00fff5c,
    rbx: 0x801a80,
    r11: 0x0,
    r10: 0x0,
    r9: 0x0,
    r8: 0x0,
    rsi: 0xb00fff88,
    rdi: 0x2,
    rdx: 0xb00fff78,
    rcx: 0x80082f,
    rax: 0x1f,
    trap_num: 0x80,
    error_code: 0x0,
    rip: 0x8004ff,
    cs: 0x23,
    rflags: 0x202,
    rsp: 0xb00fff30,
    ss: 0x2b
}

##针对matrix测试（部分通过测试）：
pid 7 is running (4600 times)!.
pid 6 is running (1900 times)!.
pid 5 is running (1100 times)!.
pid 5 done!.
pid 4 is running (1000 times)!.
pid 4 done!.
pid 3 is running (1000 times)!.
pid 3 done!.
pid 6 done!.
pid 12 is running (13100 times)!.
pid 11 is running (2600 times)!.
pid 10 is running (37100 times)!.
pid 9 is running (20600 times)!.
pid 8 is running (11000 times)!.
pid 11 done!.
pid 7 done!.
pid 17 is running (23500 times)!.
pid 16 is running (2600 times)!.
pid 15 is running (23500 times)!.
pid 14 is running (4600 times)!.
pid 13 is running (37100 times)!.
pid 16 done!.
BUG: exit failed.
pid 14 done!.
pid 22 is running (26600 times)!.
pid 21 is running (2600 times)!.
pid 20 is running (13100 times)!.
pid 19 is running (33400 times)!.
pid 18 is running (4600 times)!.
pid 21 done!.
pid 18 done!.
fork ok.
pid 23 is running (13100 times)!.
pid 8 done!.
pid 23 done!.
pid 20 done!.
pid 12 done!.
pid 9 done!.
pid 17 done!.
pid 15 done!.
pid 22 done!.
pid 19 done!.
pid 13 done!.
pid 10 done!.

##针对sleepkill测试（通过测试）：
sleepkill pass.