# wats-magisk
娲刺 magisk 修改版 (Modified Magisk for WATS)  


## 初步设计

+ 基于 magisk 的工作原理和大部分设计

+ 复用独立的 magisk 工具, 减少重复造轮子

  magiskboot, magiskpolicy, resetprop, magisk, busybox

+ 兼容 (大部分) magisk 模块与 magisk 功能

+ 用 rust 重写 magiskinit, su

+ 隐藏自身 (hide) 是第一优先功能,
  尽量减少对系统的修改

+ 默认为 **标准隐藏** 模式, 用于普通 app

+ 可启用 **增强隐藏** 模式, 用于顽固 app (积极检测 magisk, 系统修改的)

+ 只有明确启用 **停用隐藏** 模式的 app 才能获取 root
  (白名单模式)

+ 支持在 `cust` (或 `oem`) 分区安装部分模块文件,
  以便在系统启动的早期 (解密 data 之前) 进行操作和替换文件

+ `maskfs` (fuse) 用于代理 /proc 的访问,
  用于对付顽固 app 从 /proc 发现蛛丝马迹
  (记录 app 对 /proc 的访问行为)

  虚拟化 app 对 /proc 和 /sdcard 的访问,
  进一步增强隐藏, 记录访问日志

+ KBPM (Kernel Based Process Monitor)
  在内核侧监控 `zygote` 生成新的 app 进程,
  高效, 稳定, 难以被发现 (更好的隐藏)

  内核模块: `wats1`

  字符设备节点: `/dev/随机字符_wats.1`


TODO
