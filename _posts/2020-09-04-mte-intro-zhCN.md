---
layout: post
title:  ARM MTE 科普
date:   2020-09-04
categories: ios
---

## ARM MTE 科普

### 词汇
1. Tag: 标记
2. MT: Memory Tagging
3. MTE: Memory Tagging Extension
4. TG: Tag Granule，Tag 单元
5. TS: Tag Size

### 指针
C/C++ 中的指针给了程序员很大的灵活性，但同时也带来了很多的内存损坏问题。
内存损坏之所以难以检测主要是因为：
* 指针与其所指向的内存并不是严格的一对一关系。
* 指针中没有包含其所指向的内存是否存活的标记。
* 指针中没有包含其所指向的内存的范围。

### MT 的目的
MT 主要是为了解决上面提到的内存损坏相关问题，即：
* 争取在指针与其指向的内存间建立映射关系。
* 争取标记指针指向的内存是否存活。
* 争取标记指针指向的内存的范围。

### MT 的原理
MT 的原理或者解决问题的方式是：给内存打上 Tag，同时给指针打上 Tag，通过这 2 个 Tag 来解决内存相关问题。
注：
1. 指针的 Tag 与指针指向的内存的 Tag 可以相同，也可以不同，但必须保证可验证。而在实际的实现中，二者应该是不同的。
2. Tag 是什么？可以简单把 Tag 理解为一个数字，如：0b1101。

抽象的看，这里会涉及 3 个部分：指针、内存、指针与内存的映射关系（即：2 个 Tag 是否匹配）。
注：指针 Tag，可以 InPlace 形式来存储，也可以单独存储。内存的 Tag，为了保持一致性，应该需要独立的空间/单元来存储。

以下面的代码为例来说明基本的工作流程与原理，
为了更好理解，我们做假设：
1. 指针与其 Tag 独立存储，即：存在`指针与其 Tag 的字典`，Key 是指针，Value 是指针的 Tag。
2. 内存与其 Tag 独立存储，即：存在`内存与其 Tag 的字典`，Key 是内存，Value 是内存的 Tag。
```c
uint8_t ptr[8] = {0};
*ptr = 1;
```

1. `uint8_t ptr[8] = {0};`: 
  1). 首先获取 `8 个字节的内存`。
  2). 计算、选取一对 Tag，如：：Tag-P, Tag-M。
  3). 将 ptr 与 Tag-P 保存到 `指针与其 Tag 的字典`。
  4). 将 `8 个字节的内存` 与 Tag-P 保存到 `内存与其 Tag 的字典`。
2. `*ptr = 1;`:
  1). 以指针作为 Key，从`指针与其 Tag 的字典`中查询出指针的 Tag：Tag-P.
  2). 以`8 个字节的内存`作为 Key，从`内存与其 Tag 的字典`中查询出内存的 Tag：Tag-M.
  3). 验证 Tag-P 与 Tag-M 是否匹配。


说明：
1. 上面的例子中，关键就是两个映射。
2. 在上面的例子中，我们假设的是：指针与其 Tag 独立存储。而在实际中，在 ARM 平台，可以把 Tag 保存到指针的高位。指针的高位是有限的，这就引出一个概念 `TS: Tag Size`，即：指针的高位中有多少位用来保存 Tag。
3. 在上面的例子中内存的大小是 8 字节，如果是一字节呢 `uint8_t ptr[1] = {0};` ？这就引出另一个概念 `TG: Tag Granule`，即：最少的可以标记的字节是多少，Tag 单元。
3. `TS` 越大越好，`TG` 越少越好。
5. MT 既可以使用硬件来实现，也可以使用软件来实现，所以造成上面的例子中，有些东西讲得很模糊，希望我表达清楚了 Key Point。


### MT 与内存损坏问题
从内存错误的角度来看，MT 具体是如何解决问题的：
* 指针与其指向的内存间建立映射关系：指针的 Tag 与内存 Tag 匹配。
* 争取标记指针指向的内存是否存活：释放后，修改内存的标记，造成指针的标记与内存的标记不匹配。
* 争取标记指针指向的内存的范围：并不能精确表示内存范围，但可以不同的内存的单元使用不同的 Tag 来大致标识不同的内存单元。

### ARM MTE 的原理
* 这是基于硬件的 MT，内存指的是物理内存。
* 内存的 Tag 被称为：Lock，上面例子中的 Tag-M。
* 指针的 Tag 被称为：Key，上面例子中的 Tag-P。
* `指针与其 Tag 的字典` 是什么？不存在这个字典，指针的 Tag 被 InPlace 存储到了指针的高位中，因为在 `Armv8-A` 上指针的高位被忽略。
* `TS`：4 Bits，即：一共 16 个值，16 种可能性。
* `TG`：16 Bytes。每 16 个 Byte 使用同一种内存标记。下面的错误，并不会被 MTE 检测到
	```c
	uint8_t ptr[1] = {0};
	ptr[15] = 1;
	```
* `内存与其 Tag 的字典` 具体是什么 ？应该是存在一个硬件单元来负责这个事情。

### Reference
1. [Armv8.5-A Memory Tagging Extension](https://developer.arm.com/-/media/Arm%20Developer%20Community/PDF/Arm_Memory_Tagging_Extension_Whitepaper.pdf)
2. [Memory Tagging and how it improves C/C++ memory safety](https://arxiv.org/pdf/1802.09517.pdf)
3. [ARM v8.5 Memory Tagging Extension](https://www.linuxplumbersconf.org/event/4/contributions/571/attachments/399/642/MTE_LPC.pdf)
