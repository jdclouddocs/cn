## 应用安全网关日志
### 简介
目前接入日志服务的应用安全官网日志类型为WAF访问日志。

### 字段说明
日志字段 | 字段描述 | 字段类型
-- | -- | --
time | 时间 | time
waf_id | wafid | string
weblock_hit | 是否命中网页防篡改 | string
weblock_hit_id | 网页防篡改id | string
cc_hit | 是否命中cc | string
cc_hit_id | cc命中规则id | string
urule_hit | 是否命中用户规则 | string
urule_hit_id | 用户规则id | string
waf_hit | 是否命中waf规则 | string
waf_hit_id | waf规则命中id | string
remote_addr | 访问源地址 | string
server_addr | 服务器地址 | string
http_cookie | cookie | string
upstream_addr | rs地址 | string
request_time | 访问时间 | double
upstream_connect_time | rs连接时间 | double
upstream_response_time | rs响应时间 | double
status | 状态码 | int
upstream_status | rs状态码 | string
request_length | 访问长度 | int
bytes_sent | 发送长度 | int
request_method | 请求方法 | string
host | host | string
request_uri | 请求uri | string
server_protocol | 协议 | string
http_referer | referer | string
http_user_agent | user_agent | string
ssl_cipher | 加密算法 | string
ssl_protocol | ssl协议 | string

