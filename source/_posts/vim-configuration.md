---
title: vim configuration
date: 2018-04-12 11:29:53
categories:
  - coding
tags: vim
---

1. Step 1: [install vundle](https://github.com/VundleVim/Vundle.vim#quick-start)
2. Step 2: install plugins
```bash
cat > ~/.vimrc.bundles << EOF
if &compatible
  set nocompatible
end

filetype off
set rtp+=~/.vim/bundle/vundle/
call vundle#rc()

" Let Vundle manage Vundle
Bundle 'gmarik/vundle'

" Define bundles via Github repos
Bundle 'christoomey/vim-run-interactive'
Bundle 'Valloric/YouCompleteMe'
Bundle 'croaky/vim-colors-github'
Bundle 'danro/rename.vim'
Bundle 'majutsushi/tagbar'
Bundle 'kchmck/vim-coffee-script'
Bundle 'kien/ctrlp.vim'
Bundle 'pbrisbin/vim-mkdir'
Bundle 'vim-syntastic/syntastic'
Bundle 'slim-template/vim-slim'
Bundle 'thoughtbot/vim-rspec'
Bundle 'tpope/vim-bundler'
Bundle 'tpope/vim-endwise'
Bundle 'tpope/vim-fugitive'
Bundle 'tpope/vim-rails'
Bundle 'tpope/vim-surround'
Bundle 'vim-ruby/vim-ruby'
Bundle 'vim-scripts/ctags.vim'
Bundle 'vim-scripts/matchit.zip'
Bundle 'vim-scripts/tComment'
Bundle "mattn/emmet-vim"
Bundle "scrooloose/nerdtree"
Bundle "Lokaltog/vim-powerline"
Bundle "godlygeek/tabular"
Bundle "msanders/snipmate.vim"
Bundle "jelera/vim-javascript-syntax"
Bundle "altercation/vim-colors-solarized"
Bundle "othree/html5.vim"
Bundle "xsbeats/vim-blade"
Bundle "Raimondi/delimitMate"
Bundle "groenewege/vim-less"
Bundle "evanmiller/nginx-vim-syntax"
Bundle "Lokaltog/vim-easymotion"
Bundle "tomasr/molokai"
Bundle "klen/python-mode"
" Bundle "artur-shaik/vim-javacomplete2"
Bundle "Yggdroot/indentLine"
Bundle "flazz/vim-colorschemes"

" markdown plugin
" Plugin 'godlygeek/tabular'
Plugin 'plasticboy/vim-markdown'

if filereadable(expand("~/.vimrc.bundles.local"))
  source ~/.vimrc.bundles.local
endif

filetype on
EOF
vim +PluginInstall +qall

```
3. Step 3: configure vim
```bash
cat > ~/.vimrc << EOF
syntax enable
set background=dark
colorscheme solarized
set nu
set foldenable
set foldmethod=manual

if filereadable(expand("~/.vimrc.bundles"))
  source ~/.vimrc.bundles
endif

" Nerd Tree
let NERDChristmasTree=0
let NERDTreeWinSize=40
let NERDTreeChDirMode=2
let NERDTreeIgnore=['\~$', '\.pyc$', '\.swp$']
let NERDTreeShowBookmarks=1
let NERDTreeWinPos="left"
autocmd vimenter * if !argc() | NERDTree | endif " Automatically open a NERDTree if no files where specified
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTreeType") && b:NERDTreeType == "primary") | q | endif "Close vim if the only window left open is a NERDTree
map <F5> :NERDTreeToggle<cr>
map <F7> :TagbarToggle<CR>
" set encoding=utf-8
" set fileencodings=utf-8,gb2312,gb18030,gbk,ucs-bom,cp936,latin1
set tabstop=4
set softtabstop=4
set expandtab
set showcmd
" set cursorline  "highlight current line
set wildmenu
set showmatch
set incsearch   " search as characters are entered "
set hlsearch    " highlight matches "

" ctags
map <F6> :!find . -type f -name "*.h" -o -name "*.cpp" -o -name "*.m" -o -name "*.mm" -o -name "*.java" -o -name "*.py" > src.files && ctags -R -L src.files && rm -f src.files  <cr>
let Tlist_Ctags_Cmd='/usr/local/ctags-5.8/bin/ctags'
let Tlist_Show_One_File=1
let Tlist_Exit_OnlyWindow=1
let Tlist_Use_Right_Window=1

" tabpage
function! Zoom ()
    " check if is the zoomed state (tabnumber > 1 && window == 1)
    if tabpagenr('$') > 1 && tabpagewinnr(tabpagenr(), '$') == 1
        let l:cur_winview = winsaveview()
        let l:cur_bufname = bufname('')
        tabclose

        " restore the view
        if l:cur_bufname == bufname('')
            call winrestview(cur_winview)
        endif
    else
        tab split
    endif
endfunction

let mapleader = "\<Space>"
nmap <leader>z :call Zoom()<CR>'

autocmd bufnewfile *.java so ~/LICENSE 


" java autocomplete
" setlocal omnifunc=javacomplete#Complete
" if has("autocmd")
"   autocmd FileType java setlocal omnifunc=javacomplete#Complete
"   autocmd Filetype java setlocal completefunc=javacomplete#CompleteParamsInf
"   autocmd FileType java inoremap <buffer> <C-X><C-U> <C-X><C-U><C-P>
"   autocmd FileType java inoremap <buffer> <C-S-Space> <C-X><C-U><C-P>
" endif
" nmap <leader>jI <Plug>(JavaComplete-Imports-AddMissing)
" nmap <leader>jR <Plug>(JavaComplete-Imports-RemoveUnused)
" nmap <leader>ji <Plug>(JavaComplete-Imports-AddSmart)
" nmap <leader>jii <Plug>(JavaComplete-Imports-Add)
 
" imap <C-j>I <Plug>(JavaComplete-Imports-AddMissing)
" imap <C-j>R <Plug>(JavaComplete-Imports-RemoveUnused)
" imap <C-j>i <Plug>(JavaComplete-Imports-AddSmart)
" imap <C-j>ii <Plug>(JavaComplete-Imports-Add)

" nmap <leader>jM <Plug>(JavaComplete-Generate-AbstractMethods)
" 
" imap <C-j>jM <Plug>(JavaComplete-Generate-AbstractMethods)
" 
nmap <leader>jA <Plug>(JavaComplete-Generate-Accessors)
nmap <leader>js <Plug>(JavaComplete-Generate-AccessorSetter)
nmap <leader>jg <Plug>(JavaComplete-Generate-AccessorGetter)
nmap <leader>ja <Plug>(JavaComplete-Generate-AccessorSetterGetter)
nmap <leader>jts <Plug>(JavaComplete-Generate-ToString)
nmap <leader>jeq <Plug>(JavaComplete-Generate-EqualsAndHashCode)
nmap <leader>jc <Plug>(JavaComplete-Generate-Constructor)
nmap <leader>jcc <Plug>(JavaComplete-Generate-DefaultConstructor)

" imap <C-j>s <Plug>(JavaComplete-Generate-AccessorSetter)
" imap <C-j>g <Plug>(JavaComplete-Generate-AccessorGetter)
" imap <C-j>a <Plug>(JavaComplete-Generate-AccessorSetterGetter)
" 
" vmap <leader>js <Plug>(JavaComplete-Generate-AccessorSetter)
" vmap <leader>jg <Plug>(JavaComplete-Generate-AccessorGetter)
" vmap <leader>ja <Plug>(JavaComplete-Generate-AccessorSetterGetter)

" markdown config
let g:vim_markdown_folding_disabled = 1

" Customization of indentLine
let g:indentLine_color_tty_light = 7 " (default: 4)
let g:indentLine_color_dark = 1 " (default: 2)
" let g:indentLine_bgcolor_term = 202
" let g:indentLine_bgcolor_gui = '#FF5F00'

" Customization of colorschemes
colorscheme molokai

" Customization of syntastic
" set statusline+=%#warningmsg#
" set statusline+=%{SyntasticStatuslineFlag()}
" set statusline+=%*
" let g:syntastic_always_populate_loc_list = 1
" let g:syntastic_auto_loc_list = 1
" let g:syntastic_check_on_open = 1
" let g:syntastic_check_on_wq = 0
EOF
```
