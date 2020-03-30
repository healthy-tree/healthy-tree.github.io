```shell script
if empty(glob('~/.vim/autoload/plug.vim'))
  silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
    \ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
  autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif

call plug#begin('~/.vim/plugged')

Plug 'VundleVim/Vundle.vim'

Plug 'daviddavis/vim-colorpack'
Plug 'daylerees/colour-schemes', { 'rtp': 'vim/' }
Plug 'dracula/vim'
Plug 'arcticicestudio/nord-vim'
Plug 'reedes/vim-colors-pencil'

" Plug 'python-mode/python-mode'
" Plug 'python_match.vim'
Plug 'fs111/pydoc.vim'
Plug 'jmcantrell/vim-virtualenv'
Plug 'sheerun/vim-polyglot'
let g:jsx_ext_required = 0
let g:python_highlight_all = 1

Plug 'nvie/vim-flake8'
Plug 'ambv/black'
Plug 'tmhedberg/SimpylFold'
Plug 'fisadev/vim-isort'
let g:vim_isort_python_version = 'python3'
" au bufwritepost *.py silent execute 'Isort'
Plug 'google/yapf', { 'rtp': 'plugins/vim', 'for': 'python' }
au FileType python map <C-Y> :call yapf#YAPF()<cr>
au FileType python imap <C-Y> <c-o>:call yapf#YAPF()<cr>

Plug 'fatih/vim-go'
let g:go_fmt_command = "goimports"

Plug 'othree/yajs.vim'
" autocmd bufwritepost *.js silent execute '! eslint --fix %'
set autoread

Plug 'w0rp/ale'
"{{{
"\   'javascript': ['standard'],
let g:ale_linters = {
\   'javascript': ['prettier'],
\   'css': ['prettier'],
\}
let g:ale_fixers = {'python': ['isort']}
nmap <silent> <C-k> <Plug>(ale_previous_wrap)
nmap <silent> <C-j> <Plug>(ale_next_wrap)
" let g:ale_lint_on_save = 1
" let g:ale_lint_on_text_changed = 0
" let g:ale_lint_on_enter = 0
let g:ale_sign_error = '⤫'
let g:ale_sign_warning = '⚠'
let g:airline#extensions#ale#enabled = 1
autocmd FileType python noremap <buffer> ,f :ALEFix<CR>

let g:ale_fixers = {
            \ 'python': ['add_blank_lines_for_python_control_statements','autopep8','isort','yapf','remove_trailing_lines','trim_whitespace'],
            \}
" }}}

Plug 'maksimr/vim-jsbeautify'
" {{{
" map ,f :call JsBeautify()<cr>
autocmd FileType javascript noremap <buffer> ,f :call JsBeautify()<cr>
autocmd FileType html noremap <buffer> ,f :call HtmlBeautify()<cr>
autocmd FileType css noremap <buffer> ,f :call CSSBeautify()<cr>
autocmd FileType javascript vnoremap <buffer>  ,f :call RangeJsBeautify()<cr>
autocmd FileType html vnoremap <buffer> ,f :call RangeHtmlBeautify()<cr>
autocmd FileType css vnoremap <buffer> ,f :call RangeCSSBeautify()<cr>
" }}}

Plug 'Valloric/YouCompleteMe'
" Plug 'Shougo/deoplete.nvim'
" Plug 'zchee/deoplete-jedi'
" Plug 'davidhalter/jedi-vim'
" Plug 'zxqfl/tabnine-vim'
Plug 'roxma/nvim-yarp'
Plug 'roxma/vim-hug-neovim-rpc'
" {{{
let g:deoplete#enable_at_startup = 1
highlight Pmenu ctermbg=8 guibg=#606060
highlight PmenuSel ctermbg=1 guifg=#dddd00 guibg=#1f82cd
highlight PmenuSbar ctermbg=0 guibg=#d6d6d6
" }}}
Plug 'rking/ag.vim'
Plug 'majutsushi/tagbar'
 " {{{
let g:tagbar_type_markdown = {
  \ 'ctagstype' : 'markdown',
  \ 'kinds' : [ 'h:Heading_L1', 'i:Heading_L2', 'k:Heading_L3' ]
\ }
let g:tagbar_autoshowtag = 1
let g:tagbar_compact = 1
let g:tagbar_width = 26
let g:tagbar_singleclick = 1
let g:tagbar_iconchars = ['⇟', '⤑']
" }}}

Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --all' }
Plug 'kien/ctrlp.vim'
let g:ctrlp_working_path_mode = 'ra' " {{{
let g:ctrlp_dotfiles=0
let g:ctrlp_extensions = ['tag', 'buffertag']
let g:ctrlp_max_height = 25
" }}}

Plug 'scrooloose/nerdcommenter'
" {{{
let g:NERDSpaceDelims = 1
map ,c :call NERDComment(0,"toggle")<CR>
map <C-c> :call NERDComment(0,"toggle")<CR>
" }}}

Plug 'editorconfig/editorconfig-vim'
Plug 'chrisbra/vim-diff-enhanced'
Plug 'tpope/vim-abolish'
Plug 'tpope/vim-surround'
Plug 'tpope/vim-fugitive'
Plug 'tpope/vim-dispatch'
" {{{
au FileType python let b:dispatch = ':!/usr/bin/env python %'
nnoremap <S-e> :Dispatch<CR>
" }}}

Plug 'mhinz/vim-signify'
Plug 'kien/rainbow_parentheses.vim'
Plug 'itchyny/lightline.vim'
" Plugin 'bling/vim-airline'
let g:airline_powerline_fonts = 0 " {{{
let g:airline_left_sep = ''
let g:airline_right_sep = ''
"let g:airline#extensions#ale#enabled = 1
let g:airline_section_error = '%{ALEGetStatusLine()}'
" }}}

Plug 'Yggdroot/indentLine'

Plug 'Yggdroot/indentLine', { 'for': [] }
augroup plug_xtype
  autocmd FileType * if expand('<amatch>') != 'markdown' | call plug#load('indentLine') | execute 'autocmd! plug_xtype' | endif
augroup END

let g:indent_guides_auto_colors = 1 " {{{
let g:indent_guides_start_level = 1
let g:indent_guides_guide_size = 1
let g:indent_guides_enable_on_vim_startup = 1
" }}}

Plug 'gcmt/wildfire.vim'
" {{{
nmap <SPACE> <Plug>(wildfire-fuel)
"let g:wildfire_objects = {
    "\ "*" : ["i'", 'i"', "i)", "i]", "i}"],
    "\ "html,xml" : ["at", "it"],
"\ }
"let g:wildfire_fuel_map = "<SPACE>"
" }}}

Plug 'junegunn/vim-easy-align'
" {{{
vnoremap <silent> <Enter> :EasyAlign<cr>
let g:easy_align_delimiters = {
      \ '>': { 'pattern': '>>\|=>\|>' },
      \ '/': { 'pattern': '//\+\|/\*\|\*/', 'ignore_groups': ['String'] },
      \ '#': { 'pattern': '#\+', 'ignore_groups': ['String'], 'delimiter_align': 'l' },
      \ ']': {
      \     'pattern':       '[[\]]',
      \     'left_margin':   0,
      \     'right_margin':  0,
      \     'stick_to_left': 0
      \   },
      \ ')': {
      \     'pattern':       '[()]',
      \     'left_margin':   0,
      \     'right_margin':  0,
      \     'stick_to_left': 0
      \   },
      \ 'd': {
      \     'pattern': ' \(\S\+\s*[;=]\)\@=',
      \     'left_margin': 0,
      \     'right_margin': 0
      \   }
      \ }
" }}}

Plug 'tmux-plugins/vim-tmux-focus-events'
Plug 'scrooloose/nerdtree', { 'on':  'NERDTreeToggle' }
Plug 'ervandew/supertab'

" for hugo
Plug 'robertbasic/vim-hugo-helper'

call plug#end()
filetype plugin indent on

set ai! nu ignorecase
set clipboard=unnamed
set modelines=1
set textwidth=79
set backspace=indent,eol,start
set mouse=a
colo dracula

vnoremap < <gv
vnoremap > >gv
" up/down with wrapline
map j gj
map k gk
map <C-N> :tabnew<cr>

" vim: set ft=vim fdm=marker:

```
