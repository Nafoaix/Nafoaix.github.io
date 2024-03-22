---
title: 配置美化bash和vim
tags:
  - 美化
  - bash
  - vim
categories:
  - 开发环境
toc: true
top_img: 'https://s2.loli.net/2022/07/24/CKcblt1UNTEmMyI.png'
cover: 'https://s2.loli.net/2022/07/24/CKcblt1UNTEmMyI.png'
abbrlink: e8e89f2a
date: 2022-04-03 15:12:59
---

# 配置美化bash和vim

bash、vim允许你自定义你的终端，自定义的bash、vim可以实现美化效果，还可以提高你在终端的工作效率，下面我备份的配置文件。

## bash

`.bashrc`

```bash
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
[ -z "$PS1" ] && return

# don't put duplicate lines in the history. See bash(1) for more options
# ... or force ignoredups and ignorespace
HISTCONTROL=ignoredups:ignorespace

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
shopt -s checkwinsize

# make less more friendly for non-text input files, see lesspipe(1)
[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "$debian_chroot" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color) color_prompt=yes;;
esac

# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
	# We have color support; assume it's compliant with Ecma-48
	# (ISO/IEC-6429). (Lack of such support is extremely rare, and such
	# a case would tend to support setf rather than setaf.)
	color_prompt=yes
    else
	color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
#if [ -f /etc/bash_completion ] && ! shopt -oq posix; then
#    . /etc/bash_completion
#fi

# add for proxy
export hostip=$(ip route | grep default | awk '{print $3}')
export hostport=10808
alias proxy='
    export HTTPS_PROXY="socks5://${hostip}:${hostport}";
    export HTTP_PROXY="socks5://${hostip}:${hostport}";
    export ALL_PROXY="socks5://${hostip}:${hostport}";
    git config --global http.proxy "socks5://${hostip}:${hostport}";
    git config --global https.proxy "socks5://${hostip}:${hostport}";
    echo -e "Acquire::http::Proxy \"http://${hostip}:${hostport}\"; " | sudo tee -a /etc/apt/apt.conf.d/proxy.conf;
    echo -e "Acquire::https::Proxy \"http://${hostip}:${hostport}\"; " | sudo tee -a /etc/apt/apt.conf.d/proxy.conf;
    echo "nameserver 8.8.8.8" > /etc/resolv.conf;
'
alias unproxy='
    unset HTTPS_PROXY;
    unset HTTP_PROXY;
    unset ALL_PROXY;
    git config --global --unset  http.proxy;
    git config --global --unset  https.proxy;
    sudo sed -i -e '/Acquire::http::Proxy/d' /etc/apt/apt.conf.d/proxy.conf;
    sudo sed -i -e '/Acquire::https::Proxy/d' /etc/apt/apt.conf.d/proxy.conf;
'

#md folder_name  ==> mkdit folder_name and cd folder_name
md () {
    mkdir -p $1
    cd $1
}

# bash颜色配置
#   颜色代码（字体加背景）：\[\e[F;Bm\]
#   \a ASCII 响铃字符（也可以键入 \007）
#   \e ASCII 转义字符（也可以键入 \033）
#   \d ：#代表日期，格式为weekday month date，例如："Mon Aug 1"   
#   \u ：#当前用户的账号名称   
#   \H ：#完整的主机名称   
#   \h ：#仅取主机的第一个名字  
#   \t ：#显示时间为24小时格式，如：HH：MM：SS   
#   \T ：#显示时间为12小时格式   
#   \A ：#显示时间为24小时格式：HH：MM   
#   \n 换行符
#   \r 回车符
#   \v ：#BASH的版本信息   
#   \w ：#完整的工作目录名称   
#   \W ：#利用basename取得工作目录名称，所以只会列出最后一个目录   
#   \# ：#下达的第几个命令   
#   \$ ：#提示字符，如果是root时，提示符为：# ，普通用户则为：$ 
#   \[\e[m\]：将前景、背景和加粗设置重置为它们的默认值，在在提示行结束时使用这个代码，以使您键入的文字成为非彩色的。
export PS1='\[\e[36m\][\[\e[91m\]\u\[\e[m\]@\[\e[95m\]\h \[\e[34m\]\W\[\e[36m\]]\[\e[32m\]\$\[\e[m\]'
```

