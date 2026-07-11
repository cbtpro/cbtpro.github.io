---
layout: post
title: "使用 `echo` 命令美化 Shell 输出"
date: 2024-11-20
tags: ["前端", "后端", "代码规范", "算法", "程序员", "Java"]
---
## 使用 echo 命令美化 Shell 输出

在 Shell 脚本中，`echo` 是一个常用的命令，用于向终端打印文字信息。为了让输出更有表现力，我们可以通过控制字符实现文字的颜色、字体、大小和特效（如闪烁）。本文将深入介绍如何利用 `echo` 命令的功能美化输出。

### 基本用法

`echo` 命令的基本用法非常简单：

```bash
echo "Hello, World!"
```

输出：

![echo Hello World 输出](/assets/images/2024/11/20/echo-output-1.webp)

加上选项 `-e` 可以支持转义字符，例如换行符 `\n` 和制表符 `\t`：

```bash
echo -e "Hello,\nWorld!"
```

输出：

![echo 转义字符输出](/assets/images/2024/11/20/echo-output-2.webp)

### 使用 ANSI 转义序列为文字添加颜色

ANSI 转义序列是一种控制终端文本显示样式的方法。可以通过 `echo` 输出这些转义序列来改变文字颜色、背景颜色、添加下划线等。

#### 常见的 ANSI 转义序列格式

```bash
echo -e "\033[<style>;<text color>;<background color>m<your text>\033[0m"
```

- `\033[`：表示开始转义序列。
- `<style>`：文字样式（如粗体、下划线）。
- `<text color>`：文字颜色。
- `<background color>`：背景颜色。
- `m`：结束样式设置。
- `\033[0m`：重置样式。

#### 文字颜色代码

| 颜色 | 前景色代码 | 背景色代码 |
| --- | --- | --- |
| 黑色 | 30 | 40 |
| 红色 | 31 | 41 |
| 绿色 | 32 | 42 |
| 黄色 | 33 | 43 |
| 蓝色 | 34 | 44 |
| 品红 | 35 | 45 |
| 青色 | 36 | 46 |
| 白色 | 37 | 47 |

#### 文字样式代码

| 样式 | 代码 |
| --- | --- |
| 默认样式 | 0 |
| 粗体 | 1 |
| 下划线 | 4 |
| 闪烁 | 5 |
| 反显 | 7 |
| 隐藏 | 8 |

#### 示例：彩色文字输出

##### 红色文字

```bash
echo -e "\033[31m这是红色文字\033[0m"
```

输出：

![红色文字输出](/assets/images/2024/11/20/echo-output-3.webp)

##### 绿色文字 + 黄色背景

```bash
echo -e "\033[32;43m绿色文字黄色背景色\033[0m"
```

![绿色文字黄色背景输出](/assets/images/2024/11/20/echo-output-4.webp)

##### 加粗蓝色文字

```bash
echo -e "\033[1;34m蓝色粗体\033[0m"
```

![加粗蓝色文字输出](/assets/images/2024/11/20/echo-output-5.webp)

##### 闪烁的红色文字

```bash
echo -e "\033[5;31m闪烁的红色文字\033[0m"
```

![闪烁红色文字输出](/assets/images/2024/11/20/echo-output-6.webp)

### 动态文字效果

除了单一的样式，我们可以通过 `echo` 和 Shell 脚本结合实现一些动态效果。

#### 逐字输出效果

```bash
text="在大街上，我看见一个杀手

他信步向我走来

缓缓地，把我的心杀死了"
for ((i=0; i<${#text}; i++)); do
  echo -n "${text:$i:1}"
  sleep 0.1
done
echo
```

##### 输出

![逐字输出效果](/assets/images/2024/11/20/echo-output-7.webp)

#### 循环闪烁效果

```bash
for i in {1..5}; do
  echo -e "\033[5;31m闪烁文字\033[0m"
  sleep 0.5
  clear
done
```

![循环闪烁效果](/assets/images/2024/11/20/echo-output-8.webp)

### 控制文字大小

Shell 的默认终端并不支持直接调整文字大小，但可以通过控制字体比例实现伪文字大小。

#### 伪大字体（ASCII Art）

可以用工具如 `figlet` 或 `toilet`：

```bash
sudo apt install figlet    # 使用 apt 安装 figlet
# brew install figlet # 使用 brew 安装 figlet
figlet "Big Text"
```

输出：
![figlet 大字体输出](/assets/images/2024/11/20/echo-output-9.webp)

