---
layout: post
title: iOS11 的坑：UITableViewDelegate
author: stone
comment: false
---

UITableViewDelegate 的默认行为更改；<br />

<br />
之前可以仅仅实现<br />
`- (CGFloat)tableView:(UITableView *)tableView heightForFooter/HeaderInSection:(NSInteger)section`<br />
就能自定义 footer／header 的高度；<br />

但是现在**必须实现**<br />
`- (UIView *)tableView:(UITableView *)tableView viewForFooter/HeaderInSection:(NSInteger)section`<br />
可以随便 init 一个 UIView，这样才会调用上面的方法来自定义 footer／header 的高度；<br />

<br />
但是没有看到 UITableViewDelegate 官方文档有提及，有空查一下 iOS11 新特性的文档，不知道那里面是否提及～<br />
That is a 🕳️!
