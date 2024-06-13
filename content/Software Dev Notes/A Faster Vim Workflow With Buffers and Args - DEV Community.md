---
dg-publish: true
created: 2023-09-09T23:59:10 (UTC -04:00)
source: https://dev.to/iggredible/a-faster-vim-workflow-with-buffers-and-args-51kf
author: Igor Irianto
---

> [!summary] 
> A Faster Vim Workflow With Buffers and Args 

---
When I formally switched to Vim full time (having used Atom, IntelliJ, and VSCode), I started a quest to find the best Vim editing workflow.

## Buffers


After months of using Vim, I was drawn to the buffer workflow. In this article, I will share how that workflow works.

## Vim Buffers

But first, what is a Vim buffer?

A buffer is Vim's unique abstraction not found in other editors / IDEs (that I know of). A buffer is a temporary space in the memory to store your opened file(s). Each time you open a file, Vim stores it in a buffer; if you open 5 files, you have 5 buffers. A buffer remains "opened" even if that file is not visible (the buffer is hidden). This is important! Buffers won't go away until you either quit Vim or explicitly command it to close (like `:bdelete`).

When I first learned about buffers, it was hard for me to visualize it. After all, I never used buffer in editing before. The editors / IDEs that I've used all have tabs and windows, but not buffers. A good (albeit imperfect) analogy that helped me to visualize Vim buffers is to think of Vim's buffer list as a stack of playing cards. Each card represents the files you have opened so far. All the cards are stacked face-up, but because they are stacked, only the top-most card is visible. The top card is the file that you are currently seeing, `file1.txt`. For simplicity, in this analogy, we are only dealing with one tab and window (no split windows either). There are N numbers of cards in the stack just as there are N opened files. If you opened 10 files, then you have 10 buffers; you have 10 cards in the stack. If you need to view the next card, `file2.txt` card, you swap the topmost card with the second card from the top. Now your second-card-from-the-top is visible, sitting on the top of the stack. Your previous topmost card (the `file1.txt` card) is now at the second-from-the-top, no longer displayed. If you need to view the next card, `file3.txt`, you just need to find that card from the deck and put it on the top of the stack (after tucking the `file2.txt` card away back into the stack). That's what it means to switch buffers.

If you want to learn more about buffers, windows, and tabs, check out this chapter from my Vim book: [Ch02 - Buffers, Windows, and Tabs](https://github.com/iggredible/Learn-Vim/blob/master/ch02_buffers_windows_tabs.md) (it's free!).

## Buffer Workflow

Let's learn how to use the Vim buffer workflow.

First, you need to have this in your vimrc:  

Without the `set hidden`, each time you switch to a different buffer without saving the current changes, Vim stops you and (kindly) asks you to either discard your changes or save them. Common courtesy, I guess. Although it is a nice feature to be prompted to save your file before leaving it, not being able to _quickly_ switch buffers is the antithesis of the buffer workflow. Plus your files are not really closed when you switch to a different buffer - they are still there, just hidden.

Generally, you shouldn't worry about losing your changes when switching to different buffers. Even when you exit Vim (`:qall`) and forget to save your changes, Vim will still remind you to save your changes so you get another chance to save your files (just don't run `:qall!`, but in general I almost never force quit Vim). Your muscle memory should save changes (`:w`) all the time. Since the pros (quick buffer switch) outweighs the cons (losing unsaved files), I suggest having the `set hidden` in your vimrc.

### Flying

There are two major ways you can switch buffers quickly.

Let's talk about the quickest buffer-switching method here. But first, to display all buffers, you can use one of the two commands below:  

or  

To switch to a buffer, you can run:  

Where `n` is the buffer number.

I find it challenging to try and remember all the buffer numbers. The `:buffers` command makes a nice combo with the `:buffer n` command. Try running these two sequentially:  

You can also pass to the `buffer` command the buffer filename too!  

```
:buffers
:buffer file1.rb
```

Very sweet! With this, you can jump to any buffer files quickly and consistently. This command sequence is a bread-and-butter to my daily editing workflow.

You can create a keymap for this operation. In your vimrc:  

```
nnoremap &lt;Leader&gt;b :buffers&lt;CR&gt;:buffer&lt;Space&gt;
```

With this, when you press `<Leader>b`, Vim displays all buffer information (including their buffer numbers and filenames) and you can select which buffer to jump into.

