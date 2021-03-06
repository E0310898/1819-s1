# Vim Tips

I collected below some tips on `vim` that I find helpful.  If you are new to `vim`, please try out the command `vimtutor` on any machine where `vim` is installed, and check out the nice article [Learn vim Progressively]( 
http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/).  

# 1. Useful Configuration

You can configure your `vim` by putting your configuration options and scripts in the `~/.vimrc` file (a hidden file named `.vimrc` in your home directory).  This file will be loaded whenever you starts `vim`.

You can copy a sample `.vimrc` file from `~cs1010/.vimrc` to your home directory. 
You can edit this file `~/.vimrc` just like any other file, using `vim`.

## Help

In `vim,` the command `:help <topic>` shows help about a particular topic in `vim`.  Example, `:help backup`.

## Backup Files

You can ask `vim` to automatically backup files that you edit.  This has been a life saver for me in multiple  occasions.

In your `~/.vimrc` file, 

```
set backup=on
```

will cause a copy of your file to be save with suffix `~` appended to its name everytime you save.

I prefer not to clutter my working directory, so I set

```
set backupdir=~/.backup
```

and create a directory named `~/.backup` to store my backup files.

So if you made changes to a file that you regreted, or if you accidentally deleted a file, you can check under `~/.backup` to see if the backup can save you.

## Syntax Highlighting

If for some reasons, syntax highlighting is not on by default, add this to your `~/.vimrc`:

```
syntax on
```

## Ruler and Numbers

If you prefer to show the line number you are on and the column number you are on, adding the commands to `~/.vimrc`

```
set ruler
```

will display the line number and the column number on the lower right corner.  

You can also add
```
set number
```

to label each line with a line number.

# 2. Navigation

## Basic Navigation

Use `k` and `j` keys to move up and down (just like Gmail and Facebook!).  `h` and `l` to move left and right.

Others shortcut:

- `w`   jump to the beginning of the next word
- `b`   jump to the beginning of the previous word (reverse of `w`)
- `e`   jump to the end of the word (or next word when pressed again)
- `f` + char: search forward in the line and sit on the next matching char
- `t` + char:  search forward in the line and sit on one space before the matching char
- <CTRL-d> jump forward half page
- <CTRL-u> jump backward half page
- `$` jump to end of line
- `0` jump to the beginning of the line
- `%` jump between matching parentheses

## Jumping to a Line

If the compiler tells you there is an error on Line $x$, you can issue `:<x>` to jump to Line $x$.  For instance, `:40` will go to Line 40.

# 3. Editing Operations

## Undo

Since we are on the topic of correcting mistakes, `u` in command mode undo your changes.  Prefix it with a number $n$ to undo $n$ times.  If you want to undo your undo, `<CTRL-R>` will redo.

## Navigation + Editing

`vim` is powerful because you can combine _operations_ with _navigation_.  For instance `c` to change, `d` to delete, `y` to yank (copy).  Since `w` is the navigation command to move over the current word, combining them we get:

- `cw` change the current word (delete the current word and enter insert mode)
- `dw` delete the current word
- `yw` yank the current word (copy word into buffer)

Can you guess what `df)`, `dt)`, `c$`, `y0` do?

If you repeat the operation `c`, `d`, and `y`, it applies to the whole line, so:

- `cc` change the whole line
- `dd` delete the whole line
- `yy` yank the whole line

You can add a number before an operation to specify how many times you want to repeat an operation.  So `5dd` deletes 5 lines, `5dw` deletes 5 words, etc.

See the article [Operator, the True Power of `Vim`](http://whileimautomaton.net/2008/11/vimm3/operator) for more details.

## Swapping Lines

Sometimes you want to swap the order of two lines of code, in command mode, `ddp` will do the trick.  `dd` deletes the current line, `p` paste it after the current line, in effect swapping the order of the two lines.


## Commenting blocks of code

Sometimes we need to comment out a whole block of code in C for testing purposes. There are several ways to do it in `vim`:

- Place the cursor on the first line of the block of code you want to comment.
- `0` to jump to the beginning of the line
- `V` enter visual mode
- Use arrow key to select the block of code you want to comment. 
- `I` to insert at the beginning of the line (here, since we already selected the block, we will insert at the beginning of every selected)
- `//` to insert the Java comment character (you will see it inserted in the current line, but don't worry)
- <ESC> to escape from the visual code.

To uncomment, 

- Place the cursor on the first line of the block of code you want to comment.
- `0` to jump to the beginning of the line
- `<CTRL-v>` enter block visual mode
- Use arrow key to select the columns of text containing `//`
- `x` to delete them

# 4. Other Advanced Features

## Search and Replace in `vim`

```
:%s/oldWord/newWord/gc 
```

`:` enters the command mode.  `%` means apply to the whole document, `s` means substitute, `g` means global (otherwise, only the first occurance of each line is replaced). `c` is optional -- adding it cause `vim` to confirm with you before each replacement  

## Shell Command

If you need to issue a shell command quickly, you don't have to exit `vim`, run the command, and launch `vim` again.  You can use `!`, 

```
:!<command>
```

will issue the command to shell.  E.g.,

```
:!ls
```

You can use this to compile your current file, without exiting `vim`.

```
:!make
```

`make` is actually a builtin command for `vim` so you can also simply run

```
:make
```

## Abbreviation

You can use the command `ab` to abbreviate frequently typed commands.  E.g., in your `~/.vimrc`, 

```
ab pl cs1010_print_long(
```

Now, when you type `pl `, it will be expanded into `cs1010_print_long(`

## Auto-Completion

You can `<CTRL-P>` to auto-complete.  By default, the auto-complete dictionary is based on text in your current editing buffers.  This is a very useful keystroke saver for long function and variable names.

## Auto-Indent the Whole File

You can `gg=G` in command mode to auto-indent the whole file.  `gg` is the command to go to the beginning of the file.  `=` is the command to indent.  `G` is the command to go to the end of the file.

## Splitting `vim`'s Viewport

- `:sp file.c` splits the `vim` window horizontally
- `:vsp file.c` splits the `vim` window vertically
- `Ctrl-w Ctrl-w` moves between the different `vim` viewports

# 6. Plugins

## Syntax and Style Checker

I use `syntastic` to check for style and syntax whenever I save a file.  [`syntastic`](https://github.com/vim-syntastic/syntastic) is a `vim` plugin. 

My `.vimrc` configuration file contains the following:

```
"For syntastic
set laststatus=2
set statusline+=%#warningmsg#
set statusline+=%{SyntasticStatuslineFlag()}
set statusline+=%*

let g:syntastic_error_symbol = '✗'
let g:syntastic_warning_symbol = '⚠'
let g:syntastic_always_populate_loc_list = 1
let g:syntastic_auto_loc_list = 1
let g:syntastic_check_on_open = 1
let g:syntastic_check_on_wq = 0

let g:syntastic_c_checkers = [ 'clang_tidy', 'clang' ]
let g:syntastic_c_compiler = 'clang'
let g:syntastic_c_clang_args = '-Wall -Werror -Wextra -Iinclude'
let g:syntastic_c_clang_tidy_args = '-checks=*'
let g:syntastic_c_compiler_options = '-Wall -Iinclude'
let g:syntastic_c_include_dirs = [ '../include', 'include' ]
let g:syntastic_c_clang_tidy_post_args = ""
```