## vim

`.vimrc`

```bash
set nocompatible    " 去掉对vi的兼容，让vim运行在完全模式下"
filetype on                     " 开启文件类型检测"
filetype plugin on              " 开启插件的支持"
filetype indent on              " 开启文件类型相应的缩进规则"

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

Plugin 'ycm-core/YouCompleteMe'
" YouCompleteMe:语句补全插件 "
set runtimepath+=~/.vim/bundle/YouCompleteMe
autocmd InsertLeave * if pumvisible() == 0|pclose|endif "离开插入模式后自动关闭预览窗口"
let g:ycm_collect_identifiers_from_tags_files = 1           " 开启 YCM基于标签引擎"
let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释与字符串中的内容也用于补全"
let g:syntastic_ignore_files=[".*\.py$"]
let g:ycm_seed_identifiers_with_syntax = 1                  " 语法关键字补全"
let g:ycm_complete_in_comments = 1
let g:ycm_confirm_extra_conf = 0                            " 关闭加载.ycm_extra_conf.py提示"
let g:ycm_key_list_select_completion = ['<c-n>', '<Down>']  " 映射按键,没有这个会拦截掉tab, 导致其他插件的tab不能用."
let g:ycm_key_list_previous_completion = ['<c-p>', '<Up>']
let g:ycm_complete_in_comments = 1                          " 在注释输入中也能补全"
let g:ycm_complete_in_strings = 1                           " 在字符串输入中也能补全"
let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释和字符串中的文字也会被收入补全"
let g:ycm_global_ycm_extra_conf='~/.ycm_extra_conf.py'      "'~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py'"
let g:ycm_show_diagnostics_ui = 0                           " 禁用语法检查"
inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<CR>"             " 回车即选中当前项"
nnoremap <c-j> :YcmCompleter GoToDefinitionElseDeclaration<CR>     " 跳转到定义处"
let g:ycm_min_num_of_chars_for_completion=2                 " 从第2个键入字符就开始罗列匹配项"
"

Plugin 'VundleVim/Vundle.vim'

Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'
"vim-airline配置:优化vim界面"
let g:airline#extensions#tabline#enabled = 1
" airline设置"
" 显示颜色"
set t_Co=256
set laststatus=2
let g:airline_theme='bubblegum'
" 使用powerline打过补丁的字体"
let g:airline_powerline_fonts = 1
" 开启tabline"
let g:airline#extensions#tabline#enabled = 1
" tabline中当前buffer两端的分隔字符"
let g:airline#extensions#tabline#left_sep = ' '
" tabline中未激活buffer两端的分隔字符"
let g:airline#extensions#tabline#left_alt_sep = ' '
" tabline中buffer显示编号"
let g:airline#extensions#tabline#buffer_nr_show = 1
" 映射切换buffer的键位"
nnoremap [b :bp<CR>
nnoremap ]b :bn<CR>
" 映射<leader>num到num buffer"
map <leader>1 :b 1<CR>
map <leader>2 :b 2<CR>
map <leader>3 :b 3<CR>
map <leader>4 :b 4<CR>
map <leader>5 :b 5<CR>
map <leader>6 :b 6<CR>
map <leader>7 :b 7<CR>
map <leader>8 :b 8<CR>
map <leader>9 :b 9<CR>

" vim-scripts 中的插件 "
Plugin 'taglist.vim'
Plugin 'The-NERD-tree'
Plugin 'indentLine.vim'  "缩进线插件"

" Plugin 'git://git.wincent.com/command-t.git'"


call vundle#end()

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"键盘命令"
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
let mapleader = "\<space>"
nmap <leader>w :w!<cr> "<space>w强制保存文件"
nmap <leader>q :q!<cr> "<space>q强制退出文件"
nmap <leader>s :wq!<cr> "<space>s强制保存退出文件"
nmap <leader>f :find<cr> "<space>find查找"

"Alt+Up/Down向上下移动代码"
nnoremap <A-Down> :m+<CR>==
nnoremap <A-Up> :m-2<CR>==
inoremap <A-Down> <Esc>:m+<CR>==gi
inoremap <A-Up> <Esc>:m-2<CR>==gi
vnoremap <A-Down> :m'>+<CR>gv=gv
vnoremap <A-Up> :m-2<CR>gv=gv

nnoremap <S-Down> :t.<CR>$
nnoremap <S-Up> yykp$
inoremap <A-S-Down> <Esc>:t.<CR>$
inoremap <A-S-Up> <Esc>yykp$

" 映射全选+复制 ctrl+a"
map <C-A> ggVGY
map! <C-A> <Esc>ggVGY

"新建标签"
map <C-n> :tabnew<CR>

" 选中状态下 Ctrl+c 复制" 
vnoremap <C-c> "+y   "支持在Visual模式下，通过C-y复制到系统剪切板
nnoremap <C-p> "*p   "支持在normal模式下，通过C-p粘贴系统剪切板

"NERDTree 配置:F2快捷键显示当前目录树"
map <F2> :NERDTreeToggle<CR>
let NERDTreeWinSize=25 

"ctags 配置:F3快捷键显示程序中的各种tags，包括变量和函数等。"
map <F3> :TlistToggle<CR>
let Tlist_Use_Right_Window=1
let Tlist_Show_One_File=1
let Tlist_Exit_OnlyWindow=1
let Tlist_WinWidt=25

"c，c++ 按f5编译运行"
map <F5> :call CompileRunGcc()<CR>
func! CompileRunGcc()
    exec "w"
    if &filetype == 'c'
        exec "!g++ % -o %<"
        exec "! ./%<"
    elseif &filetype == 'cpp'
        exec "!g++ % -o %<"
        exec "! ./%<"
    elseif &filetype == 'java' 
        exec "!javac %" 
        exec "!java %<"
    elseif &filetype == 'sh'
        :!./%
    endif
endfunc

"F8 c,c++的调试"
map <F8> :call Rungdb()<CR>
func! Rungdb()
    exec "w"
    exec "!g++ % -g -o %<"
    exec "!gdb ./%<"
endfunc 

"F12智能缩进"
map <F12> gg=G

"mf生成main函数"
map mf i#include "stdio.h"<Enter><Enter>int main(int argc, char *argv[])<Esc>o{<Esc>oreturn 0;<Esc>o}<Esc>2ko

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
""实用设置"
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

" encoding
set encoding=utf-8              " 打开文件时编码格式"
set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1         "vim会根据该设置识别文件编码"

set nu      " 显示行号"
set ruler                   " 显示当前光标行号和列"
set history=2000            " 记录 Vim 历史操作的条数"

set autoread                " 文件在vim外修改过自动重新载入"
au CursorHold,CursorHoldI * checktime

set t_Co=256    " 开启256色"

set background=light
colorscheme desert    
" 颜色主题"

syntax on   " 语法高亮"

set cursorline  " 突出显示当前行"

set tabstop=4   " Tab键的宽度"

set smarttab    " 在行和段开始处使用制表符"

"  统一缩进为4"
set softtabstop=4
set shiftwidth=4

set autoindent  " 自动对齐"
set cindent     " 自动缩进"

set showmatch   " 高亮显示对应的括号"

" search"
set smartcase   "搜索时 如果输入大写，则严格按照大小写搜索，如果小写，并设置了ignorecase，则忽略大小写"
set ignorecase  "搜索忽略大小写"
set incsearch   "搜索时及时匹配搜索内容，需要回车确认"
set hlsearch    "高亮搜索项"

set confirm     " 在处理未保存或只读文件的时候，弹出确认"

set noeb        " 去掉输入错误的提示声音"

" 开始折叠 "
set foldcolumn=0
set foldmethod=indent 
set foldlevel=3 
set foldenable              

" 可以在buffer的任何地方使用鼠标（类似office中在工作区双击鼠标定位） "
set mouse=a
set selection=exclusive
set selectmode=mouse,key

filetype plugin indent on
" 分为三部分命令：file on, file plugin on, file indent on.分别表示自动识别文件类型，用文件类型脚本，使用缩进定义文件。"

set completeopt=preview,menu "代码补全 "

" R键编译 Compile function"
noremap r :call CompileRunGcc()<CR>
function! CompileRunGcc()
  execute "w"
  if &filetype == 'c'
    if !isdirectory('build')
      execute "!mkdir build"
    endif
    execute "!gcc % -o build/%<"
    execute "!time ./build/%<"
  endif
endfunction

```
