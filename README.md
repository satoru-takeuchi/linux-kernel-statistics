# Introduction

This article reveals information about linux kernel by analyzing the statistics of the source code of linux kernel. Although there are many such the articles about linux kernel statistics, many of these articles focus on *who* develops linux kernel. However, this article doesn't care about that. It deals with the following things instead.

- Which subsystems the kernel consists of?
- Which kind of drivers are main contens of whole drivers code?
- What are the characteristics of rc kernel and stable kernel?

# The Whole kernel

## The total lines of code

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/number_of_lines_for_each_release.png "")

There was just 4 milion lines in v2.6.12. However, in v4.10, 12 years (this year) later from v2.6.12, it becomes 21 milion lines. It's about 4 times bigger than v2.6.12. In most case, the lines of code has increased monotonously. It becomes lower than the previous version only in v3.17. It means the speed of linux kernel development is still high even it's a 25-years-old project.

## The number of patches

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/number_of_patches_for_each_release.png "")

It has increased till v2.6.24. After that, it has been between 10 thousands and 15 thousands in many case. Linux kernel developmen can be said active not only from the total number of lines, but also from the perspective of the number of patches.

## The number of line increase

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/number_of_line_increase_for_each_release.png "")

Although there are big dispersion, it has increased 250 thousands of code per version in average. Since linux kernel releases new versions once per 9 or 10 weeks, it means about 30 thousands of code has increased per week and about 4 thousands of code has increased per day. So it can say that huge amount of resource has put to develop linux kernel. In addition, it should say that there has put more resources to the following things.

- Changing and descreasing lines for example by refactoring or by cleanup.
- The codes which wasn't merged.

## The line increase for each patch

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/average_number_of_line_increase_per_one_patch_for_each_release.png "")

It can say that the average of the line increase for each patch hasn't changed so much. In addition, although it's average is about 20 lines, there are big dispersion.

# RC kernels

We have seen the informatoin per mainline kernel versions. What information we can get if narrowing down the granularity from per-mainline-version to per-rc-kernel?

Let's learn about the current developing style of linux kernel briefly. After a mainline kernel is released, two weeks "merge window" is open and the patches to add new feature should be merged in this window. The first release candidate of the next mainline version, so called -rc1, will be released with closing merge window. After that, rc kernel will be stabilized within 7 or 8 weeks in most case. During this time, new rc kernels has released once per week.

Here is the number of patches for each rc kernels.

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/number_of_patches_for_each_rc_release.png "")

We can see that most of patches has applied in rc1 kernels. The patches in rc1 kernels consist of any kinds from adding features, to bugfixes, refactoring, cleanups, and so on.

Let's see this the above information again focusing on versions from rc2 kernels to the next mainline kernels.

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/number_of_patches_for_each_rc_release_focus_on_rc_s_besides_rc1.png "")

It can be said that the number of patches has decreased from rc1 to the next mainline kernel. It's because the policy that only important patches can be applied as time has passed.

# Subsystems

There are many subsystems in linux kernel. Here is the list of them classified by directory structire in the source code.

|name|meaning|
|:----|:-----|
|arch|architecture dependent code|
|block|block device layer|
|crypto|cryptographycal code|
|drivers|device drivers like block devices, character devices, and NICs|
|fs|file systems like ext4, XFS, and Btrfs|
|init|initialization code|
|ipc|System V IPC|
|kernel|core code like process scheduler, interrupt, timer, and signal handling|
|lib|library code used from other subsystems|
|mm|memory management|
|net|network code like IPv{4,6}|
|security|security code like SELinux|
|sound|sound|
|virt|virtualization like KVM|

Let's see the information about subsystems.

## Which subsytems are the main contents of the whole kernel?

Here is the number of lines for each subsystems.

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/number_of_lines_of_each_subsystem_for_each_release.png "")

Apparently the driver code occupies the most of the number of lines. 2nd most is the architecture dependent code. These are followed by the filessytems, sound, and networks. Kernel core and memory management are far smaller than subsystems mentioned in the previous sentense.

## Which subsystems has grown (in terms of the number of lines) actively?

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/number_of_line_increase_of_each_subsystem_for_each_release.png "")

The drivers code is No.1 and it's the same as the number of lines. Other subsystem's has grown far slower than drivers. So it can be said that, although the linux has grown rapidly very much, most of the line increase has been from drivers.

Let's see this information without drivers.

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/number_of_line_increase_of_each_subsystem_wituout_driver_for_each_release.png "")

Architecture dependent code, filesystems, sonds, and networks has mainly contributed to the growth of linux kernel. It's similar to the information about the numer of codes. However, which subsytem has mainly been increased, is different between each version very much.

There are many lines decreases. One is architecture dependent code in v4.1 and the other is filesystems code in v4.3. I din't dig in the former. The latter is mainly because ext3 code has completely removed (NOTE: linux still supports ext3 by ext4 code.) 

# Drivers

## The number of lines for each type of drivers

I wrote the most of the whole code is about drivers. Then, what types of drivers are the main contents? Here is the top 10 types of drivers in v4.10.

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/the_amounts_of_code_under_drivers.png "")

The main contents are network drivers, GPU drivers, and multimedia drivers (for example webcam, video, TV). Since the network code (under net/ in the source) is the 2nd big component of the whole kernel. it can also say that network related code is the main contents of the whole kernel. On the other hand, block devices and character devices are not in the top 10 and are very small.

## Existing drivers or new drivers

Here is the number of line increase for existing drivers and new drivers.

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/new_driver_and_existing_driver.png "")

We can see that the most of line increase in drivers has been for new drivers. Since the most of line increase has been for drivers in the whole kernel, it also means that the most of line increase in the whole kernel is for new drivers.

# Stable kernels

Linux has the stable kernels maintained by Greg KH, in addition to the mainline kernels maintained by Linus Torvalds. Stable kernels are the mainline kernel with applying important bug fix patches and is released for each mainline kernel till the next main line kernel is released (there are exceptions, see later). The release cycle of the stable kernels are occasional. It's different from once-per-week rc kernels.

## The latest version number of for each stable kernel

The version number of stable kernels are v2.6.x.y, v3.x.y, and v4.x.y. Here is the latest version number, "y" in the above mentioned versioning rule, for each stable kernel.

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/the_latest_stable_version_for_each_version.png "")

In most case, the version numbers are at most 20. However, some of them have far higher version numbers. These stable kernel are called longterm kernel and are maintained even after releasing the new mainline kernel by someone who want to support that versions more and more. The EOF of the longterm kernel is defined by the maintainer for each that kernel.

## EOL of the stable kernels

Here is the number of days between the release of a mainline kernel and the release of the stable kernel corresponding to that mainline kernel.

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/the_number_of_days_from_the_initial_release_to_the_latest_stable_release.png "")

In most case, the number of days are under 3 months, it means the number of days between releasing a kernel and the next kernel, that numbers of longterm kernels are far longer. That numbers become over several years in some cases. The EOF of longterm kernel depends on their maintainers.

Please not that the release of some of the versions listed [here](https://www.kernel.org/category/releases.html) stil last.

## The number of line increase for each patch for stable kernels

![](https://github.com/satoru-takeuchi/linux-kernel-statistics/blob/master/image/average_line_increase_for_each_stable_patch.png "")

The numbers are apparently smaller than the numbers of mainline kernels. It's about 5 lines for most case. It's because of the stable kernel rule that the patches for stable kernels should be not only important, but also small.

Documentation/process/stable-kernel-rules.rst:
>  - It cannot be bigger than 100 lines, with context.
