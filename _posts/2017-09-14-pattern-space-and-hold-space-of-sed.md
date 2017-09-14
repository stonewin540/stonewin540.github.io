---
layout: post
title: pattern space and hold space of sed
author: stone
tags: [editor]
comment: false
---


Copied from: [stackoverflow](https://stackoverflow.com/questions/12833714/the-concept-of-hold-space-and-pattern-space-in-sed "stackoverflow")

> When sed reads a file line by line, the line that has been currently read is inserted into the pattern buffer (pattern space). Pattern buffer is like the temporary buffer, the scratchpad where the current information is stored. When you tell sed to print, it prints the pattern buffer.

> Hold buffer / hold space is like a long-term storage, such that you can catch something, store it and reuse it later when sed is processing another line. You do not directly process the hold space, instead, you need to copy it or append to the pattern space if you want to do something with it. For example, the print command p prints the pattern space only. Likewise, s operates on the pattern space.

> Here is an example:
sed -n '1!G;h;$p'

> (the -n option suppresses automatic printing of lines)

> There are three commands here: 1!G, h and $p. 1!G has an address, 1 (first line), but the ! means that the command will be executed everywhere but on the first line. $p on the other hand will only be executed on the last line. So what happens is this:

>    first line is read and inserted automatically into the pattern space
    on the first line, first command is not executed; h copies the first line into the hold space.
    now the second line replaces whatever was in the pattern space
    on the second line, first we execute G, appending the contents of the hold buffer to the pattern buffer, separating it by a newline. The pattern space now contains the second line, a newline, and the first line.
    Then, h command inserts the concatenated contents of the pattern buffer into the hold space, which now holds the reversed lines two and one.
    We proceed to line number three -- go to the point (3) above.

> Finally, after the last line has been read and the hold space (containing all the previous lines in a reverse order) have been appended to the pattern space, pattern space is printed with p. As you have guessed, the above does exactly what the tac command does -- prints the file in reverse.

---- translation ----<br />
当 sed 从文件中一行一行的读取时，当前被读取的那一行会被插入到 pattern buffer（pattern space）。pattern buffer 就像是一个临时性的缓冲区，中间结果寄存器就是把当前的信息先暂存到里面。当你通知 sed 去打印的时候，它打印的就是 pattern buffer。

Hold buffer / hold space 更像是一个长期的寄存器，你可以捕获信息，存储并且过后再重用它们当 sed 在处理另一行的时候。你不可能直接操作 hold space，然而，如果你想要用它干点什么的时候可以拷贝它并把它追加到 pattern space 。举例来说，打印命令 p 只是将 pattern space 打印出来。相同的，s 命令只是在 pattern space 之上进行操作。

咱们来实际操作一下：
sed -n '1!G;h;$p'

(-n 选项就是取消了自动打印)

这里有三个命令：1!G, h and $p. 1!G 有一个 address，1（第一行），但是 ! 意味着命令可以在任何地方执行出了第一行。$p 与之不同只会在最后一行执行。实际过程是这样的：
	
	第一行被读取并自动插入到了 pattern space
	在第一行，第一条命令并没有被执行；h 将第一行拷贝到 hold space。
	现在第二行取代了 pattern space 中的内容
	在第二行，我们首先执行 G，追加 hold buffer 中的内容到 pattern buffer，用一个换行符将其分割。pattern space 中现在包含第二行，一个换行符，还有第一行。
	接下来，h 命令将已经连接在一起的 pattern buffer 插入到 hold space，那里现在保存的是倒序的第二行和第一行。
	之后我们继续处理第三行 -- 重复上面的第三步。

最终，在最后一行被已经读取并且 hold space（包含着所有倒序的行）已经被追加到了 pattern space 之后，pattern space 被 p 打印出来。正如你所预料的，以上正是 tac 命令所做的事情 -- 倒序打印文件。
