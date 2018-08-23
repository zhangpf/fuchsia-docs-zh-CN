# zx_timer_set
----

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/syscalls/timer_set.md)

----
<!-- ## NAME -->
## 名称

<!-- timer_set - start a timer -->
timer_set —— 启动定时器

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_timer_set(zx_handle_t handle, zx_time_t deadline,
                         zx_duration_t slack);

```

<!-- ## DESCRIPTION -->
## 描述
<!-- 
**zx_timer_set**() starts a one-shot timer that will fire when
*deadline* passes. If a previous call to **zx_timer_set**() was
pending, the previous timer is canceled and
**ZX_TIMER_SIGNALED** is de-asserted as needed. -->
**zx_timer_set()** 的功能是启动在*deadline*到来时被触发的一次性定时器。如果先前对该定时器的**zx_timer_set()** 的调用还在等待被触发状态中，则取消先前的定时器，并根据需要取消发出**ZX_TIMER_SIGNALED**信号。

<!-- The *deadline* parameter specifies a deadline with respect to
**ZX_CLOCK_MONOTONIC**. To wait for a relative interval,
use **zx_deadline_after**() returned value in *deadline*. -->
*deadline*参数指定了相对于**ZX_CLOCK_MONOTONIC**的截止日期。为了获取相对时间间隔，请使用**zx_deadline_after()** 的返回值传递给*deadline*。

<!-- To fire the timer immediately pass a *deadline* less than or equal to **0**. -->
要立即触发计时器，请传递小于或等于**0**的值到*deadline*。
<!-- When the timer fires it asserts **ZX_TIMER_SIGNALED**. To de-assert this
signal call **timer_cancel**() or **timer_set**() again. -->
当计时器被触发时，它发出**ZX_TIMER_SIGNALED**信号。如果需取消触发此信号，请调用**timer_cancel()** 或再次调用**timer_set()**。

<!-- The *slack* parameter specifies a range from *deadline* - *slack* to
*deadline* + *slack* during which the timer is allowed to fire. The system
uses this parameter as a hint to coalesce nearby timers. -->
*slack*参数指定(*deadline* - *slack*, *deadline* + *slack*)的范围，在此期间允许定时器触发。系统使用此参数作为提示来合并附近的计时器。

<!-- 
The precise coalescing behavior is controlled by the *options* parameter
specified when the timer was created. **ZX_TIMER_SLACK_EARLY** allows only
firing in the *deadline* - *slack* interval and **ZX_TIMER_SLACK_LATE**
allows only firing in the *deadline* + *slack* interval. The default
option value of 0 is **ZX_TIMER_SLACK_CENTER** and allows both early and
late firing with an effective interval of *deadline* - *slack* to
*deadline* + *slack* -->
精确的合并行为由创建计时器时指定的*options*参数所控制。**ZX_TIMER_SLACK_EARLY**仅允许定时器在(*deadline* - *slack*, *deadline*)的区间内被触发，而**ZX_TIMER_SLACK_LATE**仅允许在(*deadline*, *deadline* + *slack*)的区间内被触发。默认的*options*值0等同于**ZX_TIMER_SLACK_CENTER**，并允许较早和较晚被触发，即有效区间为(*deadline* - *slack*, *deadline* + *slack*)。

<!-- ## RIGHTS -->
## 权限

<!-- *handle* must have **ZX_RIGHT_WRITE**. -->
*handle*需具有**ZX_RIGHT_WRITE**权限。


<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_timer_set**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned. -->
**zx_timer_set()** 调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。


<!-- **ZX_ERR_ACCESS_DENIED**  *handle* lacks the right **ZX_RIGHT_WRITE**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少**ZX_RIGHT_WRITE**权限。

<!-- **ZX_ERR_OUT_OF_RANGE**  *slack* is negative. -->
**ZX_ERR_OUT_OF_RANGE**：*slack*是负值。

<!-- ## SEE ALSO -->
## 另见

<!-- [timer_create](timer_create.md),
[timer_cancel](timer_cancel.md),
[deadline_after](deadline_after.md) -->

[timer_create](timer_create.md)，[timer_cancel](timer_cancel.md)，[deadline_after](deadline_after.md)
