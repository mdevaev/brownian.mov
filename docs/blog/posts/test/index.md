---
title: Test Post
date: 2025-03-23
slug: test-post
description: >
    The first test post
categories:
    - General
authors:
    - mdevaev
    - yar
links:
    - Google: https://google.com
    - PiKVM: https://pikvm.org
---

# Test Post

The first test post.

<!-- more -->

This is the content of the test post.

```c
#include <stdio.h>
#include <stdint.h>


/*
ulong 429_wrap_2msb(ulong val)
{
  uint uVar1;

  uVar1 = val << 3 | val >> 0x1d;
  if ((val >> 0x1d & 1) != 0) {
     uVar1 = uVar1 | 0x200;
  }
  if ((uVar1 & 2) != 0) {
     uVar1 = uVar1 | 0x400;
  }
  return CONCAT31((int3)(uVar1 >> 8),0xef);
}*/


uint32_t p429_wrap_2msb(uint32_t val)
{
    val = (val << 3) | (val >> 0x1d);
    if ((val & 1) != 0) {
        val = val | 0x200;
    }
    if ((val & 2) != 0) {
        val = val | 0x400;
    }
    val &= 0xFFFFFF00;
    val |= 0xEF;
    return val;
}

void send(uint16_t b) {
    uint32_t msg = (0x0000 | b) << 16;
    msg = p429_wrap_2msb(msg);
//  if (__builtin_popcount(msg) % 2 == 0) {
//      msg |= (1 << 24);
//  }
    printf("%032b\n", __builtin_bswap32(msg));
}

int main(void) {
    send(0x7F);
    return 0;
}
```
