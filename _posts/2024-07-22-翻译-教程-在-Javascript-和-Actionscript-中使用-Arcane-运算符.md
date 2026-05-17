---
layout: post
title: "【翻译】教程：在 Javascript 和 Actionscript 中使用 Arcane 运算符"
date: 2024-07-22
tags: ["JavaScript", "算法", "每天一个知识点", "Unity3D", "游戏开发", "操作系统"]
---
原文：[教程：在 Javascript 和 Actionscript 中使用 Arcane 运算符 (krazydad.com)](https://link.juejin.cn?target=https%3A%2F%2Fkrazydad.com%2Farcaneoperators%2F)

### 吉姆·布姆加德纳（jim bumgardner）的教程（[@jbum](https://link.juejin.cn/?target=https%3A%2F%2Ftwitter.com%2F%23!%2Fjbum)推特）

### 使用 JavaScript 和 ActionScript 中的神秘操作符

我偶尔会遇到一些有才华的 JavaScript（或 ActionScript 或 Processing）程序员，可能像你一样，他们以前没有太多编程经验，因此没有使用这些语言中较为晦涩的操作符。他们使用基本的算术和逻辑操作符没有问题，比如...

| 符号 | 语义 || --- | --- || + | 加法 || - | 减法 || * | 乘法 || / | 除法 || && | 逻辑与 || || | 逻辑或 || ! | 逻辑非 |
但他们对这些更为晦涩的运算符没有太多用处，这些运算符都是从 C 编程语言中继承来的：

| 符号 | 语义 || --- | --- || % | 取余 || & | 按位与 ||  |  | 按位 OR || ~ | 按位或 || 按位异或 || > | 右移运算符 |
作为一名老派的C语言程序员，我经常使用所有这些操作符。我想写这篇教程，来说明我在一些典型情况下如何使用它们，并提供一些示例代码片段。

## % 取模

取模运算符在整数除法后给出余数。例如，16 % 3 是 1。为什么呢？如果你将 16 除以 3，得到的结果是 5，余数是 1。这个“余数”部分就是取模运算符给出的结果。如果你对一系列递增的数字使用取模运算符，它会产生一个特征性的重复“条纹”模式，范围从 0 到 N-1，如下所示：

```ini
0 % 3 == 0
1 % 3 == 1
2 % 3 == 2
3 % 3 == 0
4 % 3 == 1
5 % 3 == 2
6 % 3 == 0
7 % 3 == 1
etc.
```

对于正数，取模运算的结果是可预测的。不幸的是，对于负数，结果会有所不同，取决于所使用的编程语言。JavaScript 具有以下模式，这在数学上是正确的：

```ini
-1 % 3 == -1
-2 % 3 == -2
-3 % 3 == 0
-4 % 3 == -1
-5 % 3 == -2
-6 % 3 == 0
-7 % 3 == -1
etc.
```

但其他语言，如 Perl，则具有以下模式，该模式基本上延续了你在正数中看到的模式：

```ini
-1 % 3 == 2
-2 % 3 == 1
-3 % 3 == 0
-4 % 3 == 2
-5 % 3 == 1
-6 % 3 == 0
-7 % 3 == 2
etc.
```

你可以为这两种情况提出论据，因为它们各有其用途。由于我个人很难记住这些语言特定的细节（而且我在多种不同的语言中编程），所以我倾向于在工作中避免使用负数的取模运算。现在，我会集中讨论正数，然后展示一个避免负数问题的技巧。

那种基本的条纹模式 0,1,2, 0,1,2, 0,1,2... 是非常有用的，当我处理具有左右箭头导航的软件时，我通常会使用它。例如，可能会有一个用户界面元素，其中包含一个“右箭头”，它会带你到“下一页”。如果我在最后一页，我通常希望“回绕”到第一页。 “回绕”是取模运算符的理想应用场景。

如果页码从 0 到 N-1（其中 N 是页数），而当前页码用 curPage 表示，那么按下右箭头可以调用一个像这样的脚本：

```ini
curPage = (curPage + 1) % N;
```

如果你不熟悉取模运算符，你可能会这样编写代码：

```ini
curPage = curPage + 1
if (curPage == N)
  curPage = 0;
```

如果 N 是 3，两段代码都会产生以下过渡效果：

```rust
0 -> 1
1 -> 2
2 -> 0
```

...但取模版本只有一行。这对我来说是取模运算符的经典应用。通常，我会使用零起始的数字来内部表示页码，以便更方便地使用取模运算符。但即使你的页码从 1 开始，你仍然可以通过如下调整公式来使用取模运算符：

```ini
curPage = ((curPage-1)+1 % N) + 1;
```

这可以简化为：

```ini
curPage = (curPage % N) + 1;
```

如果 N 是 3，这将产生以下过渡效果。现在让我们考虑如何处理左箭头，它将你带到上一页，并且在从第一页回绕到最后一页时也会循环。如果你的页码是从 0 到 N-1，你可以从以下公式开始：但请注意，如果 curPage 设置为 0（零），则可能会出现问题，这取决于取模运算符的行为。在 Perl 的取模实现中，这段代码有效（这可能是 Perl 以这种方式实现取模的原因），但在数学上正确的 JavaScript 取模实现中，这段代码不起作用，因此你需要特殊处理：这里有一个避免特殊处理的巧妙技巧：注意，这仍然有以下 N=3 的过渡表：如果你的页码从 1 开始，则首先将其转换为零起始数字（通过减去 1），执行操作，然后再转换回（通过加 1）：这具有以下 N=3 的过渡表：你可以通过使用 delta 变量来概括这两个方向，delta 变量跟踪旅行方向。如果向左移动，delta = -1；如果向右移动，delta = 1。在这种情况下，你可以对零起始页码（从 0 到 N-1）使用以下代码：对于从 1 到 N 的一基页码，你可以使用以下代码：你可以在需要“条纹”效果的任何地方使用取模运算符——甚至可以在图形软件中制作条纹！我也用它来处理游戏软件，其中宇宙飞船需要绕过屏幕——在右侧消失后重新出现在左侧。这看起来像这样：

```rust
1 -> 2
2 -> 3
3 -> 1
```

```ini
curPage = (curPage - 1) % N;
```

```ini
if (curPage == 0)
  curPage = N-1;
else
  curPage = (curPage - 1) % N;
```

```ini
curPage = (curPage + (N-1)) % N;
```

```rust
0 -> 2
1 -> 0
2 -> 1
```

```ini
curPage = ((curPage-1) + (N-1)) % N + 1;
```

```rust
1 -> 3
2 -> 1
3 -> 2
```

```ini
function gotoNextPage(delta)
{
  curPage = (curPage + N + delta) % N;
}
```

```ini
function gotoNextPage(delta)
{
  curPage = ((curPage-1) + N + delta) % N + 1;
}
```

```ini
spaceship.x = (spaceship.x + screenWidth + horizontalSpeed) % screenWidth;
spaceship.y = (spaceship.y + screenHeight + verticalSpeed) % screenHeight;
```

## & 按位与 | 按位或 ~ 按位非

以下是一些表格，展示了这些按位逻辑运算符的效果。我还包括了 ^ XOR 运算符，稍后我会进一步讨论。这些运算符被称为“按位”运算符，因为它们作用于操作数中的每一个单独的位。例如，在二进制中，数字 12 对应的十进制数是 12。

我最常使用这些按位逻辑运算符来维护一个标志集，这些标志存储在一个单一的数字变量中。为了理解这些运算符，你需要将涉及的数字的二进制表示形式可视化。

```ini
0 & 0 == 0
0 & 1 == 0
1 & 0 == 0
1 & 1 == 1

0 | 0 == 0
0 | 1 == 1
1 | 0 == 1
1 | 1 == 1

0 ^ 0 == 0
0 ^ 1 == 1
1 ^ 0 == 1
1 ^ 1 == 0

~0 == 1
~1 == 0
```

```ini
00101101 & 00011100 == 00001100
```

```ini
45 & 28 == 12
```

假设我有一个游戏，涉及动物的不同特征，这些特征通过标志来跟踪。

```ini
kHasTeeth   = 1;
kHasFur     = 2;
kHasScales  = 4;
kHasLungs   = 8;
kHasGills   = 16;
kHasTusks   = 32;
```

需要注意的是，这些标志对应于二进制数中的单个位：我们可以使用按位或 (|) 运算符将这些标志组合成一个单一的“特征”变量。我们可以使用按位与 (&) 运算符来测试某个特定标志是否被开启。以下是一个更复杂的例子：按位取反 (~) 运算符给出其操作数的相反位模式，通常扩展到 32 位。这个运算符通常与按位与 (&) 运算符结合使用，以清除标志中的单个位。例如，以下代码会关闭 `kHasHair` 位。

这可以简化为：

注意，这段代码会关闭 `kHasHair` 位（如果它已开启），否则不会产生任何效果。如果你想要切换该位的值，可以使用 XOR (^) 运算符，而不进行取反。有关 XOR 的更多信息，请参见下文……

```ini
// decimal    binary
kHasTeeth   = 1;    // 00000001
kHasHair    = 2;    // 00000010
kHasScales  = 4;    // 00000100
kHasLungs   = 8;    // 00001000
kHasGills   = 16;   // 00010000
kHasTusks   = 32;   // 00100000
```

```ini
if (animal == "sheep")
  characteristics = kHasTeeth | kHasHair | kHasLungs;
else if (animal == "snake")  
  characteristics = kHasScales | kHasLungs;
else if (animal == "goldfish")  
  characteristics = kHasScales | kHasGills;
else if (animal == "shark")  
  characteristics = kHasTeeth | kHasScales | kHasGills;
else if (animal == "elephant")  
  characteristics = kHasTusks | kHasLungs | kHasHair;
```

```ini
if ( (characteristics & kHasTeeth) )
   message = "This animal has teeth.";
```

```ini
if ( (characteristics & kHasScales) && (characteristics & kHasLungs) )
   message = "This animal is probably a reptile.";
else if ( (characteristics & kHasScales) && (characteristics & kHasGills) )
   message = "This animal is probably a fish.";
```

```ini
X = 1      // x = 00000000000000000000000000000001
X = ~X;    // x = 11111111111111111111111111111110
```

```ini
characteristics = (characteristics & ~kHasHair);
```

```ini
characteristics &= ~kHasHair;
```

```ini
characteristics ^= kHasHair;
```

## ^按位异或 XOR

按位异或运算符XOR是我最喜欢的运算符之一，因为它通常在一些有趣的复杂技巧中扮演重要角色。

按位异或的效果是：

```css
A = A ^ B
```

或是切换 A 中与 B 中设置（或开启）的位相对应的任何位。如果这些位在 A 中被关闭，它们会被开启。如果这些位在 A 中被开启，它们会被关闭。

```css
A ^= B
```

有趣的是，如果你应用两次，你会得到相同的结果。例如，如果你执行以下代码：

```ini
myVar = (myVar ^ X) ^ X
```

或者，如果 X 是任何整数值，那么 `myVar` 的值不会改变。

```
myVar ^= X
myVar ^= X
```

如上所述，XOR 的切换特性使其非常适合用来切换标志。

```ini
myFlags = (myFlags ^ SomeFlag);
```

或

我曾经使用 XOR 的一个更不常见的用途是交换两个整数变量的值，而无需使用第三个临时变量。通常，如果你想交换两个整数变量，你会这样做：

然而，在我还关心如最小化寄存器数量等事情的日子里，我会写这样的代码：

……它具有相同的效果。让我们通过一些实际的值来分析发生了什么，以直观地理解原因。假设 X 等于 6，Y 等于 5。以下表格显示了二进制中的情况。瞧！它们已经被交换了！

```ini
myFlags ^= SomeFlag;
```

```ini
var temp = x;
x = y;
y = temp;
```

```ini
x ^= y;
y ^= x;
x ^= y;
```

```ini
X               Y
original values       110             101
x ^= y                011             101   x=x^y
y ^= x                011             110   y=y^(x^y) or y=x
x ^= y                101             110   x=(x^y)^x or x=y
```

XOR 经常用于简单（且容易破解）的加密算法，因为你可以将其应用于一系列字符代码，首先进行加密，然后进行解密。

##  左移运算符 >> 右移运算符

左移运算符将一个数字中的所有位向左移动。例如：

注意，将一个数字左移 1 位等同于将其乘以 2。左移 N 位等同于将其乘以 2 的 N 次方。同样，右移运算符等同于对 2 的 N 次方进行整数除法。

```vbnet
x = 6                   // X is 110 in binary
x = (x << 1)            // X is now 1100 in binary, or 12 in decimal.
```

在 CPU 速度是一个主要问题的时代，你可以通过将乘法和除法操作替换为左移和右移来获得速度提升。然而，在像 JavaScript 和 ActionScript 这样的高级语言中，这种优化没有实际的收益。

我确实经常使用移位运算符将序号转换为标志。例如，在我的数独软件中，我使用一个数字来指示某个特定单元格的数字（从 1 到 9）有哪些可能性。

我可以使用如下代码将一个数字设置为可能性：

```ini
possibles = (possibles | (1 << digit));
```

或者，我可以使用如下代码清除一个数字作为可能性：

或者，如果有 9 个可能的数字（1-9），我可以一次性设置所有可能的数字，如下所示：

不过，请注意，这段代码假设我使用的是从 0 到 N-1 的位标志，而不是从 1 到 N 的位标志。作为一名老派程序员，我通常在可以的情况下使用零起始的计数系统，因为这简化了结果代码。

```bash
possibles |= (1 << digit);
```

```ini
possibles = (possibles & ~(1 << digit));
```

```bash
possibles &= ~(1 << digit);
```

```ini
N = 9
possibles = (1 << N)-1;
```

另一种我非常常用的方式是将单独的 R、G 和 B 值转换成颜色，这涉及使用移位和按位逻辑运算符。

```ini
compositeColor = (red << 16) | (green << 8) | blue;
```

[亨利·沃伦 （Henry S. Warren） 的 《黑客的喜悦](https://link.juejin.cn?target=https%3A%2F%2Fwww.amazon.com%2FHackers-Delight-2nd-Henry-Warren%2Fdp%2F0321842685)》是一本很棒的书 它更深入地研究了许多此类技巧。您可以从[作者的网站](https://link.juejin.cn?target=http%3A%2F%2Fwww.hackersdelight.org%2F)免费下载第 2 章“基础知识”。

有关颜色操作的更多信息，请查看我的教程[“在 Javascript 中制作烦人的彩虹”](https://link.juejin.cn?target=https%3A%2F%2Fkrazydad.com%2Fmakecolors.php)。

这篇博客提供了有关在 JavaScript 和 ActionScript 中使用高级操作符的资源。网站详细介绍了各种操作符，如取模（%）、按位与（&）、按位或（|）、按位取反（~）、异或（^）等，以及它们在编码中的实际应用。这些操作符通过实例展示了它们在分页管理、标志处理和游戏开发中特效创建等方面的使用。该网站旨在通过提供详细的解释和用例，提升程序员使用这些不常用但强大操作符的技能。

更多参考请参阅：[表达式和运算符 - JavaScript | MDN (mozilla.org)](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FOperators)

本文由Microsoft Edge内置翻译工具、ChatGPT4o翻译，人工校对。
