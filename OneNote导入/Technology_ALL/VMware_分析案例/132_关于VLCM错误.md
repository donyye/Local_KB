关于VLCM错误

2021年10月25日

14:08

关于VLCM的更新页面显示"身份验证失败，无法访问Lifecycle Manager服务器"的问题，通过检查和确认，

这是vCenter appliance 7.0 u3的一个Bug，目前官方开发那边在调查分析中，还没有直接的解决方案，

如果使用更新功能可以换用sso admin账户登录。我会关注该KB的进展，如果颁布的解决方案，会及时提供给您解决办法。

 

2021-10-12T08:29:22.04Z info vlcm \[auth/session.go:143\] Scheduling session cleanup in 23h45m49.959942124s

2021-10-12T08:33:54.702Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Security Context missing in the request

2021-10-12T08:33:54.702Z info vlcm \[auth/session.go:199\] Session not found

2021-10-12T08:33:54.702Z error vlcm \[auth/handlers.go:343\] Session not found.

2021-10-12T08:33:54.702Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Authentication failed.

2021-10-12T08:33:54.702Z info vlcm \[logger/teelogger.go:54\] \[opID=vapi\] Message formatter created for en, en, UTC, 2, SHORT_DATE_TIME

2021-10-12T08:33:54.703Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Internal server error occured on authorization: com.vmware.vapi.std.errors.unauthenticated

2021-10-12T08:35:40.382Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Security Context missing in the request

2021-10-12T08:35:40.382Z info vlcm \[auth/session.go:199\] Session not found

2021-10-12T08:35:40.382Z error vlcm \[auth/handlers.go:343\] Session not found.

2021-10-12T08:35:40.382Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Authentication failed.

2021-10-12T08:35:40.382Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Internal server error occured on authorization: com.vmware.vapi.std.errors.unauthenticated

2021-10-12T08:36:13.094Z info vlcm \[auth/handlers.go:662\] Received UI Plugin Authentication request with hashed session id \'2998dcee56f5dc777f8c046844a35c99633ae8fdcd60ea448511366fedb861cf\'

2021-10-12T08:36:13.095Z error vlcm \[plugin/session.go:79\] No session found

 

 

 

[https://kb.vmware.com/s/article/85962?lang=en_US \[kb.vmware.com\]](https://urldefense.com/v3/__https:/kb.vmware.com/s/article/85962?lang=en_US__;!!LpKI!1o5kVVyqFxBBSjvO5PPAVjCs38YRBuN64Nk7-u5tXKDSHJfJcj6kDtAVH1p8ww$)

 

目前在

 

关于VLCM的更新页面显示"身份验证失败，无法访问Lifecycle Manager服务器"的问题，通过检查和确认，

这是vCenter appliance 7.0 u3的一个Bug，目前官方开发那边在调查分析中，还没有直接的解决方案，

如果使用更新功能可以换用sso admin账户登录。我会关注该KB的进展，如果颁布的解决方案，会及时提供给您解决办法。

 

2021-10-12T08:29:22.04Z info vlcm \[auth/session.go:143\] Scheduling session cleanup in 23h45m49.959942124s

2021-10-12T08:33:54.702Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Security Context missing in the request

2021-10-12T08:33:54.702Z info vlcm \[auth/session.go:199\] Session not found

2021-10-12T08:33:54.702Z error vlcm \[auth/handlers.go:343\] Session not found.

2021-10-12T08:33:54.702Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Authentication failed.

2021-10-12T08:33:54.702Z info vlcm \[logger/teelogger.go:54\] \[opID=vapi\] Message formatter created for en, en, UTC, 2, SHORT_DATE_TIME

2021-10-12T08:33:54.703Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Internal server error occured on authorization: com.vmware.vapi.std.errors.unauthenticated

2021-10-12T08:35:40.382Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Security Context missing in the request

2021-10-12T08:35:40.382Z info vlcm \[auth/session.go:199\] Session not found

2021-10-12T08:35:40.382Z error vlcm \[auth/handlers.go:343\] Session not found.

2021-10-12T08:35:40.382Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Authentication failed.

2021-10-12T08:35:40.382Z error vlcm \[logger/teelogger.go:33\] \[opID=vapi\] Internal server error occured on authorization: com.vmware.vapi.std.errors.unauthenticated

2021-10-12T08:36:13.094Z info vlcm \[auth/handlers.go:662\] Received UI Plugin Authentication request with hashed session id \'2998dcee56f5dc777f8c046844a35c99633ae8fdcd60ea448511366fedb861cf\'

2021-10-12T08:36:13.095Z error vlcm \[plugin/session.go:79\] No session found

 

 

 

目前在  vCenter 7.0 Update 3a. 版本上解决了此问题

 

[https://kb.vmware.com/s/article/85962?lang=en_US \[kb.vmware.com\]](https://urldefense.com/v3/__https:/kb.vmware.com/s/article/85962?lang=en_US__;!!LpKI!1o5kVVyqFxBBSjvO5PPAVjCs38YRBuN64Nk7-u5tXKDSHJfJcj6kDtAVH1p8ww$)

 

 

![[Technology_ALL_VMware_分析案例_132_关于VLCM错误_001.png]]

 

已使用 OneNote 创建。
