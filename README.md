# oui([中文](/README_ZH.md))

[1]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=plastic
[2]: /LICENSE
[3]: https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=plastic
[4]: https://github.com/zhaojh329/oui/pulls
[5]: https://img.shields.io/badge/Issues-welcome-brightgreen.svg?style=plastic
[6]: https://github.com/zhaojh329/oui/issues/new
[7]: https://travis-ci.org/zhaojh329/oui.svg?branch=master
[8]: https://travis-ci.org/zhaojh329/oui

[![license][1]][2]
[![PRs Welcome][3]][4]
[![Issue Welcome][5]][6]
[![Build Status][7]][8]

[vue.js]: https://github.com/vuejs/vue
[element-ui]: https://github.com/ElemeFE/element
[LuCI2]: https://git.openwrt.org/?p=project/luci2/ui.git
[json-rpc]: https://www.jsonrpc.org/
[ubus]: https://wiki.openwrt.org/doc/techref/ubus
[uhttpd-mod-ubus]: https://wiki.openwrt.org/doc/techref/ubus#access_to_ubus_over_http

![](/screenshots.png)

OpenWrt web user interface implemented in [vue.js] and [element-ui], inspired by [LuCI2].

oui uses [json-rpc] to communicate with OpenWrt subsystems. Call [ubus] via [json-rpc].
To access any kind of system data through [ubus] with [json-rpc](with the help of [uhttpd-mod-ubus] to provide HTTP based API).

# TODO

* Dynamic switching theme
* Plugin

# [Donate](https://gitee.com/zhaojh329/oui#project-donate-overview)

* 深圳市云联芯科技有限公司 - 400¥
* 北京钛铂云老戴 - 200¥
* JunoSky - 100¥
* 许玉善(北京友联智诚科技有限公司) - 100¥
* lemon - 10¥

# Contributing
If you would like to help making [oui](https://github.com/zhaojh329/oui) better,
see the [CONTRIBUTING.md](/CONTRIBUTING.md) file.
