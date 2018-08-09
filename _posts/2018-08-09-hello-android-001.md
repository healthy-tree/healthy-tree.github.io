---
title: "Hello Android 001"
date: "2018-08-09 10:51"
key: 20180809
tags:
  - Android
  - Debug
  - Log
lang: zh-Hans
---

> Git Tips
> List of all files changed in a commit:
> ``` Git
> git diff-tree --no-commit-id --name-only -r <commit-ish>
> ```

# Android 日志系统

## Android Monitor

显示调试消息的 logcat 监视器。

## Logcat 日志格式

``` Logcat
date time PID-TID/package priority/tag: message
```

## Logcat 优先级

* V — 详细（最低优先级）

* D — 调试

* I — 信息

* W — 警告

* E — 错误

* A — 断言

## 使用

### 添加 TAG

``` Java
private static final String TAG = "MyActivity";
```

``` Android Studio live templates
* logt
```

### 添加日志

``` Android Studio live templates
* logv
* logd
* logi
* logw
* loge
* loga
```
### 添加日志控制机制

``` Android
if (Log.isLoggable(TAG, Log.DEBUG)) {
    Log.d(TAG, "onInit: Unknown Error");
}
```

### 修改设备 Prop
``` ADB Shell
adb shell

setprop log.tag.MyActivity DEBUG

setprop log.tag.MyActivity INFO
```

### 发布版本日志就不见了，也不用巴拉巴拉的框架，And So On。
