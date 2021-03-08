# Vim
## Install vim-plug
vim-plug is easy to use vim plugin manager. Follow instruction in [github](https://github.com/junegunn/vim-plug) to install vim-plug.  

## Install new plugin
Add gihub plugin link in vimrc and the run PlugInstall  
```
call plug#begin('~/.vim/plugged')

Plug 'fatih/vim-go'
Plug 'Yggdroot/indentLine'
Plug 'mrk21/yaml-vim'

call plug#end()
```

PlugInstall can run in two way inside vim or as a bash command  
```
$ vim test
~
~
:PlugInstall
```

```
vim +PlugInstall +qa
```

## My .vimrc
```
call plug#begin('~/.vim/plugged')

Plug 'fatih/vim-go'
Plug 'Yggdroot/indentLine'
Plug 'mrk21/yaml-vim'

call plug#end()

filetype off
filetype plugin indent on

" Map <leader> to comma
let mapleader=","

" ts (tabstop) a <Tab> key will count as two spaces
" sw (shiftwidth) identation and auto-identation will use two spaces (eg. when using >> or gg=G)
" sts (softtabstop) a <Tab> will count for two spaces when expanding tabs (inserting a tab, or using the Backspace key)
" expandttab use spaces instead of tabs
" foldmethod folding will be based on indentation levels
" nofoldenable the file will be opened without any folds
" nosmd (noshowmode) If in Insert, Replace or Visual mode put a message on the last line. Use the 'M' flag in 'highlight'
"   to set the type of highlighting for this message.
" set autoindent    - turns it on
" set smartindent   - does the right thing (mostly) in programs
" set cindent       - stricter rules for C programs
" set noswapfile

if has("autocmd")
  autocmd Filetype go set ts=2 sw=2 sts=2 noet nolist autowrite number nosmd backspace=indent,eol,start cursorcolumn
  autocmd FileType yaml setlocal ts=2 sts=2 sw=2 expandtab indentkeys-=0# indentkeys-=<:> foldmethod=indent nofoldenable cursorcolumn
endif

set mouse-=a
set clipboard=unnamed
set clipboard=unnamedplus
syntax on
```
