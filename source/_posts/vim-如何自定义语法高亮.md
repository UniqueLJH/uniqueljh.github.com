title: vim 如何自定义语法高亮

date: 2015-10-13 23:52:37

## tags:  Vim
---
用vim写python 发现有一些词没用语法高亮写起来十分不便.

自己用的molokai配色

首先找到vim的安装路径,而非修改当前用户下.vimrc

在vim中

```{bash}
:echo $VIMRUNTIME
```

比如我安装目录是
```
/usr/local/share/vim/vim74
```
进入vim目录之后进入./syntax文件, 然后进入你需要修改的相关语法,例如我需要增加python 的语法高亮
则是编辑
```
./syntax/python.vim
```
例如我想要对logging 加语法高亮
```
syn keyword MyConditional       logging
```

格式为 syn keyword 新语法名 高亮词1 高亮词2 //高亮词通过空格间隔
这里为logging加入新的条件后
编辑配色文件 例如博主使用的molokai配色 则直接修改molokai.vim
该文件通常位于
```
~/.vim/colors/molokai.vim
```
molokai在终端中有多种模式注意以下molokai.vim文件中的语句
```
if &t_Co > 255 //如果终端支持256色 并且在vimrc 中开启了概模式
if exists("g:rehash256") && g:rehash256 == 1
```
则需要在相应的位置加入对新语法条件的配色方案
```
hi MyConditional   ctermfg=24 // 这样就能为logging 加入语法高亮为蓝色
```
