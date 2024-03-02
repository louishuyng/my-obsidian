---
tags: Vim, Linux
---
## 1. Overview[](https://www.baeldung.com/linux/vim-registers#overview)

[Vim](http://www.vim.org/) is a popular and powerful text editor. It supports many great features that help us efficiently perform different sorts of text editing operations.

In this tutorial, we’ll take a closer look at the Vim registers.

## 2. Introduction to Vim Registers[](https://www.baeldung.com/linux/vim-registers#introduction-to-vim-registers)

### 2.1. What Are Vim Registers?[](https://www.baeldung.com/linux/vim-registers#1-what-are-vim-registers)

As a normal Vim user, we may not have heard about the term “Vim registers”. However, very likely, we have already used registers many times, but we just didn’t realize it.

Let’s take a look at an example of register usages we have already used many times: copy and paste.

We know in Vim we can copy/yank (_y_) some text and paste (_p_) it at other places. After we perform the copy/yank command, the copied text is stored in a register (unnamed register _“”_). Later, when we perform the paste command, the unnamed register’s content will be read and pasted.

**Vim registers are spaces in memory that Vim uses to store some text or operation details. Each of these spaces has an identifier so that it can be accessed later.**

**When we want to reference a Vim register, we use a double quote followed by the register’s name.** For instance, _“a_ means _the Vim register a_ and _“:_ means _the Vim register :_.

In fact, **registers are used everywhere in Vim**. Let’s see some examples of common usages of Vim registers:

- When we remove texts – for example, using the _x_ command – the deleted text is stored in the unnamed register “”
- Once we execute an _Ex_ command (_:Command_) in Vim, the register _“:_ holds the command
- When we search text by _/somePattern_, the search pattern is stored in the _“/_ register

### 2.2. Get the Content of Vim Registers in _Normal_ Mode[](https://www.baeldung.com/linux/vim-registers#2-get-the-content-ofvim-registers-in-normal-mode)

**In Vim _Normal_ mode, we can paste the content of the register _x_ using _“xp_.** If we don’t give the register, the content of the unnamed register _“”_ will be pasted. Let’s see a quick example to understand how to paste the content of a register.

Let’s open Vim editor and type a line of text:

```bash
Vim is a powerful editor.
```

First, in the _Normal_ mode, we move our cursor on the last word “_editor_” and press _yaw_ to yank the word editor into the unnamed register _“”_.

Next, we move the cursor to an empty line and press _p_ without giving a register name. We’ll see the yanked text “_editor_” is pasted.

Then, we execute a command to replace “_powerful_” with “_wonderful_” by typing _:s/powerful/wonderful/[enter]_. After that, the command is stored in the _“:_ register.

Therefore, we can use _“:p_ to paste the content of this register.

Let’s see the usage of the _p_ command through a quick demo:

![p command](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201108_211748.gif)

### 2.3. Get the Content of Vim Registers in _Insert_ Mode[](https://www.baeldung.com/linux/vim-registers#3-get-the-content-ofvim-registers-in-insert-mode)

**If we want to get the content of a register in _Insert_ mode, we can press the “_Ctrl-r”_ followed by the register’s name.**

For example, we want to have the content of _“:_ register in the _Insert_ mode, we press “_Ctrl-r”_ and “_:_“.

Again, let’s see how to do it in a demo:

![ctrl-r](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201108_212740.gif)

### 2.4. Get the Content of Vim Registers in _Command_ Mode[](https://www.baeldung.com/linux/vim-registers#4-get-the-content-ofvim-registers-in-command-mode)

**Further, we can use the _:registers_ (or the short form _:reg_) command to check a Vim register’s value.**

If we don’t provide any register name, the _:reg_ command will list all registers with their content and the type of the content if a register has been written:

```bash
:reg
Type Name Content
  c  ""    editor
  c  "0    editor
  l  "1   ...
  b  "2   ...
  b  "3    ^J ^J ^J
  b  "4   3. ^J7. ^J9. ^J10.
...
  c  ":   s/powerful/wonderful/    
```

In the output above, the _Type_ column means the type of the content. It may contain three different values:

- _c_ – characterwise text
- _l_ – linewise text
- _b_ – blockwise text

It references the three text selection types in Vim. **When we paste the content of a register, line breaks will be controlled differently depending on the content type.**

We can also pass register names to the _:reg_ command and tell it to list certain registers:

```bash
:reg 0 : "                         
Type Name Content
  c  ""    editor
  c  "0    editor
  c  ":   s/powerful/wonderful/
```

### 2.5. Types of Vim Registers[](https://www.baeldung.com/linux/vim-registers#5-types-of-vim-registers)

So far, we’ve seen examples of the unnamed register _“”_ and the register _“:_ holding the last executed command. They are two different register types.

**Vim has ten different types of registers**:

1. The unnamed register “”
2. 26 Named registers _“a_ to _“z_ (or _“A_ to_“Z)_
3. The small delete register _“-_
4. 10 numbered registers _“0_ to _“9_
5. The selection and drop registers _“*_, _“+,_ and _“~_
6. Three read-only registers _“:_, _“.,_ and _“%_
7. The alternate file register _“#_
8. The expression register _“=_
9. Last search pattern register _“/_
10. The black hole register _“__

In this tutorial, we’ll explain all of them in detail.

Moreover, we’ll learn how to use those registers through examples.

## 3. The Unnamed Register _“”_[](https://www.baeldung.com/linux/vim-registers#the-unnamed-register-)

**Vim will fill the unnamed register when we delete text using the commands _d_, _D_, _x_, _X_, _s_, _S_, _c_, _C_, or yank text by _y_ or _Y._**

The unnamed register is used quite often. A common scenario would be a quick copy and paste operation like we use the normal system clipboard.

However, **there is a big difference between the clipboard and the unnamed register. The clipboard won’t save deleted text automatically, but the Vim unnamed register does.** Sometimes, this could be a problem.

An example can help us to understand the problem quickly. Let’s say we have some text in a Vim buffer:

```bash
correct: (right-text)
wrong: (wrong-text)
wrong: (another wrong text)
wrong: (wrong again)
```

What we attempt to do is replace all the “_(….)_“s in lines beginning with “_wrong:_” with “_(right-text)_“.

It looks like an easy copy-paste task. We can just yank “_(right-text)_” by the _Normal_ mode _ya(_ command_,_ then remove each “_(…)_” we want to replace by _da(_ and paste the yanked text using _p._

Next, let’s see if it can be done in this way through a little demo. In the demo, we’ll execute the _:reg ”_ command after yanking and deleting text to monitor the content:

![da(](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201109_225537.gif)

As the demo shows, after we executed _da(_, the content of the unnamed register has been changed into the deleted text “_(wrong-text)_“. Therefore, we’ve lost the yanked value.

We may think it would be good if a register’s content is not updated automatically so that we can always get the yanked text, just like we work with normal clipboards.

Actually, Vim registers can do more than that. It’s time to have a look at the 26 named registers now.

## 4. Named Registers[](https://www.baeldung.com/linux/vim-registers#named-registers)

**Vim has 26 named registers _“a/A_ to _“z/Z_. Vim fills or updates the content of these registers only when we tell it to do so.** That is to say, we have 26 clipboards in Vim!

### 4.1. The 26 Clipboards[](https://www.baeldung.com/linux/vim-registers#1-the-26-clipboards)

Now, we can solve the problem in the previous section easily with a named register. What we can do is, when we yank the text, we press _“aya(_ to save the text in register _“a._ Similarly, when we want to paste after the deletion, we tell Vim to paste the content of the _“a_ register using _“ap_.

Let’s see how it gets done:

![20201110_212300](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201110_212300.gif)

Moreover, we can yank different texts in different named registers and paste from the register we want:

![20201110_213717](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201110_213717.gif)

As the demo above shows, it’s pretty convenient, just like we work with multiple clipboards. We don’t have to back and forth to copy different texts.

### 4.2. “a – “Z vs “a – “Z[](https://www.baeldung.com/linux/vim-registers#2-a--z-vs-a--z)

We can reference a named register using a lowercase or uppercase letter. For example, _“a_ and _“A_ refer to the same register. Therefore, we have 26 named registers instead of 52.

However, **even though a lowercase letter and its corresponding uppercase letter refer to the same register, they behave differently when used**. When we write text to a named register using a lowercase register name, Vim will overwrite the existing content in that register. However, if we use an uppercase register name, Vim will append the new content to the given register instead.

Let’s see the difference between using lowercase and uppercase registers through a demo:

![20201110_221435](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201110_221435.gif)

As we can see in the demo, if we yank texts with the uppercase register, for example, _“Byiw_, the word is appended to _“b_.

Named registers are pretty handy. However, we should keep in mind that **even if we use a named register to hold the deleted or yanked text – for example,_“adiw_ or _“ayiw –_ the unnamed register will be filled, too.** That is, the unnamed register will have the same content as the named register.

## 5. The Small Delete Register _“-_[](https://www.baeldung.com/linux/vim-registers#the-small-delete-register--)

We’ve learned that the unnamed register will be filled by deleted and yanked text automatically, no matter if we specify a named register or not.

Vim has another register that is similar to the unnamed register: small delete register _“-._ As the name implies, **the small delete register will save “small deletions” automatically. If the deleted text is less than one line, Vim considers it as a small delete.**

Unlike the unnamed register, **the _“-_ register won’t be filled by yanked text**.

Further, **if we specify a named register to the delete command, the deleted text won’t be saved in the _“-_ register** either, even if it is a small delete.

As usual, let’s see a demo to understand the small delete register quickly:

![20201112_001126](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201112_001126.gif)

As the demo shows, the _“-_ register is filled by the deleted text only if we delete less than one line and do so without specifying a named register. However, the unnamed register _“”_ is always filled.

## 6. Numbered Registers[](https://www.baeldung.com/linux/vim-registers#numbered-registers)

Vim also provides ten numbered registers _“0_ – _“9_, and it fills these numbered registers from yank and delete commands.

**The most recently yanked text will fill the “0 register, unless we specify another register in the yank command**, for example: _“ayaw_.

**However, the numbered register _“1_ contains the text deleted by the most recent delete or change command.**

The _“1_ register gets filled automatically only when two requirements are met:

- The delete command must not specify another register. For example, the word deleted by the command_“adaw_ doesn’t go to the _“1_ register
- The changed or deleted text is not less than one line (the “- register will be used otherwise)

So far, we’ve talked about _“0_ and _“1_, and we still have _“2_ – _“9_. These eight registers maintain a deletion history. **With each successful change or deletion, Vim shifts the previous content** **of register _“1_ into register “2, “2 into “3, and so forth, and the previous content of register _“9_ will be discarded.**

As always, let’s see how the numbered registers are filled through a demo:

![20201112_233326](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201112_233326.gif)

## 7. The Selection and Drop Registers[](https://www.baeldung.com/linux/vim-registers#the-selection-and-drop-registers)

### 7.1. The Selection Registers: _“*_ and _“+_[](https://www.baeldung.com/linux/vim-registers#1-the-selection-registers--and-)

So far, we’ve discussed quite a few types of Vim registers. We know that we can copy and paste text using Vim registers. Moreover, Vim registers provide the possibility of using multiple clipboards. It’s pretty handy when we edit text.

However, there’s still one big difference between the Vim registers we’ve learned so far and the regular system clipboard: using those Vim registers, we can only copy and paste within Vim.

Sometimes, we do want Vim to talk to the system clipboard. For example, we may want to copy text from a web page and paste it into Vim to edit, or we may need to copy some text in Vim and paste it into another application.

It’s also a pretty basic feature of a text editor. As a powerful editor, Vim can certainly talk to the system clipboard as well. The selection registers are the bridge to connect Vim and the system clipboard. Vim has two selection registers: _“*_ and _“+_.

### 7.2. Operating Systems and Selection Registers[](https://www.baeldung.com/linux/vim-registers#2-operating-systems-and-selection-registers)

**If we’re running Vim on a Windows system, there is no difference between “* and “+ registers. Both registers will communicate to the system clipboard.**

For example, _Normal_ mode command _“+yaw_ (or _“*yaw_) will copy the word under the cursor to the system clipboard, while _“+p_ (or _“*p_) will paste the value from the system clipboard to Vim.

However, when our Vim runs on a *nix system with X11, _“+_ and _“*_ are different.

When we use the _“+_ register, Vim will still communicate to the system clipboard, similar to how it works under the Windows system. However, the _“*_ register will talk to the [X Window primary selection,](https://en.wikipedia.org/wiki/X_Window_selection) which is usually stored by mouse selection and read by mouse middle click.

Next, let’s see a demo to understand how the Vim selection registers read and write system clipboard and X Window selection:

![20201115_001251](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201115_001251.gif)

Finally, **we should keep in mind that Vim must be compiled with a _+clipboard_ feature to support system clipboard access. Also, in order to use _“*_ to talk to the X Windows selection, the _+x11-selection_ feature should be enabled.**

Vim has enabled the mentioned features above in most modern distributions’ package repositories. Therefore, very likely, our Vim has those features enabled as well.

After all, **we can execute the _:version_ command in Vim to check if the required features are enabled in the current Vim.** 

### 7.3. The Drop Register _“~_[](https://www.baeldung.com/linux/vim-registers#3-the-drop-register-)

**The drop register _“~_ stores the dropped text from the last drag and drop operation. This register is only available with _GVim_.**

A short demo will explain this register straightforwardly:

![20201115_232009](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201115_232009.gif)

**To use the drop register, the GVim must be compiled with the _+dnd_ feature.**

## 8. Three Read-Only Registers _“:_, _“.,_ and _“%_[](https://www.baeldung.com/linux/vim-registers#three-read-only-registers---and-)

Actually, the _“:_ is not new for us. It contains the most recently executed command. We’ve seen a demo to paste the last executed command in a previous demo.

**The _“%_ register contains the name of the current file, while the _“._ register holds the last inserted text.**

Let’s show the usage of these three registers through a demo:

![0201115_233549](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201115_233549.gif)

**_“:_, _“.,_ and _“%_ are read-only Vim registers.** Their contents are set by Vim automatically. We can only read their values.

## 9. The Alternate File Register _“#_[](https://www.baeldung.com/linux/vim-registers#the-alternate-file-register-)

First, we need to understand what is an alternate file in Vim before we talk about the alternate file register.

We can open multiple files in Vim. Each file is opened in a Vim buffer. The _:ls_ command will list all the buffers in the current window.

Let’s say we open two files, _file1_ and _file2_, in Vim and start editing _file1_. Later, we want to make some changes in _file2_, so we switch to the _file2_ buffer.

At this moment, the current file is _file2_, while the last edited file in the same Vim window is _file1_.

**We call the last edited file or buffer in the same Vim window the alternate file or buffer.** Therefore, in this example, _file1_ is the alternate file.

If we switch from _file2_ back to _file1_, then _file1_ becomes the current file again and _file2_ becomes the alternate file.

The _“#_ register stores the alternate file.

As always, let’s understand Vim alternate file and the _“#_ register through a demo.

In the demo, we’ll open two files in a single Vim window and switch between them, then list the content of _“%_ and _“#_ registers:

![20201116_000428](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201116_000428.gif)

In the demo, we switched between buffers using the command _:buf [buferNo]_. **If we want to switch between the current file and the alternate file quickly in _Normal_ mode, we can press _Ctrl-^._**

## 10. The Expression Register _“=_[](https://www.baeldung.com/linux/vim-registers#10-the-expression-register-)

We’ve seen different types of Vim registers. Usually, a register stores some text. However, the next register we’re going to look at is a special one. It’s the expression register _“=_.

**“= stores a Vim expression. Vim can compute the expression and return us the result.** It’s pretty handy when we want to evaluate some expression while editing.

The common usages of _“=_ are:

- In _Normal_ mode, we can press _“=AnExpression_ to set the register and use the command _:put =_ to print the result
- In _Insert_ mode, we can press _Ctrl-R=AnExpression_, and after we press enter, the result of _AnExpression_ will be inserted at the cursor position

Let’s see these usages in action:

![20201117_004413](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201117_004413.gif)

## 11. Last Search Pattern Register _“/_[](https://www.baeldung.com/linux/vim-registers#11-last-search-pattern-register-)

In Vim, we can search for text in various ways. For example, we can use _/pattern_ command to apply a forward search, _?pattern_ to do a backward search, or press * and # to search the current word forward and backward.

**Vim stores the most recent search pattern in the search pattern register _“/_. When we press _n_ or _N_ to navigate to the next or previous occurrence, the content of _“/_ will be used**:

![20201116_234913](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201116_234913.gif)

**_“/_ is a not read-only register. However, we cannot yank or delete text and save it into the “/ register.** It is set automatically when we apply a search. If we want to change its content manually, as shown in the demo, we can use the command _:let @/=’newPattern’_.

## 12. The Black Hole Register _“__[](https://www.baeldung.com/linux/vim-registers#12-the-black-hole-register-)

So far, we’ve discussed most types of Vim registers. We’ve learned that when we change, delete, or yank text, some registers will be automatically updated — for example, the unnamed register _“”_. However, sometimes, we want to change or delete text without changing any register, including _“”_.

The black hole register _“__ is the key to achieve that. As its name implies, **when writing text to the black hole register, nothing happens. If we read the content of this register, nothing is returned.**

Therefore, we can specify this register in the delete or change command to ask the “black hole” to swallow the changed or deleted text. For example, to remove the current word with the black hole register, we can use _“_daw_.

Let’s see a demo to understand the difference between deletion with and without the black hole register:

![20201116_232941](https://www.baeldung.com/wp-content/uploads/sites/2/2020/11/20201116_232941.gif)  
As the demo shows, **_“__ is a good choice when we want to change or delete text without interfering with normal registers**.

## 13. Conclusion[](https://www.baeldung.com/linux/vim-registers#13-conclusion)

In this article, we’ve learned what Vim registers are and how to use them.

Further, we’ve discussed each type of Vim register and demonstrated them through examples.

When we edit files in Vim, the proper usage of Vim registers will boost our productivity.

Comments are closed on this article!