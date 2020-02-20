---
layout: post
title: Stack protection in GCC/Linux
date: 2020-02-20 10:25
category: Notes
author: Rohan Garg
tags: [linux, gcc, stack, security]
summary: Canaries and stack smashing detection in GCC/Linux
---

Here's how GCC implements stack overflow protection (i.e., when one
compiles with `-fstack-protector`).

Stack check guard is implemented using thread-local storage (TLS). GCC
inserts this assembly at function entry (in the prologue), where it
pushes a "canary" (which is derived from `AT_RANDOM` aux vector) from
thread-local storage (using, our friend, the $fs register) to the
stack.

(Recall that TLS is implemented using the $fs register on Linux x86-64.)

So, when exiting from a function in a library (which has this stack
protection code enabled), if there's a mismatch between the value on
the stack and the value in the TLS, we end up triggering the stack check
failure assert: `if (stack value != TLS value) assert(stack_check_failed)`.

Function prologue from the `sleep()` function in libc.so:

```
                                  push   %rbp             # Push (or save) stack base pointer on the stack (function prologue)
  0x7fa9aa6d9821 <__sleep+1>      push   %rbx             # Push $rbx on the stack
  0x7fa9aa6d9822 <__sleep+2>      sub    $0x28,%rsp       # Create stack frame of 40 bytes
  0x7fa9aa6d9826 <__sleep+6>      mov    0x2f264b(%rip),%rbx
  0x7fa9aa6d982d <__sleep+13>     mov    %fs:0x28,%rax    # Get canary from TLS (fs base address + 40 bytes) and ...
  0x7fa9aa6d9836 <__sleep+22>     mov    %rax,0x18(%rsp)  # Save canary to stack; this can be used for verification later on, when exiting the function
  ...
```

In case of buffer overflow/stack overflow/stack smashing, the code is likely
to end up modifying the canary stored in the stack. This is caught later
while exiting the function (in the epilogue).

Function epilogue from the sleep() function in libc.so:
```
  0x7fa9aa6d9863 <__sleep+67>     mov    0x18(%rsp),%rdx              # Read the saved canary value from the stack
  0x7fa9aa6d9868 <__sleep+72>     xor    %fs:0x28,%rdx                # Get canary from TLS (fs base address + 40 bytes) and compare against stack value
  0x7fa9aa6d9871 <__sleep+81>     jne    0x7fa9aa6d9885 <__sleep+101> # Jump and call __stack_chk_fail(), if values are not equal
  ...
                                  pop    %rbx                         # Pop (restore) $rbx
                                  mov    %ebp,%eax
                                  pop    %rbp                         # Pop (restore) stack base pointer
                                  retq                                # Return from function
  ...
  0x7fa9aa6d9885 <__sleep+101>    callq  0x7fa9aa71e0e0 <__stack_chk_fail>
```

(Note that not all the glibc functions have this stack protection code --
 `nanosleep()` being an example of such a function with no stack protection.)
