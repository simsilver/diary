Title: Android NDK shared library undefined reference
Date: 2016-02-24 16:50
Tags: android, ndk, link
Category: 备忘

使用 ndk 开发时遇到一个问题，
动态链接库 mymod 依赖静态库 mod1, mod2, mod3，
使用如下形式Android.mk 成功编译 mod1, mod2, mod3,
但最后链接 mymod 时提示某些函数为 undefined reference，
且确认函数确实已定义：

~~~
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)
LOCAL_MODULE    := mod1
LOCAL_SRC_FILES := mod1.cpp
include $(BUILD_STATIC_LIBRARY)

include $(CLEAR_VARS)
LOCAL_MODULE    := mod2
LOCAL_SRC_FILES := mod2.cpp
include $(BUILD_STATIC_LIBRARY)

include $(CLEAR_VARS)
LOCAL_MODULE    := mod3
LOCAL_SRC_FILES := mod3.cpp
include $(BUILD_STATIC_LIBRARY)

include $(CLEAR_VARS)
LOCAL_MODULE    := mymod
LOCAL_SRC_FILES := mymod.cpp
LOCAL_STATIC_LIBRARIES  := mod1 mod2 mod3
include $(BUILD_SHARED_LIBRARY)
~~~

尝试后发现 mod1, mod2, mod3 也有依赖关系，
mod2 依赖 mod1,
mod3 依赖 mod1, mod2,
在最后 link 时一次指定所有静态库这种形式在 linux 上编译时可以通过，
但使用 ndk 就会失败。
尝试**直接在模块中指定依赖库**后编译通过，
最后 Android.mk 如下：

~~~
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)
LOCAL_MODULE    := mod1
LOCAL_SRC_FILES := mod1.cpp
include $(BUILD_STATIC_LIBRARY)

include $(CLEAR_VARS)
LOCAL_MODULE    := mod2
LOCAL_SRC_FILES := mod2.cpp
# 依赖 mod1
LOCAL_STATIC_LIBRARIES  := mod1
include $(BUILD_STATIC_LIBRARY)

include $(CLEAR_VARS)
LOCAL_MODULE    := mod3
LOCAL_SRC_FILES := mod3.cpp
# 依赖 mod1 mod2
LOCAL_STATIC_LIBRARIES  := mod1 mod2
include $(BUILD_STATIC_LIBRARY)

include $(CLEAR_VARS)
LOCAL_MODULE    := mymod
LOCAL_SRC_FILES := mymod.cpp
LOCAL_STATIC_LIBRARIES  := mod1 mod2 mod3
include $(BUILD_SHARED_LIBRARY)
~~~
