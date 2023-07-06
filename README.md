# 适配 emqx v4.4 版本

This is a template plugin for EMQ X version >= 4.3.

For development guide, see https://github.com/emqx/emqx/blob/main-v4.4/lib-extra/README.md

Plugin Config
-------------

Each plugin should have a 'etc/{plugin_name}.conf|config' file to store application config.

Authentication and ACL
----------------------

```
emqx:hook('client.authenticate', fun ?MODULE:on_client_authenticate/3, [Env]).
emqx:hook('client.check_acl', fun ?MODULE:on_client_check_acl/5, [Env]).
```

Build the EMQX broker
-----------------
###### 1. 基于CentOS7.x、OTP R24环境下编译，先启动编译环境
```
  docker run -d --name emqx-builder-centos-otp24  -p 1883:1883  -p 8083:8083  -p 8084:8084  -p 8883:8883 -p 18083:18083   ghcr.io/emqx/emqx-builder/4.4-24:24.3.4.2-1-el7  bash -c "tail -f /dev/null"
  
  # 进入容器
  docker exec -it emqx-builder-centos-otp24 bash
```

注：由于本插件引用的第三方依赖`ekaf`中使用了`pg2`模块，该模块在`OTP 24`及之后的版本已被官方移除，因此***请使用`OTP 24`以下的版本***。

###### 2. 下载EMQX源码

官方源码仓库地址为[emqx/emqx: An Open-Source, Cloud-Native, Distributed MQTT Message Broker for IoT. (github.com)](https://github.com/emqx/emqx) ，分支为`main-v4.4`

本人修改了官方的编译脚本，并且在插件目录里添加了该kafka插件的信息，仓库地址为[ULTRAKID/emqx at main-v4.3 (github.com)](https://github.com/weachy/emqx/tree/main-v4.4) ，分支为`main-v4.4`。

###### 3. 修改EMQX文件，增加kafka插件
参照[emqx/README.md at main-v4.4 · ULTRAKID/emqx (github.com)](https://github.com/weachy/emqx/blob/main-v4.4/lib-extra/README.md) 。

注：[ULTRAKID/emqx at main-v4.4 (github.com)](https://github.com/ULTRAKID/emqx/tree/main-v4.4) 仓库内已进行此项修改。

###### 4. 编译EMQX，并且启动EMQX

进入emqx目录，执行make命令，需要保持外网通畅，有条件建议科学上网。

二进制编译命令：`make`

docker镜像打包：`make emqx-docker`

Plugin and Hooks
-----------------

[Plugin Development](https://docs.emqx.io/en/broker/v4.3/advanced/plugins.html#plugin-development)

[EMQ X Hook points](https://docs.emqx.io/en/broker/v4.3/advanced/hooks.html)

License
-------

Apache License Version 2.0

Author
------

EMQ X Team.
