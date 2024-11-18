---
title: Blackholed问题
updated: 2022-10-03T16:15:33
created: 2022-10-03T16:07:09
---

**Blackholed 黑洞问题**
向 Exchange 投递了 Message ，而由于各种原因导致该 Message 丢失，但发送者却不知道，以为投递失败，会发送。

可导致 blackholed 的情况：
1、向未绑定 queue 的 exchange 发送 message 。
2、exchange 以 key_A 绑定了 queue_A，但向该 exchange 发送 message 使用的却是 key_B 。

**如何防止出现 blackholed 问题？**

没有特别好的办法，只能在具体实践中通过各种方式保证相关 fabric 的存在。另外，如果在执行 Basic.Publish 时设置 mandatory=true ，则在遇到可能出现 blackholed 情况时，服务器会通过返回 Basic.Return 告之当前 message 无法被正确投递（内含原因 312 NO_ROUTE）。
