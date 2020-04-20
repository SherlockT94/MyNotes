# Vim Pluggins & Shortcut
## Visual mode
* viw -> select a word
* C-v, select head column, I(uppercase), #, ESC -> comment multipul lines
# Plugins
## easymotion
| Shortcut | Explanation                        |
|----------|------------------------------------|
| ,,w      | mark every words                   |
| ,,sh     | search words initial with letter h |
## vim-surround
| Shortcut       | Explanation                                               |
| -------------- | --------------------------------------------------------- |
| cs"'           | change  "" to ''                                          |
| ds"            | delete ""                                                 |
| ys iw "        | only user inner word surround by ""                       |
| 试图模式下 S(  | surround by (  ) with inner space                         |
| 试图模式下 S)  | surround by () without inner space                        |
| yssb           | wrap the entire line in parentheses with "yssb" or "yss)" |
## nerdcommenter
| Shortcut | Explanation                  |
| -------- | ---------------------------- |
| ,ci      | comment/undo comment a line  |
| 3,ci     | comment/undo comment 3 lines |
* or using visual-mode
## nerdtree
| Shortcut | Explanation                                 |
| -------- | ------------------------------------------- |
| ,t       | toggle nerdtree                             |
| t        | open file in new tag and jump to new tag    |
| T        | open file in new tage, but not jump into it |
| gt       | go to next tag                              |
| gT       | go to prev tag                              |

[NERDTree安装和使用](https://blog.51cto.com/chinalx1/2084871)
## Lightline
    " 配置问题:
    " All of settings must in one {} 
    " or using 
    let g:lightline = {}
## ALE
| Shortcut | Explanation    |
|----------|----------------|
| ,s       | Toggle ALE     |
| ,d       | show details   |
| sn       | next error     |
| sp       | previous error |
before using, you have to install linter(语法分析器):
python -> sudo pip install pylint
python -> sudo pip isntall flake8
install fixer:
python -> 
* for pylint work in virtualenv
    * [https://pypi.org/project/pylint-venv/](https://pypi.org/project/pylint-venv/)
## vim-autoformat
| Shortcut  | Explanation |
| --------- | ----------- |
| \<F3>     | format code |

before: sudo pip install yapf(for python3)
[How to use](https://github.com/Chiel92/vim-autoformat#how-to-use)
语法有错误的情况下是无法正确format的
## vim-markdown
: TableFormat -> align table
## markdowm-preview
: MarkdownPreview(support tab auto-completion)
## interestingwords
| Shortcut | Explanation |
| -------- | ----------- |
| ,k       | mark word   |
| ,K       | unmark all  |
## vim-devicons
> brew tap homebrew/cask-fonts
> brew cask install font-hack-nerd-font
[Installation Guide](https://github.com/ryanoasis/vim-devicons/wiki/Installation)
[Usage](https://github.com/ryanoasis/vim-devicons/wiki/usage)
`In Arch Linux:`
> sudo pacman -S <nerd-font>
> fc-list to find font 
> change .Xresources

## Tagbar

Relay on universal-ctags: [Github depository](https://github.com/universal-ctags/ctags)
install on MAC:
> brew install --HEAD universal-ctags/universal-ctags/universal-ctags
> nmap <F8> :TagbarToggle<CR>

## Tabular
| Shortcut | Explanation |
|----------|-------------|
| ,ln      | align '='   |

: Tab /= -> align base on '='
: Tabular /| -> format Markdown table
## Instant-markdown-preview
brew install node
check if node installed:
* node -v
* npm -v

install instant-markdown-d:(sudo) npm -g install instant-markdown-d
edit init.vim: set shell=bash\ -i( If you're on OSX and are using zsh)
## deoplete
After install the plugin: 
> pip3 install --user pynvim

In nvim:
: UpdateRemotePlugins 
## vim-virtualenv
`no need, because pip install pylint-venv will solve the problem!!!`
Deactivate the current virtualenv:
>VirtualEnvDeactivate

List all virtualenvs:(have to export VIRTUAL_ENV=...)
>VirtualEnvList

Activate the 'spam' virtualenv:
>VirtualEnvActivate spam

If you're unsure which one to activate, you could always use tab completion:
>VirtualEnvActivate <tab>