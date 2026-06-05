
```shell
" Disable all Vim error/beep entirely (Vim8+)
set belloff=all
" Legacy fallback for older Vim builds
set noerrorbells visualbell t_vb=
if has('autocmd')
    autocmd GUIEnter * set visualbell t_vb=
    endif

set expandtab       " Press Tab insert spaces instead of \t tab character
set tabstop=4       " A tab width display = 4 columns
set shiftwidth=4    " auto indent width (>> <<) =4
set softtabstop=4   " <BS> backspace delete 4 spaces like a tab

set number          " show absolute line numbers

set encoding=utf-8
set fileencoding=utf-8
set ignorecase smartcase " case insensitive search unless uppercase typed
set hlsearch        " highlight matched search words
```