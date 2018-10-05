#RiscvRust测试报告
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

##针对sh的测试（测试没有通过，需要增加0x66号系统调用）：
user sh is running!!!$ [ERROR] unknown syscall id: 0x66, args: [0, 7000ff97, 1, 7000ff70, 25, 73]
[ERROR] Process 2 error:
TrapFrame {
    x: [
        0x0,
        0x800e1c,
        0x7000ff38,
        0x0,
        0x0,
        0x0,
        0x7000ff70,
        0x0,
        0x0,
        0x3,
        0x66,
        0x0,
        0x7000ff97,
        0x1,
        0x7000ff70,
        0x25,
        0x73,
        0x20,
        0x1f,
        0xffe,
        0xd,
        0x8,
        0xa,
        0x804000,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0
    ],
    sstatus: Sstatus {
        bits: 0x80046000
    },
    sepc: 0x800144,
    sbadaddr: 0x0,
    scause: Scause {
        bits: 0x8
    }
}

##针对forktest的测试（测试通过）：
I am child 31
I am child 30
I am child 29
I am child 28
I am child 27
I am child 26
I am child 25
I am child 24
I am child 23
I am child 22
I am child 21
I am child 20
I am child 19
I am child 18
I am child 17
I am child 16
I am child 15
I am child 14
I am child 13
I am child 12
I am child 11
I am child 10
I am child 9
I am child 8
I am child 7
I am child 6
I am child 5
I am child 4
I am child 3
I am child 2
I am child 1
I am child 0
forktest pass.

##针对faultread的测试（测试未通过）：
[ERROR] Process 2 error:
TrapFrame {
    x: [
        0x0,
        0x8002e0,
        0x7000ffe8,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x7000fffc,
        0x0,
        0x7000fffc,
        0x1,
        0x0,
        0x0,
        0x800a14,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0
    ],
    sstatus: Sstatus {
        bits: 0x80046000
    },
    sepc: 0x80099c,
    sbadaddr: 0x0,
    scause: Scause {
        bits: 0xd
    }
}

##针对forktree的测试（测试未通过）：
forktree process will sleep 400 ticks
0002: I am ''
0004: I am '1'
0005: I am '11'
0007: I am '111'
0009: I am '1111'
0008: I am '1110'
0006: I am '110'
000b: I am '1101'
000a: I am '1100'
0003: I am '0'
[ERROR] PANIC in src/lang.rs at line 22
    out of memory

##针对divzero的测试（测试通过）：
value is -1.
user panic at user/divzero.c:9:
    FAIL: T.T

##针对yield的测试（测试通过）：
Hello, I am process 2.
Back in process 2, iteration 0.
Back in process 2, iteration 1.
Back in process 2, iteration 2.
Back in process 2, iteration 3.
Back in process 2, iteration 4.
All done in process 2.
yield pass.

##针对faultreadkernel测试（测试不通过）：
[ERROR] Process 2 error:
TrapFrame {
    x: [
        0x0,
        0x800340,
        0x7000ffe8,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x7000fffc,
        0x0,
        0x7000fffc,
        0x1,
        0x0,
        0x0,
        0xfac00000,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0
    ],
    sstatus: Sstatus {
        bits: 0x80046000
    },
    sepc: 0x800a00,
    sbadaddr: 0xfac00000,
    scause: Scause {
        bits: 0xd
    }
}

##针对exit的测试（测试通过）：
I am the parent. Forking the child...
I am parent, fork a child pid 3
I am the parent, waiting now..
I am the child.
waitpid 3 ok.
exit pass.

##针对softint的测试（测试通过）：
无输出

##针对badarg的测试（测试通过）：
user panic at user/badsegment.c:9:
    FAIL: T.T

##针对hello的测试（测试通过）：
Hello world!!.
I am process 2.
hello pass.

##针对ls的测试（测试通过）：
由于还没有实现文件系统因此该测试没有意义

