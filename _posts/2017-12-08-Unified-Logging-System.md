---
layout: post
title: Unified Logging System
author: stone
comment: false
tags: [log, debug]
---

先来一条命令：
此时我的 iOS Simulator 在运行我自己的 app
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

---
新鲜出炉：<br />
logger 是针对 syslog 的简洁 API，对应 ULS 用什么效果呢？<br />
我们分别调用<br />
```
logger -is -t MYTEST -p 7 "[MYTEST]: Can U see me?(-p 7)"
logger -is -t MYTEST -p 6 "[MYTEST]: Can U see me?(-p 6)"
logger -is -t MYTEST -p 5 "[MYTEST]: Can U see me?(-p 5)"
logger -is -t MYTEST -p 4 "[MYTEST]: Can U see me?(-p 4)"
logger -is -t MYTEST -p 3 "[MYTEST]: Can U see me?(-p 3)"
logger -is -t MYTEST -p 2 "[MYTEST]: Can U see me?(-p 2)"
logger -is -t MYTEST -p 1 "[MYTEST]: Can U see me?(-p 1)"
logger -is -t MYTEST -p 0 "[MYTEST]: Can U see me?(-p 0)"
```
terminal 输出
```
Dec  8 12:55:34  MYTEST[48944] <Debug>: [MYTEST]: Can U see me?(-p 7)
Dec  8 12:55:45  MYTEST[48945] <Info>: [MYTEST]: Can U see me?(-p 6)
Dec  8 12:55:59  MYTEST[48946] <Notice>: [MYTEST]: Can U see me?(-p 5)
Dec  8 12:56:06  MYTEST[48947] <Warning>: [MYTEST]: Can U see me?(-p 4)
Dec  8 12:56:16  MYTEST[48948] <Error>: [MYTEST]: Can U see me?(-p 3)
Dec  8 12:56:23  MYTEST[48950] <Critical>: [MYTEST]: Can U see me?(-p 2)
Dec  8 12:56:31  MYTEST[48951] <Alert>: [MYTEST]: Can U see me?(-p 1)
Dec  8 12:56:37  MYTEST[48952] <Emergency>: [MYTEST]: Can U see me?(-p 0)
```
log stream 输出
```
log stream --debug --predicate '(senderImagePath CONTAINS "logger") || (processImagePath CONTAINS "logger")'
Filtering the log data using "senderImagePath CONTAINS "logger" OR processImagePath CONTAINS "logger""
Timestamp                       Thread     Type        Activity             PID    
2017-12-08 11:23:02.821408+0800 0x1f62a0   Debug       0x0                  48708  logger: [MYTEST]: Can U see me?(-p 7)
2017-12-08 11:23:28.957501+0800 0x1f636f   Info        0x0                  48709  logger: [MYTEST]: Can U see me?(-p 6)
2017-12-08 11:23:58.765892+0800 0x1f650d   Default     0x0                  48711  logger: [MYTEST]: Can U see me?(-p 5)
2017-12-08 11:24:05.365871+0800 0x1f65a4   Default     0x0                  48712  logger: [MYTEST]: Can U see me?(-p 4)
2017-12-08 11:24:14.310924+0800 0x1f65c4   Default     0x0                  48713  logger: [MYTEST]: Can U see me?(-p 3)
2017-12-08 11:24:21.381879+0800 0x1f6646   Default     0x0                  48714  logger: [MYTEST]: Can U see me?(-p 2)
2017-12-08 11:24:31.934175+0800 0x1f66d7   Default     0x0                  48715  logger: [MYTEST]: Can U see me?(-p 1)
2017-12-08 11:24:38.845758+0800 0x1f66fc   Default     0x0                  48716  logger: [MYTEST]: Can U see me?(-p 0)
```
+ 之前的 0～7 （Debug～Emergency）个等级，被新的 Debug～Fault 取代后，只能对应上 Debug～Default；
+ -t tag 对 ULS 没有作用；
+ ULS 中找不到对应的 subsystem，但是可以通过 logger 进行过滤；
+ 而且在 logger 的 message 字段中增加自己手动的标签也可以起到 subsystem 的作用；
<br />

---
再来一组：
```
stone$ logger -is -t MYTEST -p 7 "[MYTEST]: Can U see me?(-p 7)"
Dec  8 13:12:23  MYTEST[48975] <Debug>: [MYTEST]: Can U see me?(-p 7)

stone$ logger -s -t MYTEST -p 7 "[MYTEST]: Can U see me?(-p 7)"
Dec  8 13:12:46  MYTEST[48976] <Debug>: [MYTEST]: Can U see me?(-p 7)

stone$ logger -t MYTEST -p 7 "[MYTEST]: Can U see me?(-p 7)"

stone$ logger -s -p 7 "[MYTEST]: Can U see me?(-p 7)"
Dec  8 13:13:24  stone[48979] <Debug>: [MYTEST]: Can U see me?(-p 7)
```
log stream
```
stone$ log stream --debug --predicate '(eventMessage CONTAINS "MYTEST")'
Filtering the log data using "eventMessage CONTAINS "MYTEST""
Timestamp                       Thread     Type        Activity             PID    
2017-12-08 13:12:23.204626+0800 0x208094   Debug       0x0                  48975  logger: [MYTEST]: Can U see me?(-p 7)
2017-12-08 13:12:46.029104+0800 0x208174   Debug       0x0                  48976  logger: [MYTEST]: Can U see me?(-p 7)
2017-12-08 13:12:52.190058+0800 0x2081ed   Debug       0x0                  48977  logger: [MYTEST]: Can U see me?(-p 7)
2017-12-08 13:13:24.797663+0800 0x2083b2   Debug       0x0                  48979  logger: [MYTEST]: Can U see me?(-p 7)
```
+ -i 没有看到变化，对 logger 本身可能已经没有影响了；
+ -s 只影响了 terminal 输出，但不影响 ULS 的输出；
+ 采用 MYTEST（伪 subsystem）过滤结果依然有效；

* 基于以上结果，基本已经可以在 shell 脚本中使用 logger 来自定义自己的 log；
* 代码中可以使用 NSLog() or os_log；
* syslog 是否比 logger 更强大❓；

---

---
References: <br />
[Apple Developer Documentation: Logging](https://developer.apple.com/documentation/os/logging#2878594?language=objc)<br />
[WWDC2016-721: Unified Logging and Activity Tracing](https://developer.apple.com/videos/play/wwdc2016/721/)<br />
[Logging: Using the os_log APIs](https://developer.apple.com/library/content/samplecode/Logging/Introduction/Intro.html)<br />

