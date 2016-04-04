---
layout: post
title:  "How to add clang_complete to your vim"
date:   2016-03-30 17:14:38 +0300
categories: vim programming
---
Clang Complete is an addon to the popular text editor vim. It allows the user to use clang to do a lightweight compile/parse of their code, in order to allow extremely accurate code autocompletion. Alternatives out there (gcc_complete/ctags) either use hackish methods (modified compilers) or do manual parsing of the file. Clang was built with ease of IDE integration built-in, so Clang Complete simply builds on that feature.

Source: [VTLUUG wiki][clang_complete-wiki].

## Setup libclang
If your hadn't install Clang before, you need to install `libclang-dev`:
{% highlight bash %}
sudo apt-get install libclang-3.9-dev
{% endhighlight %}

<b style='color:red'>NOTE:</b> I'd recommend to setup the latest Clang/LLVM from [LLVM Debian/Ubuntu nightly packages][llvm-apt] to have access to the latest issue-free version.

## Setup clang_complete
[clang_complete][clang_complete] uses clang for accurately completing C and C++ code.
Clone the latest [clang_complete][clang_complete-git] into your local directory:
{% highlight bash %}
git clone https://github.com/Rip-Rip/clang_complete.git
{% endhighlight %}

To build and install it in one step, type:
{% highlight bash %}
make install
{% endhighlight %}

To build and install in two steps, type:
{% highlight bash %}
$ make
$ vim clang_complete.vmb -c 'so %' -c 'q'
{% endhighlight %}

Alternatively, you can simply put all files in `~/.vim/`

## Configure your .vimrc
{% highlight bash %}
"clang_autocomplete options
set conceallevel=2
set concealcursor=vin
set completeopt=menuone,menu
" Complete shortcuts
imap <C-Space> <C-X><C-U>
imap <Nul> <C-X><C-U>
let g:clang_use_library=1
let g:clang_library_path='/usr/lib/llvm-3.9/lib'
let g:clang_complete_auto=1
let g:clang_periodic_quickfix=1
let g:clang_snippets=1
let g:clang_conceal_snippets=1
let g:clang_snippets_engine='clang_complete'
" Show clang errors in the quickfix window
"let g:clang_complete_copen = 1
set completeopt=longest,menuone,preview
let g:clang_c_options = '-std=gnu11'
let g:clang_cpp_options = '-std=c++11 -stdlib=libc++'
{% endhighlight %}

## Create .clang_complete config for your project
Here is my config for [JerryScript][jerryscript] project, which is the great JS engine I've been developing for almost two years:

{% highlight bash %}
-I./jerry-core/
-I./jerry-core/ecma
-I./jerry-core/ecma/base
-I./jerry-core/ecma/builtin-objects
-I./jerry-core/ecma/operations
-I./jerry-core/jit
-I./jerry-core/jrt
-I./jerry-core/lit
-I./jerry-core/mem
-I./jerry-core/parser
-I./jerry-core/parser/js
-I./jerry-core/parser/js/bc
-I./jerry-core/parser/js/collections
-I./jerry-core/parser/regexp
-I./jerry-core/vm
{% endhighlight %}

## Congratulations
If you've done all steps correctly it's likely that you will see the same picture on `CTRL+SPACE` hotkey:
![Example of clang_complete](http://egavrin.github.io/images/clang_complete.png)

[jerryscript]:    https://github.com/Samsung/jerryscript
[clang_complete]: https://github.com/Rip-Rip/clang_complete
[clang_complete-git]: https://github.com/Rip-Rip/clang_complete.git
[vim-plus-minus]: https://github.com/egavrin/vim-plus-minus/blob/master/.vimrc
[llvm-apt]: http://llvm.org/apt/
[clang_complete-wiki]: https://vtluug.org/wiki/Clang_Complete
