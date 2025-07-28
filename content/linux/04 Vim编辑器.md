---
🌻日期🌻: 2025 07 26 11:51:12
🌙星期🌙: 星期六
⌚️时间⌚️: 11:51:12
🌍位置🌍: 北京-昌平区
☁️天气☁️: 🌅中雨 / 🌃雷阵雨
🌡️温度🌡️: 🌅34.0℃/ 🌃26.0℃
tags:
  - linux
publish: true
---
# vim 编辑器 [[Vim_Reference_Sheet.pdf|说明文档]]

Vim 是 vi 的升级版，是文本编辑器，通常用 Vim 来编辑程序或Shell 脚本等。

![[image/Pasted image 20250724160933.png]]

编辑模式的命令

| 命令  |              说明               |
| :-: | :---------------------------: |
|  i  |         在当前光标处进入插入状态          |
|  I  |        光标移动到行首后进入插入状态         |
|  a  |         在当前光标后进入插入状态          |
|  A  |     将光标移动到当前行的行末，并进入插入状态      |
|  o  | 在当前行的下面插入新行，光标移动到新行的行首，进入插入状态 |
|  O  | 在当前行的上面插入新行，光标移动到新行的行首，进入插入状态 |

## 末行命令

|          命令           |                      说明                      |
| :-------------------: | :------------------------------------------: |
|          :w           |                   保存当前编辑。                    |
|       :w 文件路径名        |                   另存为新文件。                    |
|          :wq          |                   保存修改后退出。                   |
|          :q           |               退出vi（如果文档没有修改）。                |
|          :q!          |                 不保存修改，强制退出。                  |
|      :e 其他文件路径名       |                在当前编辑器打开其他文件。                 |
|      :e! 其他文件路径名      |            在当前编辑器打开其他文件，放弃之前的修改。             |
|          :e#          |                  回到上次打开的文件。                  |
|      :r 其他文件路径名       |               读取文件内容到当前vi编辑器中。               |
|         :! 命令         |             执行命令并显示结果。回车键返回到 vi。             |
|        :r! 命令         |             执行命令并将结果添加到当前vi编辑器中。             |
|        :set nu        |                    显示行号。                     |
|       :set nonu       |                   取消显示行号。                    |
|          :数字          |         光标跳转到指定的行，如 *:20* 跳转到第 20 行。         |
| *:sp* 或 *:sp* 其他文件路径名 | 水平分割当前窗口，在命令模式下使用 *Ctrl + w + w* 可以在窗口内切换光标。 |
| *:vs* 或 *:vs* 其他文件路径名 |    垂直分割当前窗口，同上，可以使用 *Ctrl + w + w* 切换光标。     |

命令模式下光标相关的命令：

|            命令            |             说明             |
| :----------------------: | :------------------------: |
|     *h* 或 *左方向键(←)*      |           向左移动光标           |
|     *l* 或 *右方向键(→)*      |           向右移动光标           |
|     *k* 或 *上方向键(↑)*      |           向上移动光标           |
|     *j* 或 *下方向键(↓)*      |           向下移动光标           |
|  *Ctrl + f* 或 *Page Up*  |           向前翻整页            |
| *Ctrl + b* 或 *Page Down* |           向后翻整页            |
|        *Ctrl + u*        |           向前翻半页            |
|        *Ctrl + d*        |           向后翻半页            |
|       *^* 或 *Home*       |         快速定位光标到行首          |
|          *End*           |         快速定位光标到行尾          |
|           *w*            | 将光标快速跳转到当前光标所在位置的后一个单词的首字母 |
|           *b*            | 将光标快速跳转到当前光标所在位置的前一个单词的首字母 |
|           *e*            | 将光标快速跳转到当前光标所在位置的后一个单词的尾字母 |
|       *gg* 或 *1G*        |          跳转到文件的首行          |
|           *G*            |         跳转到文件的末尾行          |

## 命令模式下删除、复制、粘贴相关的命令

| 命令  |                  说明                  |
| :-: | :----------------------------------: |
|  x  |             删除光标处的单个字符。              |
| dd  |               删除光标所在行。               |
| dw  |        删除当前字符到单词尾（包括空格）的所有字符。        |
| de  |     删除当前字符到单词尾（不包括单词尾部的空格）的所有字符。     |
| d$  |           删除当前字符到行尾的所有字符。            |
| d^  |           删除当前字符到行首的所有字符。            |
|  J  |    删除光标所在行行尾的换行符，相当于合并当前行和下一行的内容。    |
| yy  |          复制当前行整行的内容到vi缓冲区。           |
| yw  |        复制当前光标到单词尾字符的内容到vi缓冲区。        |
| y$  |         复制当前光标到行尾的内容到vi缓冲区。          |
| y^  |         复制当前光标到行首的内容到vi缓冲区。          |
|  p  | 读取vi缓冲区中的内容，并粘贴到光标当前的位置（不覆盖文件已有的内容）。 |

