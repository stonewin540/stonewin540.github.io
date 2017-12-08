---
layout: post
title: Unified Logging System
author: stone
comment: false
---

[WWDC2016-721: Unified Logging and Activity Tracing](https://developer.apple.com/videos/play/wwdc2016/721/)<br />
<br />
先来一条命令：
```shell
log stream --predicate '(senderImagePath CONTAINS "Ur iOS AppName") || (processImagePath CONTAINS "Ur iOS AppName")' --level debug
```
部分输出：
```
Filtering the log data using "senderImagePath CONTAINS "Ur iOS AppName" OR processImagePath CONTAINS "Ur iOS AppName""
Timestamp                       Thread     Type        Activity             PID    
...
2017-12-07 18:33:56.230481+0800 0x1daa15   Activity    0x80000000000ab54b   48240  Ur iOS AppName: (UIKit) send gesture actions
2017-12-07 18:33:56.247135+0800 0x1daa15   Activity    0x80000000000ab54c   48240  Ur iOS AppName: (UIKit) send gesture actions
2017-12-07 18:33:56.270794+0800 0x1daa15   Activity    0x80000000000ab54d   48240  Ur iOS AppName: (UIKit) send gesture actions
2017-12-07 18:33:56.358427+0800 0x1daa15   Activity    0x80000000000ab54e   48240  Ur iOS AppName: (UIKit) send gesture actions
2017-12-07 18:33:56.382574+0800 0x1daa15   Activity    0x80000000000ab54f   48240  Ur iOS AppName: (UIKit) send gesture actions
2017-12-07 18:33:56.967288+0800 0x1daa15   Activity    0x80000000000ab560   48240  Ur iOS AppName: (UIKit) send gesture actions
2017-12-07 18:33:57.087589+0800 0x1db404   Default     0x0                  48240  Ur iOS AppName: (CFNetwork) Task <E1909D86-F6C8-4AFF-A567-65B59EFE0F22>.<37> now using Connection 10
2017-12-07 18:33:57.087660+0800 0x1db404   Default     0x0                  48240  Ur iOS AppName: (CFNetwork) Task <E1909D86-F6C8-4AFF-A567-65B59EFE0F22>.<37> sent request, body S
2017-12-07 18:33:57.230273+0800 0x1db402   Default     0x0                  48240  Ur iOS AppName: (CFNetwork) Task <E1909D86-F6C8-4AFF-A567-65B59EFE0F22>.<37> received response, status 200 content K
2017-12-07 18:33:57.230509+0800 0x1db906   Default     0x0                  48240  Ur iOS AppName: (CFNetwork) Task <E1909D86-F6C8-4AFF-A567-65B59EFE0F22>.<37> response ended
2017-12-07 18:33:57.231838+0800 0x1db404   Activity    0x80000000000ab561   48240  Ur iOS AppName: (CoreFoundation) Sending Updated Preferences to System CFPrefsD
2017-12-07 18:34:24.067014+0800 0x1db907   Default     0x0                  48240  Ur iOS AppName: (CFNetwork) Task <CE880C85-1BBF-4E29-8C34-1C0BE861EC21>.<38> now using Connection 10
2017-12-07 18:34:24.067108+0800 0x1db907   Default     0x0                  48240  Ur iOS AppName: (CFNetwork) Task <CE880C85-1BBF-4E29-8C34-1C0BE861EC21>.<38> sent request, body S
2017-12-07 18:34:24.190890+0800 0x1db404   Default     0x0                  48240  Ur iOS AppName: (CFNetwork) Task <CE880C85-1BBF-4E29-8C34-1C0BE861EC21>.<38> received response, status 200 content K
2017-12-07 18:34:24.191559+0800 0x1db404   Default     0x0                  48240  Ur iOS AppName: (CFNetwork) Task <CE880C85-1BBF-4E29-8C34-1C0BE861EC21>.<38> response ended
2017-12-07 18:34:24.192855+0800 0x1db9d2   Activity    0x80000000000ab562   48240  Ur iOS AppName: (CoreFoundation) Sending Updated Preferences to System CFPrefsD
2017-12-07 18:34:44.022264+0800 0x1daa15   Activity    0x80000000000ab563   48240  Ur iOS AppName: (UIKit) send control actions
2017-12-07 18:34:44.087356+0800 0x1daa15   Activity    0x80000000000ab564   48240  Ur iOS AppName: (UIKit) send control actions
2017-12-07 18:34:44.088332+0800 0x1daa15   Activity    0x80000000000ab565   48240  Ur iOS AppName: (UIKit) send control actions
...
```

详尽吗？连 UIKit、CFNetwork 的隐藏 log 都被打印出来了👍！<br />
os_log 只能 iOS10 之后可用...<br />
os_log 对应的 terminal 命令是什么？syslog 依然停留在 ASL 的层面，难到是因为我没有升级 High Sierra 的原因吗？<br />
算是广告占位吧，最近正在研究这个，希望自己之后能给详细描述整理一下...<br />
