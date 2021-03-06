.. _scheduling_v2:

调度
##########

内核的调度器是基于优先级的，它允许应用程序的多个线程共享 CPU。

.. contents::
    :local:
    :depth: 2

概念
********

调度器的主要作用是判断将要执行哪个线程。被调度器选定的线程叫做 **当前线程**。

无论什么时候，当调度器改变当前线程的标识符，或者当前线程被 ISR 替代时，内核都会先保存当前线程的 CPU 寄存器值。当这个线程在之后恢复执行时，这些寄存器的值就会被恢复。

线程状态
=============

如果一个线程没有阻碍其执行的因子，就被认为是 **就绪态**。就绪的线程可以被选择作为当前线程。

如果一个线程有一个或多个阻碍其执行的因子，就被认为是 **非就绪态**。非就绪的线程不能被选择作为当前线程。

下列因子将使线程成为非就绪线程：

* 线程还未被启动。
* 线程正在等待某个内核对象。（例如，现在正在获取一个无效的信号量。）
* 线程正在等待延时服务。
* 线程被挂起。
* 线程已经结束或终止。

线程优先级
=================

线程的优先级是一个整数值，可以是负数或者非负数。数字越低，优先级越高。例如，如果线程 A 的优先级是 4，线程 B 的优先级是 7，调度器则认为 A 的优先级比 B 的优先级高；同样地，如果线程 C 的优先级是 -2，则它的优先级比 A 和 B 都高。

调度器基于线程的优先级将线程分为两类：

* :dfn:`协作式线程（cooperative thread）`：优先级为负数的线程。这样的线程一旦成为了当前线程，它将一直执行下去，直到它采取的某种动作导致自己变为非就绪线程。

* :dfn:`抢占式线程（preemptible thread）`：优先级为非负的线程。这样的线程成为了当前线程后，它可以在任何时刻被协作式线程或者优先级更高（或相等）的抢占式线程替代。抢占式线程被替代后，它依然是就绪态。

线程的初始优先级值可以在线程启动后动态地增加或减小。因此，通过改变线程的优先级，抢占式线程可以变为协作式线程，或者相反。

内核几乎可以支持无数个优先级等级。配置选项 :option:`CONFIG_NUM_COOP_PRIORITIES` 和 :option:`CONFIG_NUM_PREEMPT_PRIORITIES` 指定了这两种线程的优先级的范围：

* 协作式线程: (-:option:`CONFIG_NUM_COOP_PRIORITIES`) 至 -1
* 抢占式线程: 0 至 (:option:`CONFIG_NUM_PREEMPT_PRIORITIES` - 1)

例如，将协作式和抢占式线程的优先级数配置为 5 和 10，其对应有优先级范围是 -5 到 -1、0 到 9。

调度算法
====================

内核的调度器总是选择优先级最高的就绪线程作为当前线程。当多个线程具有相同的优先级时，调度器选择等待时间最久的线程。

.. note::
    ISR 优先于线程，因此当前线程可能会在任何时刻被非屏蔽中的的 ISR 代替。这对协作式线程和抢占式线程都成立。

协作式时间片
========================

协作式线程一旦成为了当前线程，它将一直执行下去，直到它采取的某种动作导致自己变为非就绪线程。这种方式其实有一个缺陷，即如果协作式线程需要执行长时间的计算，将导致包括优先级高于或等于该线程在内的其它所有线程的调度被延迟到一个不可接受的时间之后。

为了解决这个问题，协作式线程可以自身间或性地放弃 CPU，让其它线程得以执行。线程放弃 CPU 的方法有两种：

* 调用 :cpp:func:`k_yield()` 将线程放到调度器维护的按照优先级排列的就绪线程链表中，然后调用调度器。在该线程被再次调度前，所有优先级高于或等于该线程的就绪线程都将得以执行。如果不存在优先级更高或相等的线程，调度器将不会进行上下文切换，立即再次调度该线程。

* 调用 :cpp:func:`k_sleep()` 让该线程在一段指定时间内变为非就绪线程。*所有* 优先级的就绪线程都可能得以执行；不过，不能保证优先级低于该睡眠线程的其它线程都能在睡眠线程再次变为就绪线程前执行完。

抢占式时间片
=======================

