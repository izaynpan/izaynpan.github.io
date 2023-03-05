---
title: N1盒子部署Yunzai-Bot的总结
tag: 折腾
date: 2022-10-17
---

## 前言

看到一个qq群里有只yunzai机器人，想着绑定下自己的账号查查数据体验一下，结果那只机器人迟迟没反应，于是我想就自己也搭一个吧，正好自己拉个群给朋友们玩玩。于是目光锁定了那台近乎吃灰的斐讯N1盒子。

## N1盒子重刷Armbian系统

我的N1盒子此前一直安装着Openwrt系统当作一个旁路网关用的，考虑到Openwrt系统精简了不少依赖和指令，为减少后续部署Yunzai-Bot可能会出现的问题，最终选择将N1重刷成Armbian系统。经实测Openwrt重刷Armbian不需刷回到之前的安卓系统，直接覆盖写入emmc即可。 该注意的地方这篇[博客](https://yuerblog.cc/2019/10/23/%E6%96%90%E8%AE%AFn1-%E5%AE%8C%E7%BE%8E%E5%88%B7%E6%9C%BAarmbian%E6%95%99%E7%A8%8B/)都提到了，感谢那位博主帮我避了那么多坑。

## 安装Docker遇到的问题

安装Docker：第一次尝试使用官方的安装脚本`get-docker.sh`，但提示我Debian版本(armbian5.77, debian9)过低，建议使用高版本进行安装，不出意外地安装失败了，于是使用 `apt install docker-ce `进行安装，执行`docker --version`结果`Docker version 19.03.15, build 99e3ed8`，安装成功。

安装docker-compose的坑：开始时看一些帖子推荐使用pip安装docker-compose，我便照做，结果安装过程中疯狂出现依赖问题，踩坑😐。继续查找资料有帖子说尝试换用高版本python，于是下载了python3.9的源码进行编译升级([参考](https://my.oschina.net/ranvane/blog/5559922))，继续使用pip安装，看似安装成功了，但无法使用，踩坑😐。最后选择直接下载官方的可执行文件，参考了这个[帖子](https://tataramoriko.com/index.php/wkyuu/149.html)安装1.29.2版本，下载的docker-compose可执行文件才9byte，显然是有问题的，踩坑😐。执行`uname -s`结果`Linux`，执行`uname -m`结果`aarch64`，上[官方GitHub发行页面](https://github.com/docker/compose/releases)才发现1.29.2版本没有linux-aarch64的版本，而目前最新版2.12.0支持，于是从[官方文档](https://docs.docker.com/compose/install/other/)上拷贝了`curl -SL https://github.com/docker/compose/releases/download/v2.12.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose`，下载成功。设置下权限`chmod +x /usr/local/bin/docker-compose`，执行`docker-compose --version`结果`Docker Compose version v2.11.2`，安装成功。（以后装啥之前都尽量上官方文档瞅一眼，装2.12.0的时候甚至还踩了个坑，`v2.12.0`少输了前面的`v`，因为下载低版本的链接是没有那个`v`的，然后就下载失败了😐）(**2023-3-3补充：直接在`Assets`里右键复制链接地址问题就解决了。。**)

## 部署过程中遇到的问题

第一次尝试手动部署，yunzai-bot容器里出现了redis disconnted的问题，提供的解决方法是启动redis，但redis在yunzai-redis容器里是已经启动的状态。后来改用一键部署脚本部署并没出现问题。开始时怀疑是私自改了redis文件夹在宿主机的位置造成的，但查看一键部署脚本的[内容](https://jihulab.com/Xm798/Yunzai-Bot-Docs/-/raw/master/scripts/install_v3.sh)，发现了`sed -i 's|127.0.0.1|redis|g' ./yunzai/config/redis.yaml`这行命令，将宿主机`redis.yaml`这个文件里的`host: 127.0.0.1`改成了`host: redis`，以便从外部访问，我猜原因应该就出在这。

## 使用Yunzai-Bot遇到的一些问题（持续更新）

更换机器人qq号：`./yunzai/config/qq.yaml`里更改

登录问题.[Yunzai-Bot#679](https://github.com/Le-niao/Yunzai-Bot/issues/679) + [云崽机器人总结问题六](https://docs.qq.com/doc/DT25kQ0x3VVlyc0Jz)

自带的三个插件加载错误：重新下载文件解决.[Yunzai-Bot#17](https://github.com/SirlyDreamer/Yunzai-Bot/issues/17)

原神语音合成插件错误：原神语音api关闭.[YunzaiJsPlugins#3](https://github.com/yhArcadia/YunzaiJsPlugins/issues/3) + [云崽机器人总结问题九](https://docs.qq.com/doc/DT25kQ0x3VVlyc0Jz)

逍遥插件的签到问题.[Yunzai-Bot#729](https://github.com/Le-niao/Yunzai-Bot/issues/729) + 搭配云崽自身的配置文件（或者`#开启/关闭签到`）.[Yunzai-Bot#542](https://github.com/Le-niao/Yunzai-Bot/issues/542)

逍遥插件的图鉴问题.[xiaoyao-cvs-plugin#16](https://github.com/ctrlcvs/xiaoyao-cvs-plugin/issues/16)

Redis容器AOF数据持久化问题.[解决方案](https://github.com/SirlyDreamer/Yunzai-Bot/issues/23)

部分插件的功能绑定cookie后需要`#重启`才能生效.[Yunzai-Bot#569](https://github.com/Le-niao/Yunzai-Bot/issues/569)

更新面版失败问题：更换请求url为国内中转地址.[miao-plugin#63](https://github.com/yoimiya-kokomi/miao-plugin/issues/63)



PS：之前用的Openwrt整合固件貌似有后门😑，详见[帖子](https://muguang.me/guff/2801.html)。现在开源项目都要留个心眼，非开源项目更要慎用了。

2022-10-23更新：N1的8G储存几乎被吃完了，继续添加其他插件是不太可能了，赶着双十一优惠，购入了一台腾讯云轻量应用服务器，2核4G、6M带宽1000G月流量、60G固态盘，首年100RMB。直接使用时雨大佬的一键部署[脚本](https://gitee.com/TimeRainStarSky/TRSS_Yunzai)，部署在这台服务器上，十几分钟就解决战斗了😂。鉴于还是有全局翻墙的需求，大概会在N1的Docker里部署个Openwrt。