> 删除命令删除的内容都会进入*vi缓冲区*，可以使用 *p* 命令进行粘贴。

命令模式下撤销/重做操作

|    命令    |                  说明                   |
| :------: | :-----------------------------------: |
|    u     | 取消最近一次的操作，并恢复操作结果可以多次使用u命令恢复已进行的多步操作。 |
|    U     |            取消对当前行进行的所有操作。             |
| Ctrl + r |           对使用u命令撤销的操作进行恢复。            |

## 字符串替换操作

在命令模式下用 `:` 进入末行模式进行查找。

|       命令       |                                                                       说明                                                                       |
| :------------: | :--------------------------------------------------------------------------------------------------------------------------------------------: |
| :%s/old/new/gc | 在整个文件范围内替换所有的字符串"old"为"new"，提示：*(y/n/a/q/l/^E/^Y)?*，*y*替换当前，*n* 不替换（跳过），*a*替换所有，*q*退出替换。*l* 替换当前匹配后退出，*^E* (Ctrl+E)向下滚动屏幕，*^Y* (Ctrl+Y)向上滚动屏幕。 |

## Vim 的可视化操作

|    命令    |    说明     |
| :------: | :-------: |
|    v     | 任意位置可视化块选 |
|    V     | 以行为单位块选。  |
| Ctrl + v |    块选     |

## Vim 的配置文件

Vim 用户级别的配置文件是 ~/.vimrc

自用配置文件

