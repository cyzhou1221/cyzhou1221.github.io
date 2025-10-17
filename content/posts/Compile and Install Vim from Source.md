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

官方仓库地址: https://github.com/vim/vim

大概需要 5mins~

0. 安装编译依赖项
   ```
   sudo apt install git make clang libtool-bin -y
   ```
   
1. 下载仓库代码, 更改 `vim/src` 中的 `Makefile`: `--enable-python3interp`, `--disable-gui`
   ```
   cd vim/src
   ```
   
2. ```
   make reconfig -j8
   ```
   
3. 卸载系统中的 Vim
   ```
   sudo apt purge vim-tiny -y
   sudo apt purge vim -y
   sudo apt autoremove -y
   ```
   
4. ```
   sudo make install
   ```

5. 验证安装结果
   ```
   source ~/.bashrc
   vim --version
   ```
   得到输出如下：
   ```
   source ~/.bashrc
   VIM - Vi IMproved 9.1 (2024 Jan 02, compiled Oct 17 2025 19:26:23)
   Included patches: 1-1863
   Compiled by cyzhou@CYZHOU-LEGION
   Huge version without GUI.  Features included (+) or not (-):
   +acl                 +jumplist            -sodium
   +arabic              +keymap              -sound
   +autocmd             +lambda              +spell
   +autochdir           +langmap             +startuptime
   -autoservername      +libcall             +statusline
   -balloon_eval        +linebreak           -sun_workshop
   +balloon_eval_term   +lispindent          +syntax
   -browse              +listcmds            +tabpanel
   ++builtin_terms      +localmap            +tag_binary
   +byte_offset         -lua                 -tag_old_static
   +channel             +menu                -tag_any_white
   +cindent             +mksession           -tcl
   +clientserver        +modify_fname        +termguicolors
   +clipboard           +mouse               +terminal
   +clipboard_provider  -mouseshape          +terminfo
   +cmdline_compl       +mouse_dec           +termresponse
   +cmdline_hist        -mouse_gpm           +textobjects
   +cmdline_info        -mouse_jsbterm       +textprop
   +comments            +mouse_netterm       +timers
   +conceal             +mouse_sgr           +title
   +cryptv              -mouse_sysmouse      -toolbar
   +cscope              +mouse_urxvt         +user_commands
   +cursorbind          +mouse_xterm         +vartabs
   +cursorshape         +multi_byte          +vertsplit
   +dialog_con          +multi_lang          +vim9script
   +diff                -mzscheme            +viminfo
   +digraphs            +netbeans_intg       +virtualedit
   -dnd                 +num64               +visual
   -ebcdic              +packages            +visualextra
   +emacs_tags          +path_extra          +vreplace
   +eval                -perl                -wayland
   +ex_extra            +persistent_undo     -wayland_clipboard
   +extra_search        +popupwin            -wayland_focus_steal
   -farsi               +postscript          +wildignore
   +file_in_path        +printer             +wildmenu
   +find_in_path        +profile             +windows
   +float               -python              +writebackup
   +folding             -python3             -X11
   -footer              +quickfix            +xattr
   +fork()              +reltime             -xfontset
   -gettext             +rightleft           -xim
   -hangul_input        -ruby                -xpm
   +iconv               +scrollbind          -xsmp
   +insert_expand       +signs               -xterm_clipboard
   +ipv6                +smartindent         -xterm_save
   +job                 +socketserver
      system vimrc file: "$VIM/vimrc"
        user vimrc file: "$HOME/.vimrc"
    2nd user vimrc file: "~/.vim/vimrc"
    3rd user vimrc file: "~/.config/vim/vimrc"
         user exrc file: "$HOME/.exrc"
          defaults file: "$VIMRUNTIME/defaults.vim"
     fall-back for $VIM: "/usr/local/share/vim"
   Compilation: gcc -c -I. -Iproto -DHAVE_CONFIG_H -O2 -fno-strength-reduce -Wall -Wno-deprecated-declarations -D_REENTRANT -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=1
   Linking: gcc -L/usr/local/lib -Wl,--as-needed -o vim -lm -ltinfo
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