可以访问使用`man figlet`或者查看官网[www.figlet.org/examples.ht…](http://www.figlet.org/examples.html) 查看更多细节，这里不展开了。

figlet 还是 node 版本的[www.npmjs.com/package/fig…](https://www.npmjs.com/package/figlet)

他们都不支持中文。

### 组合样式

你可以组合多种样式，创造更复杂的输出。

#### 示例：组合输出

```bash
echo -e "\033[1;4;33;44m粗体带下划线颜色文字蓝色背景色\033[0m"
```

### 把样式封装为函数

为了方便使用，可以将样式封装成函数。

#### 创建彩色输出函数

```bash
color_text() {
  echo -e "\033[$1;$2m$3\033[0m"
}

color_text "1" "31" "This is bold red text"
color_text "4" "34" "This is underlined blue text"
```

对于非常常用的颜色，比如红色的错误、黄色警告、绿色成功也可以单独封装成语义化的函数来使用，我下面将它封装成了一个`shell_utils.sh`的文件。

```shell
# 定义颜色常量
# 白色
text_color_black=30
# 白色背景
bg_color_black=40
# 红色
text_color_red=31
# 红色背景
bg_color_red=41
# 绿色
text_color_green=32
# 绿色背景
bg_color_green=42
# 黄色
text_color_yellow=33
# 黄色背景
bg_color_yellow=43
# 蓝色
text_color_blue=34
# 蓝色背景
bg_color_blue=44
# 品红
text_color_azaleine=35
# 品红背景
bg_color_azaleine=45
# 青色
text_color_cyan=36
# 青色背景
bg_color_cyan=46
# 白色
text_color_white=37
# 白色背景
bg_color_white=47

# 文字样式代码
# 默认样式
text_style_default=0
# 粗体
text_style_bold=1
# 下划线
text_style_underline=4
# 闪烁
text_style_flash=5
# 反显
text_style_back_display=7
# 隐藏
text_style_hidden=9

# 打印颜色文字
color_text() {
  echo -e "\033[$1;$2m$3\033[0m"
}
# 黄色文字
color_yellow_text() {
  color_text ${text_style_bold} ${text_color_yellow} $1
}
# 红色文字
color_red_text() {
  color_text ${text_style_bold} ${text_color_red} $1
}
# 绿色文字
color_green_text() {
  color_text ${text_style_bold} ${text_color_green} $1
}

# 定义打字机效果
typewriter_effect() {
  # 从参数中获取文本
  local text="$1"
  # 设置每个字符的延迟，默认为 0.1 秒
  local delay=${2:-0.1}

  for ((i=0; i<${#text}; i++)); do
    echo -n "${text:$i:1}"
    sleep "$delay"
  done
  # 输出换行
  echo
}
```

很多人觉得这是多此一举，认为是脱裤子放屁，直接用`echo -e "\033[;;m\033[0m"`就可以了，这不是我月薪 3000 考虑的事情等等借口来推脱。如果你只想码代码、而不想着提高代码质量，无形之中就在生产屎山代码。

提高代码复用性、提高代码可读性和可维护性、简化调试和测试、解耦代码，降低模块间依赖、提高扩展性、强制设计规范。
当你使用时不需要关注函数具体实现，只需要调用方法即可、当你需要修改、新增函数时，只需要关注这一个文件即可。
这些都是大型项目产生优秀代码不可或缺的。

### 注意事项

1. **兼容性**：ANSI 转义序列在大多数现代终端（如 Linux 和 macOS）中都能正常工作，但某些终端可能不支持。
2. **清除样式**：始终在样式结束后使用 `\033[0m` 重置，否则后续输出可能继承样式。

### 使用实践

我创建了`print_my_env.sh`的文件，并在我的终端中使用，每次打开终端时就会输出当前环境信息。

文件`print_my_env.sh`

```shell
source ~/shell_utils.sh

# 打开命令行时输出信息
color_yellow_text "java信息："
color_green_text "`java --version`"
# color_green_text "`java -version`"

color_yellow_text "adb版本: "
color_green_text "`adb --version`"

color_yellow_text "nvm版本："
color_green_text "`nvm --version`"

color_yellow_text "node版本："
color_green_text "`node -v`"

color_yellow_text "npm版本："
color_green_text "`npm -v`"

color_yellow_text "pnpm版本："
color_green_text "`pnpm --version`"

color_yellow_text "brew版本："
color_green_text "`brew -v`"

color_yellow_text "deno版本："
color_green_text "`deno --version`"

color_yellow_text "git版本："
color_green_text "`git --version`"

color_yellow_text "rust版本："
color_green_text "`rustc --version`"

color_yellow_text "redis版本："
color_green_text "`redis-server --version`"
echo -e ""
echo -e "可以使用docker创建指定版本和配置的redis"
echo -e ""
```

我使用的是`mac`的`zsh`

文件`.zshrc`

```shell
# 其他环境配置
source ~/print_my_env.sh
```

![终端环境信息输出](/assets/images/2024/11/20/echo-output-10.webp)

有了这个工具，我只需要打开终端，就可以了解到我的开发环境的版本，了解自己的开发环境版本是开发过程中非常重要的一环，因为开发环境的版本会直接影响到代码的兼容性、功能可用性和开发效率。

### 总结

通过 `echo` 和 ANSI 转义序列，你可以让 Shell 脚本的输出变得更加丰富多彩。无论是调整颜色、字体样式，还是实现动态效果，这些技巧都能为你的终端输出增添趣味。如果结合工具如 `figlet`，还可以打造出更加个性化的输出效果！

现在，试着用这些技巧美化你的 Shell 输出吧！🎨