One of my favorite Vim plugins, [fzf.vim](https://github.com/junegunn/fzf.vim), has a buffer command `:Buffers` that uses fzf and Vim's popup window to fuzzy search your buffer list. I personally use this over the `:buffers` command because I find the pop-up window easier to see.

In my vimrc, I map the `:Buffers` command to `Ctrl-b`:  

```
nnoremap &lt;silent&gt; &lt;C-b&gt; :Buffers&lt;CR&gt;
```

If you are a purist, stick with `:buffers`. If you are fzf's fan, use `:Buffers`. Principally, they do the same thing: displaying a list of buffers and quickly jumping to a buffer. I am pretty sure that there are alternatives out there to switch buffers that I am not aware of. Don't take my word - shop around!

This is the crux of the buffer workflow:

-   You open N files; these are the files relevant to your current task.
-   With these files opened, you can jump to any of them thanks to the buffer operation. I call this _flying_ because you can reach any of these files quickly - _with the same number of keystrokes_.

Let's say that you have 9 opened files in the buffers: `file1.rb`, `file2.rb`, ... `file9.rb`. You can reach any file with the same number of keystrokes. If you have the `<Leader>b` keymap, you can reach any of the 9 files with 4 keystrokes: `<Leader>` + `b` + (buffer number) + `Return`. Hey, a constant number of keystrokes for an indefinite number of opened files? It's a win!!

Prior to the buffer workflow, I used to do a codebase-wide search to switch files. For example, if I need to go to the `get_pdf` method, I would go to my editor's fuzzy search textbox and type `g-e-t-p-d-f` in-file search. That is a lot of typing! Moreover, what if the method name is long? I'd have to type nearly a dozen letters before getting there. No thanks. With buffer workflow, once you found all your files, you only need to type a constant number of keystrokes to go to any file.

You may argue (at least I used to) that you can just open all the relevant files in split windows or store them in your editor's tabs (if your editor supports multiple tabs); some editors also have "recently opened files" section. Having multiple split windows will quickly crowd your screen real estate (imagine having 9+ split windows? You won't be able to see a thing!). Having multiple tabs sounded like an acceptable strategy - until you realize that if you have multiple tabs (9+), it would take a long time to go to your target tab (you need to pause, browse through the list of tabs you have opened for that one file you want to open).

With Vim buffers, all your files are reachable, searchable, and you get the whole screen in front of you so you can focus on the most important task: coding.

A programmer should spend most of his/her time programming, not searching for files.

### Sprinting

In addition to the `:buffers` and `:buffer` commands, here are more useful buffer commands:  

```
:bnext
:bprev
:bfirst
:blast
```

`:bnext` and `:bprev` navigate to the next and previous buffer on the list, while `:bfirst` and `:blast` navigate to the first and last buffer on the list.