##针对priority测试（测试通过）：
priority process will sleep 400 ticks
main: fork ok,now need to wait pids.
child pid 7, acc 4000, time 13131
child pid 6, acc 4000, time 13132
child pid 5, acc 4000, time 13132
child pid 4, acc 4000, time 13132
child pid 3, acc 4000, time 13132
main: pid 3, acc 4000, time 13132
main: pid 4, acc 4000, time 13132
main: pid 5, acc 4000, time 13133
main: pid 6, acc 4000, time 13133
main: pid 7, acc 4000, time 13133
main: wait pids over
stride sched correct result: 1 1 1 1 1

##针对badarg测试（测试不通过）：
[ERROR] Process 2 error:
TrapFrame {
    x: [
        0x1c0,
        0x800368f8,
        0x80157cd0,
        0x0,
        0x0,
        0x0,
        0x80036e18,
        0x0,
        0x7000ff78,
        0x800f7620,
        0xff9c0030,
        0x10,
        0x1,
        0xffffffff,
        0x21,
        0xffffffff,
        0x1f,
        0x20,
        0xff9c0030,
        0x1,
        0x802f7248,
        0x800f7620,
        0x201fe001,
        0xcafebff8,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0
    ],
    sstatus: Sstatus {
        bits: 0x80046100
    },
    sepc: 0x80036910,
    sbadaddr: 0xff9c0030,
    scause: Scause {
        bits: 0xd
    }
}

##针对testbss测试（测试不通过）：
[ERROR] 
PANIC in /Users/victor/.rustup/toolchains/nightly-2018-09-18-x86_64-apple-darwin/lib/rustlib/src/rust/src/libcore/option.rs at line 1000
    failed to allocate frame

##针对pgdir测试（测试不通过）：
I am 2, print pgdir.
[ERROR] unknown syscall id: 0x1f, args: [a, ffff6ad9, 2, 0, 15, ffffffff]
[ERROR] Process 2 error:
TrapFrame {
    x: [
        0x0,
        0x8009d4,
        0x7000ff88,
        0x0,
        0x0,
        0x0,
        0x800164,
        0x0,
        0x0,
        0x7000fffc,
        0x1f,
        0xa,
        0xffff6ad9,
        0x2,
        0x0,
        0x15,
        0xffffffff,
        0x20,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0,
        0x0
    ],
    sstatus: Sstatus {
        bits: 0x80046000
    },
    sepc: 0x8000e4,
    sbadaddr: 0x0,
    scause: Scause {
        bits: 0x8
    }
}

##针对matrix测试（测试通过）：
fork ok.
pid 25 is running (17900 times)!.
pid 25 done!.
pid 24 is running (33400 times)!.
pid 23 is running (3500 times)!.
pid 23 done!.
pid 22 is running (15400 times)!.
pid 22 done!.
pid 21 is running (37100 times)!.
pid 20 is running (5900 times)!.
pid 20 done!.
pid 19 is running (26600 times)!.
pid 18 is running (3500 times)!.
pid 18 done!.
pid 17 is running (26600 times)!.
pid 16 is running (5900 times)!.
pid 16 done!.
pid 15 is running (41000 times)!.
pid 14 is running (15400 times)!.
pid 14 done!.
pid 13 is running (3500 times)!.
pid 13 done!.
pid 12 is running (41000 times)!.
pid 11 is running (23500 times)!.
pid 11 done!.
pid 10 is running (13100 times)!.
pid 10 done!.
pid 9 is running (5900 times)!.
pid 9 done!.
pid 8 is running (2600 times)!.
pid 8 done!.
pid 7 is running (1400 times)!.
pid 7 done!.
pid 6 is running (1100 times)!.
pid 6 done!.
pid 5 is running (1100 times)!.
pid 5 done!.
pid 24 done!.
pid 21 done!.
pid 19 done!.
pid 17 done!.
matrix pass.
pid 15 done!.
pid 12 done!.

##针对sleepkill测试（测试通过）：
[ERROR] 

PANIC in /Users/victor/RustOS/RustOS/crate/process/src/event_hub.rs at line 55
    attempt to add with overflow