```
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 0. 头部说明
" 简化版 ~/.vimrc  ——  yzfh
" 支持 Linux / Windows / macOS，终端 / GVim
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 1. 基础选项
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
set nocompatible
filetype on
filetype plugin on
filetype indent on

set encoding=utf-8
set termencoding=utf-8
set fileencodings=ucs-bom,utf-8,cp936,gb18030,gbk,big5,euc-jp,euc-kr,latin1

syntax on
set number " relativenumber
set cursorline
set hlsearch incsearch ignorecase smartcase
set backspace=2
set whichwrap+=<,>,h,l
set clipboard+=unnamed,unnamedplus
set scrolloff=5 sidescrolloff=5
set laststatus=1
set cmdheight=2
set completeopt=preview,menu
set autowrite
set cursorline
set magic
set guioptions-=T
set guioptions-=m
set foldcolumn=0
set foldmethod=indent
set foldlevel=3
set foldenable
set autoindent
set cindent
set tabstop=4
set softtabstop=4
set shiftwidth=4
set expandtab
set smarttab
set enc=utf-8
set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936
set langmenu=zh_CN.UTF-8
set helplang=cn

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 2. 自动补全
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 自动补全括号和引号
inoremap ( ()<Esc>i
inoremap ) <c-r>=ClosePair(')')<CR>
inoremap { {<CR>}<Esc>O
inoremap } <c-r>=ClosePair('}')<CR>
inoremap [ []<Esc>i
inoremap ] <c-r>=ClosePair(']')<CR>
inoremap " ""<Esc>i
inoremap ' ''<Esc>i

function! ClosePair(char)
    if getline('.')[col('.') - 1] == a:char
        return "\<Right>"
    else
        return a:char
    endif
endfunction

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 3. 新文件模板
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 新建文件时自动插入文件头
autocmd BufNewFile *.c,*.cpp,*.sh,*.java,*.py call SetTitle()

function! SetTitle()
    if &filetype == 'sh'
        call setline(1, "#########################################################################")
        call append(line('.'),   "# File Name  : ".expand('%'))
        call append(line('.')+1, "# Author     : yzfh")
        call append(line('.')+2, "# Email      : 2811362054@qq.com")
        call append(line('.')+3, "# Created    : ".strftime('%Y-%m-%d %H:%M:%S'))
        call append(line('.')+4, "#########################################################################")
        call append(line('.')+5, "#!/usr/bin/env bash")
        call append(line('.')+6, "")
    elseif &filetype == 'c'
        call setline(1, "#include <stdio.h>")
        call append(line('.'), "")
        call append(line('.')+1, "int main(int argc, char const *argv[])")
        call append(line('.')+2, "{")
        call append(line('.')+3, "")
        call append(line('.')+4, "    return 0;")
        call append(line('.')+5, "}")
    elseif &filetype == 'cpp'
        call setline(1, "/*************************************************************************")
        call append(line('.'),   " * File Name  : ".expand('%'))
        call append(line('.')+1, " * Author     : yzfh")
        call append(line('.')+2, " * Email      : 2811362054@qq.com")
        call append(line('.')+3, " * Created    : ".strftime('%Y-%m-%d %H:%M:%S'))
        call append(line('.')+4, " ************************************************************************/")
        call append(line('.')+5, "#include <iostream>")
        call append(line('.')+6, "using namespace std;")
        call append(line('.')+7, "")
        call append(line('.')+8, "int main() {")
        call append(line('.')+9, "    return 0;")
        call append(line('.')+10, "}")
    elseif &filetype == 'python'
        call setline(1, "#!/usr/bin/env python3")
        call append(line('.'),   "# -*- coding: utf-8 -*-")
        call append(line('.')+1, '"""')
        call append(line('.')+2, " * File Name  : ".expand('%'))
        call append(line('.')+3, " * Author     : yzfh")
        call append(line('.')+4, " * Email      : 2811362054@qq.com")
        call append(line('.')+5, " * Created    : ".strftime('%Y-%m-%d %H:%M:%S'))
        call append(line('.')+6, '"""')
        call append(line('.')+7, "")
    endif
    normal! G
endfunction

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 4. 编译/运行/调试快捷键
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 编译并运行当前文件
function! CompileRunGcc()
    exec "w"
    if &filetype == 'c'
        exec "!gcc % -std=c11  -o %< && ./%<"
        "-Wall -Wextra   放在-o前面，严格模式
    elseif &filetype == 'cpp'
        exec "!g++ % -std=c++17 -Wall -Wextra -o %< && ./%<"
    elseif &filetype == 'java'
        exec "!javac % && java %<"
    elseif &filetype == 'sh'
        exec "!chmod +x % && ./%"
    elseif &filetype == 'python'
        exec "!python3 %"
    endif
endfunction
nnoremap <F5> :call CompileRunGcc()<CR>

" 调试当前文件
function! Rungdb()
    exec "w"
    if &filetype == 'c'
        exec "!gcc % -std=c11 -g -Wall -Wextra -o %< && gdb ./%<"
    elseif &filetype == 'cpp'
        exec "!g++ % -std=c++17 -g -Wall -Wextra -o %< && gdb ./%<"
    endif
endfunction
nnoremap <F8> :call Rungdb()<CR>

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 5. 其它小优化
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 保存时自动删除行尾空格
autocmd BufWritePre * %s/\s\+$//e

" 快速保存/退出
nnoremap <leader>w :w<CR>
nnoremap <leader>q :q<CR>

" 一键注释
" 安装 nerdcommenter 后：
"  <leader>cc 注释  <leader>cu 取消注释  默认 leader 是 \
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 4. 插件管理器——vim-plug（如已用其它管理器可跳过）
" 终端执行一次：
"   curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
"     https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
" :PlugUpdate 
" 执行这个进行插件安装
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
call plug#begin('~/.vim/plugged')

" 4.1 文件、目录、模糊查找
Plug 'scrooloose/nerdtree'
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'

" 4.2 代码补全、语法检查、美化
" Plug 'neoclide/coc.nvim', {'branch': 'release'}
" Plug 'dense-analysis/ale'
" Plug 'sbdchd/neoformat'

" 4.3 Git 集成
Plug 'tpope/vim-fugitive'
Plug 'airblade/vim-gitgutter'

" 4.4 注释、配对、快速跳转
Plug 'preservim/nerdcommenter'
Plug 'tpope/vim-surround'
Plug 'easymotion/vim-easymotion'

" 4.5 主题
Plug 'morhetz/gruvbox'

call plug#end()

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 5. 插件简单配置
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" NERDTree
nnoremap <C-n> :NERDTreeToggle<CR>
let NERDTreeShowHidden=1

" fzf
nnoremap <C-p> :Files<CR>

" Gruvbox
colorscheme gruvbox
set background=dark

" ALE 实时语法检查
" let g:ale_linters = {
" \   'c': ['gcc'],
" \   'cpp': ['g++'],
" \   'python': ['pylint'],
" \}
" let g:ale_fixers = {
" \   'c': ['clang-format'],
" \   'cpp': ['clang-format'],
" \   'python': ['black'],
" \}
" let g:ale_fix_on_save = 1
```