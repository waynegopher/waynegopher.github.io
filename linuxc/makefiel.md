---
title: "Makefiel"
date: 2021-10-24T14:36:23+08:00
draft: true
---

# makefile 基础

## makefile 变量
~~~makefile
# makefile 变量的使用
obiects = main.o b.o c.o
main: $(objects)
~~~

## 不同赋值方法
赋值方法包括`= := ?= +=`
~~~makefile
a = 1
b = $(name)
a = 2
# 这里 b 的值为 2

c = 1
d := $(c)
c = 2
# 这里 d 的值为 1

e = 1
e ?= 2
f ?= 3
# 这里 e 为 1 f 为 3(存在值 ?= 不会赋值,不存在才赋值)

g = main.o hello.o
g += ok.o
# 这里 g的值为 main.o hello.o ok.o
~~~
## 模式规则进行文件的匹配
### 自动化变量
|变量|描述|
|:-:|:-|
|$@|规则中的目标集合，在模式规则中，如果有多个目标话，“$@”表示匹配模式中定义的目标集合。|
|$%|当目标是函数库的时候表示规则中的目标成员名，如果目标不是函数库文件，那么其值为空。|
|$<|依赖文件集合中的第一个文件，如果依赖文件是以模式(即“%” )定义的，那么“$<”就是符合模式的一系列的文件集合。|
|$?| 所有比目标新的依赖目标集合，以空格分开。|
|$^|所有依赖文件的集合，使用空格分开，如果在依赖文件中有多个重复的文件，“$^”会去除重复的依赖文件，值保留一份。|
|$+| 和“$^”类似，但是当依赖文件存在重复的话不会去除重复的依赖文件。|
|$*|这个变量表示目标模式中"%"及其之前的部分，如果目标是 test/a.test.c，目标模式为 a.%.c，那么“$*”就是 test/a.test。|
- 常用的三种
~~~ makefile
%.o : %.c
    gcc -c $<
~~~

## 为目标
~~~ makefile
# 当 存在clean 文件的时候, make clean同样回执行rm
.PHONY: clean

clean:
    rm *.o
    rm main
~~~
## 条件判断
- ifeq
- ifneq
- ifdef
- ifndef
~~~makefile
ifeq "123", "123"
    echo 'hello world'
else
    echo 'haha'
endif

a = 123
ifdef a
    echo "定义了a"
~~~

## makefile 函数
- makefile 函数是已经定义好的,不支持自定义函数
- 调用方法
    - `$(函数名 参数1,参数2)`
    - `${函数名 参数1,参数2,参数3}`
### 类似 字符串的replace方法 $(subst <from>,<to>,<text>), 字符串替换
- `$(subst a,b,abc)` 得到字符串 bbc
### 规则匹配的替换字符串 $(subst <from>,<to>,<text>)
`$(patsubst %.c,%.o,a.c b.c c.c)` 得到  a.o b.o c.o
### 提取目录所在的目录 $(dir <names…>)
`$(dir </src/a.c>)` 提取目录位置,去除文件名部分的 /src
### 提取目录中的文件名
`$(notdir <names…>)`
### 函数 foreach (每次执行会按顺序返回参数列表值最前面的一个)
`$(foreach <var>, <list>,<text>)`
### 在变量和命令中使用通配符 $(foreach <var>, <list>,<text>)
`$(wildcard *.c)`