Another one of my favorite Vim plugins, [unimpaired.vim](https://github.com/tpope/vim-unimpaired) offers intuitive shortcuts to traverse different file collections, like buffers.

With it, I can traverse the buffer list with `]b` (`:bnext`), `[b` (`:bprev`), `]B` (`:blast`), and `[B` (`:bfirst`). This allows me to traverse my buffer list very quickly. I call it _sprinting_ because I can type `]b ]b ]b ]b` rapidly and go across my buffer list at an olympian speed.

### My Typical Workflow

So what does a typical buffer workflow look like?

By the time I open Vim, I usually already know what task I need to do. Let's say that I need to fix a particular UI bug. By then, I have a gut feeling what / where the relevant method is. I would then search with either `:grep`, `:find`, `:vimgrep`, or plugins like `fzf.vim`. Suppose my starting line is `ui.js`. From there, I follow the logic. Eventually, I have `ui.jsx`, `uis_controller.rb`, and `uis_controller_spec.rb` opened also in my buffer list.

My buffers now contain 4 files: `ui.js`, `ui.jsx`, `uis_controller.rb`, and `uis_controller_spec.rb`.

From here, I can easily travel to any of these 4 files using a combination of "flying" (`:Buffers` + select one) and "sprinting" (`[b ]b [B ]B`).

Whenever I add a new file to my workflow task, that file is automatically added to the buffer list (remember, Vim automatically adds an opened file into the buffer list), so I can immediately fly / sprint to it.

Why walk if you can sprint and fly?

## Bufdo

As an added bonus, you can also run an operation across all buffers with `:bufdo`. You can pass an operation and apply it to all the buffer files.

So if you want to substitute all "pancake" with "waffle" across all buffers, run:  

```
:bufdo %s/pancake/waffle/g | update
```

`s/pancake/waffle` is the substitute command that replaces pancakes with waffles. `update` saves each buffer file after substitution.

## Argument List

An alternative to the buffer workflow is the argument list (args/arglist/argslist) workflow. Before I go over what an args list is, I'm going to explain why you might want to also learn how to use `:args`.

## The Downside of the Buffer Workflow

Over the years using the buffer workflow, I started noticing a friction: over time, my buffer list would grow into an uncontrollably large list of files!

This usually happens when working on a complex task where the file set that I need to work with is not clear. I've seen my buffer list grew to 50 files (it is entirely plausible to have over 100+ files in the buffer list)

When you have that many files, it is not a sprint anymore. It is now a marathon. I don't like marathons (sorry, runners ;D). If you have that many files in your buffers list, many of them are probably irrelevant files.

There are a few things you can do. You can give up on Vim and use a different editor - jk! First, you can make a mental note of all the relevant files in your buffer list, then reopen Vim and open only the relevant files (which hopefully you still remember). However, I don't have a good short-term memory, so this isn't ideal. Second, you can create a different, new set of files that you can still sprint across, yet distinct from buffers: the arglist files.

An arglist is a controlled file set. It is a subset of the buffer list. Whereas Vim automatically adds all opened files into the buffer list, it doesn't automatically add them into the arglist. _You_ decide when and which file goes into the list.

Before you start thinking that buffers aren't that hot after reading the above paragraph - buffers are still awesome. I still use buffers more than args.

It is only when I am working with complex tasks that cause my buffer list to grow exponentially when I resort to the arglist workflow. When I know exactly which files I need to work with or if the size of my buffer list is manageable, the buffer workflow is always my immediate go-to partner.

## Vim Arglist

So what is Vim arglist?

In the terminal, when you pass to the `vim` command file(s) as arguments, like:  

```
vim file1.rb file2.rb file3.rb
```

Vim opens them and stores them not only in the buffer list, but also the arglist.

Recall that you can check all the buffer files with the `:buffers` command (or `:ls`) - to check all the args files, you can run:  

On the bottom screen, Vim displays:  

```
[file1.rb] file2.rb file3.rb
```

The enclosed file in a square bracket, `[file1.rb]`, is the current args file.

### Adding New Files to Args

If you open a new file `file4.rb`, your buffer list now has 4 items (file1 - file4). However, Vim doesn't add it into the arglist automatically. You have to do it manually.

To add `file4.rb` into the arglist, use the `:arga` command:  

Running `:args` one more time reveals that the arglist now contains 4 files: `file1.rb file2.rb file3.rb file4.rb`

If you need to remove / delete a file from the arglist, you can run the `:argd` command:  

To create a new arglist, you can run the `:args` command again - but this time you pass it file(s) arguments:  

The arglist now contains `file1.js` and `file2.js` instead of `file1.rb`, `file2.rb`, and `file3.rb`. _Running `:args YOUR_FILES` will overwrite your existing arglist_. You have been warned!

So if you run `:args` without arguments, it displays the arglist. If you run `:args some_files`, it starts a new args list with `some_files`.

### Globs

The `:args` command accepts globs so you can quickly build up your args files. To recursively add all files ending with `controller.rb`:  

### Args and the CLI Commands

You can combine most terminal search commands to populate the arglist (don't forget the back-tick). For example, to integrate the classic `find` command with arglist:  

```
:args `find . -name '*txt'`
```

Check out your arglist again (`:args`). Now it has all the text files. Cool!

## No-Fly Zone

With the buffer workflow, you can easily fly between the buffer files. Unfortunately with args, there is no way to fly between the args list. Unlike the `:buffers` command that presents you a list of buffer files for you to fly into with `:buffer BUFFER_NUMBER` or `:buffer FILE_NAME`, there is no equivalent command with args.

Maybe in the future someone will make it possible to fly with args. But for now, we have to be content with sprinting.

## Sprinting

Similar to buffer's `bnext bprev bfirst blast` commands, args have:  

The vim.unimpaired plugin also has useful commands: `[a ]a [A ]A`.

## Multi-File Arguments

With `:argdo`, you can execute an operation across the arglist (similar to `:bufdo`).

If you want to substitute all `method_a` with `method_b` across your args files, run:  

```
:argdo %s/method_a/method/b/g | update
```

## When to Use Buffers vs When to Use Args

Buffer list and arglist have their own use cases. Neither is superior than the other. So when do you use a buffer list and when do you use an arglist?

My rule of thumb is, whenever you start feeling the friction from having too many buffer files, it's a good indicator to use args. Some people can tolerate having a lot of buffer files while some can't even handle 10 buffer files (like me). Whenever my buffer files ballooned to more than 10, smokes would start coming out of my ears. This would be a good time to use arglist. Otherwise, I would stick with buffers.

You can always purge your buffer list whenever it gets too large. In fact, I have two custom commands in my vimrc: one to delete all buffers and one to delete specific buffer file(s). You can find them here in my [dotfiles](https://github.com/iggredible/dotfiles/blob/master/vim/custom-functions/remove-buffers.vim).

It is common to go into the detective mode: inspecting and opening a large number of files. You can quickly accumulate a large number of buffer files. The more files you open, the harder it is to clean up your buffer list. The argument list starts a fresh list of files, distinct from the buffer list. Once you know which files are relevant, just run `:arga %` to add the current file or `:arga name/of/file` to add to the argument list.

## Conclusion

This has been a far lengthier article than I originally thought, in a good way. The buffer and args workflows are unique to Vim. If you are not already familiar with them, get acquainted with them! They may pragmatically change your editing workflow forever!

Even if you are not a Vim user, I think it is worth learning this - and who knows, maybe after reading about buffers and args, you may implement something similar in your favorite non-Vim editor / IDE!

If you want to learn more, check out some of these resources:

-   `:h arglist` and `:h buffers` for help docs
-   [_Learn Vim Ch02. Buffers, Windows, and Tabs_](https://github.com/iggredible/Learn-Vim/blob/master/ch02_buffers_windows_tabs.md) to learn more about buffers, windows, and tabs
-   Vimcast's [_Working with buffers_](http://vimcasts.org/episodes/working-with-buffers/) and [_Meet the Arglist_](http://vimcasts.org/episodes/meet-the-arglist/) (plus other buffer and arglist related videos)
-   [_@learnvim_](https://twitter.com/learnvim) for Vim tips and tricks

