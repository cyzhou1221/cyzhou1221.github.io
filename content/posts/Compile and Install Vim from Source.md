---
title: Compile and Install Vim from Source
date: 2025-10-17
categories:
---

<!--more-->

## 1. Connecting to GitHub using SSH keys[Optional]

[Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

## 2. Install gcc-12 and g++-12

```
sudo apt install gcc-12 g++12 -y
sudo ln -sf /usr/bin/gcc-12 /usr/bin/gcc
sudo ln -sf /usr/bin/g++-12 /usr/bin/g++
```

输入
```
gcc --version
g++ --version
```
确认版本已更换.


## 3. Compile and install Vim
[参考博客](https://medium.com/@nayan.ruparelia1/vim-with-python-how-to-d687920ce8b2)

官方仓库地址: https://github.com/vim/vim

大概需要 5mins~

0. 安装编译依赖项
   ```
   sudo apt install git make clang libtool-bin -y
   ```
   
1. 下载仓库代码
   ```
   git clone https://github.com/vim/vim.git
   ```

2. 添加 Python3 支持，并取消 GUI 功能
   ```
   sudo apt install libpython3-dev
   ```
   ```
   ./configure --enable-python3interp \
               --with-python3-command=/usr/bin/python3 \
               --with-python3-config-dir=/usr/lib/python3.10/config-* \
               --disable-gui
   ```
   其中 `--with-python3-config-dir` 后的参数通过
   ```
   ls -al /usr/lib/python*
   ```
   进行查找.
   
3. ```
   make -j10
   ```
   
4. ```
   sudo make install
   ```

5. 验证安装结果
   ```
   /usr/local/bin/vim --version
   ```
   得到输出如下：
   ```
   VIM - Vi IMproved 9.1 (2024 Jan 02, compiled Nov 19 2025 08:51:45)
   Included patches: 1-1918
   Compiled by cyzhou@CYZHOU-LEGION
   Huge version without GUI.  Features included (+) or not (-):
   +acl
   +arabic
   +autocmd
   +autochdir
   -autoservername
   -balloon_eval
   +balloon_eval_term
   -browse
   ++builtin_terms
   +byte_offset
   +channel
   +cindent
   +clientserver
   -clipboard
   +cmdline_compl
   +cmdline_hist
   +cmdline_info
   +comments
   +conceal
   +cryptv
   +cscope
   +cursorbind
   +cursorshape
   +dialog_con
   +diff
   +digraphs
   -dnd
   -ebcdic
   +emacs_tags
   +eval
   +ex_extra
   +extra_search
   -farsi
   +file_in_path
   +find_in_path
   +float
   +folding
   -footer
   +fork()
   -gettext
   -hangul_input
   +iconv
   +insert_expand
   +ipv6
   +job
   +jumplist
   +keymap
   +lambda
   +langmap
   +libcall
   +linebreak
   +lispindent
   +listcmds
   +localmap
   -lua
   +menu
   +mksession
   +modify_fname
   +mouse
   -mouseshape
   +mouse_dec
   -mouse_gpm
   -mouse_jsbterm
   +mouse_netterm
   +mouse_sgr
   -mouse_sysmouse
   +mouse_urxvt
   +mouse_xterm
   +multi_byte
   +multi_lang
   -mzscheme
   +netbeans_intg
   +num64
   +packages
   +path_extra
   -perl
   +persistent_undo
   +popupwin
   +postscript
   +printer
   +profile
   -python
   +python3
   +quickfix
   +reltime
   +rightleft
   -ruby
   +scrollbind
   +signs
   +smartindent
   +socketserver
   -sodium
   -sound
   +spell
   +startuptime
   +statusline
   -sun_workshop
   +syntax
   +tabpanel
   +tag_binary
   -tag_old_static
   -tag_any_white
   -tcl
   +termguicolors
   +terminal
   +terminfo
   +termresponse
   +textobjects
   +textprop
   +timers
   +title
   -toolbar
   +user_commands
   +vartabs
   +vertsplit
   +vim9script
   +viminfo
   +virtualedit
   +visual
   +visualextra
   +vreplace
   -wayland
   -wayland_clipboard
   -wayland_focus_steal
   +wildignore
   +wildmenu
   +windows
   +writebackup
   -X11
   +xattr
   -xfontset
   -xim
   -xpm
   -xsmp
   -xterm_clipboard
   -xterm_save
      system vimrc file: "$VIM/vimrc"
        user vimrc file: "$HOME/.vimrc"
    2nd user vimrc file: "~/.vim/vimrc"
    3rd user vimrc file: "~/.config/vim/vimrc"
         user exrc file: "$HOME/.exrc"
          defaults file: "$VIMRUNTIME/defaults.vim"
     fall-back for $VIM: "/usr/local/share/vim"
   Compilation: gcc -c -I. -Iproto -DHAVE_CONFIG_H -g -O2 -D_REENTRANT -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=1
   Linking: gcc -L/usr/local/lib -Wl,--as-needed -o vim -lm -ltinfo -L/usr/lib/python3.10/config-* -lpython3.10
   ```
   表示安装成功.

## 4. Configure
1. Copy the [Configuration](https://github.com/cyzhou1221/dotfiles),
   ```
   cd dotfiles/vim
   cp -r .vim ~/
   cp .vimrc ~/
   ```
   
2. Install Vim-Plug and plugins
   1. Download plug.vim in [Vim-Plug](https://github.com/junegunn/vim-plug), and put it in the "autoload" directory.
      ```
      curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
          https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
      ```
   2. Before doing this, make sure you have configured the proxy correctly so that you can access github.com. If you haven't, you can find my tutorial [here](https://cyzhou1221.github.io/posts/wsl2-%E4%BB%A3%E7%90%86%E8%AE%BE%E7%BD%AE/).
      ```
      vim
      :PlugInstall
      ```

## 5. 收尾
删除下载的 vim 仓库, 节省存储空间.