抢占式线程成为了当前线程后，它将一直执行下去，直到有更高优先级的线程变为就绪线程，或者线程自己执行了某种动作导致其变为非就绪线程。相应地，如果抢占式线程需要执行长时间的计算，将导致包括优先级等于该线程在内的其它所有线程的调度被延迟到一个不可接受的时间之后。

为了解决这个问题，可抢占式线程可以执行协作式时间片（如上面所述）或者使用调度器的时间片功能，让优先级等于该线程的其它线程得以执行。

调度器将时间分割为一系列的 **时间片**。时间片的单位是系统时钟滴答。时间片的大小是可配置的，并且可以在程序运行期间修改。

在每个时间片结束时，调度器会检查当前线程是否是可抢占的。如果是，它将对该线程隐式地调用 :cpp:func:`k_yield()`，让其它同优先级的就绪线程在该线程被再次调度前得以执行；否则，当前线程继续执行。

优先级高于指定极限的线程不用实现抢占式时间片，且不能被同优先级的其它线程抢占。应用程序只有当处理优先级更低且对时间不敏感的线程时采用抢占式时间片。

.. note::
   内核的时间片算法 *不* 确保同等优先级的所有线程占用的 CPU 时间完全相同，因为它不会测量线程的实际执行时间。例如，某个线程可能在时间片快完的时候才刚刚执行，但是时间片到后会立即释放 CPU。尽管如此，该算法将 *确保* 某个线程的执行时间超过单个时间片的长度后释放 CPU（译注：当然，也可能释放 CPU 后不进行上下文切换而立即再次执行）。

调度器锁
=================

如果抢占式线程希望在执行某个特殊的操作时不被抢占，它可以调用 :cpp:func:`k_sched_lock()`，让调度器将其临时当做协作式线程，从而避免被抢占。

一旦完成特殊操作，该线程必须调用 :cpp:func:`k_sched_unlock()`，以恢复其可抢占特性。

如果线程调用了 :cpp:func:`k_sched_lock()`，但是随后执行了一个动作导致其非就绪，调度器会将这个锁定的线程切换出去，以允许其它线程得以执行。当锁定的线程再次成为当前线程后，其不可抢占状态依然有效。

.. note:
    相对于将线程的优先级设为负数，锁定调度器是一个让抢占式线程拥有不可抢占特性的更高效的做法。

.. _thread_sleeping:

线程睡眠
===============

线程可以调用 :cpp:func:`k_sleep()` 让其延迟一段指定的时间后再执行。在线程睡眠的这段时间，CPU 被释放给其它线程。指定的时间到达后，线程将变为就绪状态，然后才能够再次被调度。

正在睡眠的线程可以被其它线程使用 :cpp:func:`k_wakeup()` 唤醒。这种技术可以让其它线程给该睡眠线程发送信号，而睡眠线程 *不* 需要请求某个内核对象（例如信号量）。唤醒一个未睡眠的线程也是允许的，但是不是有如何效果。

.. _busy_waiting:

忙等待
============

线程可以调用 :cpp:func:`k_busy_wait()` 执行一个 ``忙等待`` 操作。所谓的忙等待，指的是线程延迟一段指定的时间后再处理相关任务，但是它并不会将 CPU 释放给其它就绪线程。

使用忙等待而不使用线程睡眠的典型情况是：由于所需要的延迟太短，调度器来不及从当前线程切换到其它线程再切换回当前线程。

建议的用法
**************

建议在设备驱动程序和执行挑剔任务时使用协作式线程。

使用协作式线程实现手动排除，而不需要内核对象（例如互斥量）。

使用抢占式线程让时间更敏感的处理比时间不敏感的处理先执行。

配置选项
*********************

相关的配置选项：

* :option:`CONFIG_NUM_COOP_PRIORITIES`
* :option:`CONFIG_NUM_PREEMPT_PRIORITIES`
* :option:`CONFIG_TIMESLICING`
* :option:`CONFIG_TIMESLICE_SIZE`
* :option:`CONFIG_TIMESLICE_PRIORITY`

APIs
****

头文件 :file:`kernel.h` 中提供了如下的线程调度 API：

* :cpp:func:`k_current_get()`
* :cpp:func:`k_sched_lock()`
* :cpp:func:`k_sched_unlock()`
* :cpp:func:`k_yield()`
* :cpp:func:`k_sleep()`
* :cpp:func:`k_wakeup()`
* :cpp:func:`k_busy_wait()`
* :cpp:func:`k_sched_time_slice_set()`
