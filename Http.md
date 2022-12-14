# Http

## 各版本更新内容

### 1.1

1. 默认长连接
2. 同时打开多个TCP连接
3. 支持流水线pipeline （存在阻塞问题，所以几乎没人用）
4. 支持虚拟主机
5. 支持分块传输文件
6. 新增缓存指令 max-age Etag

### 2.0

1. 二进制分帧
2. 首部压缩
3. 服务端推送（请求一次，返回多个文件）

### 3.0

1. 使用UDP替换掉TCP (彻底解决阻塞问题，阻塞本质上是因为tcp的窗口)
2. 连接迁移
3. 0-1个RTT连接
4. 前向纠错（传输十个包后，紧跟着这十个包的异或包，这样如果丢了其中一个，可以根据剩余十个来直接还原这个包。后被废除）

## 客户端缓存

### 强制缓存

#### Expires

Expires (v1.0) Expires 后加指定日期

缺点

1. 受限于客户端的时间
2. 无法处理私有资源
3. 无法描述 “不缓存”的语义

#### Cache-Control

Cache-Control (v1.1) 优先级大于 Expries

max-age + 秒数   客户端

s-maxage + 秒数  CDN

public private

no-cache 使强制缓存失效 但协商缓存仍旧生效

no-store 禁止客户端CDN以任何形式保存资源 

### 协商缓存

#### Last-Modified 和 Last-Modified-Since 

Last-Modified 和 Last-Modified-Since (v1.0) 使用资源的修改时间来确定，精确到秒。再次请求若未修改返回 304。

缺点是若一个文件在一秒内修改多次，则会失效。

#### Etag 和 If-Match If-None-Match

Etag (v1.1) 优先级大于Last-Modified

对文件做摘要计算。意味着每次请求都需要计算一遍。

## JWT

header + payload + signature

缺点

1. 令牌难以主动失效
2. 更容易遭受到重放攻击
3. 只能携带相当有限的数据（http无限制，但服务器、浏览器一般有限制）
4. 必须考虑令牌在客户端如何存储
5. 无状态也不总是好的，例如统计实时在线人数

## 攻击技术

### 重放攻击

使用时间戳+随机数 nonce，保证时间范围内随机数不重复。

### XSS

1. 设置cookie为 http only
2. 转义特殊字符

### CSRF

1. 检查Referer
2. 校验token
3. 验证码

### SQL注入

转义特殊字符

## TLS/SSL

1. RSA 使用证书密钥。不具有前向安全性，过于依赖私钥，若私钥泄露之前的会话也会被破解
2. DH

## TCP 7种定时器

1. 建立连接定时器
2. 重传定时器
3. 延迟应答定时器
4. 坚持定时器（对方窗口为0时 定时探测）
5. 保活定时器 keep alive
6. FIN_WAIT_2 定时器
7. TIME_WAIT 定时器

## TCP

1. 可靠传输-超时重传
2. 流量控制-滑动窗口
3. 拥塞控制-慢开始、拥塞避免、快重传、快恢复

