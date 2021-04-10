# MikroTik-Phicomm-Backup
* [xueyun.club](http://xueyun.club) 免费机场
* [https://chinag.pro/](https://chinag.pro/) chinag
* [rerere.top](http://www.rerere.top/auth/register) 10GB ssr
* [jgjc.top](http://www.jgjc.top/auth/register) v2ray
* [6pan](https://alkt.lanzoui.com/iDWoxgdkowf) 6盘下载工具
* [v2ray客户端](https://tlanyan.me/v2ray-clients-download/)
* [merlinblog.xyz](https://merlinblog.xyz/wiki/freess.html)
* [EDCwifi](https://www.edcwifi.com.cn/resources) 资源教程
* [EDCwifi.com](https://download.edcwifi.com/index.php?title=MikroTik%E6%89%8B%E5%86%8C) edcwifi npk download
* [hap ac2 vs rt-ac86u](http://routerchart.com/compare/mikrotik-routerboard-hap-ac-rb962uigs-5hact2hnt-151,asus-rt-ac86u-rt-ac86u-369)
* [hap ac2.pdf](https://www.edcwifi.com.cn/project/afc_api/Public/Uploads/2019-10-17/5da816a82f565.pdf) RouterOS Wi-Fi
* [k2p_mtk.zip](https://www.mingjinglu.com/write/548.html)  k2p官改固件
* [bigwan-GF-PSG1218-K2-0.0.23-fix10-27782bd.bin](http://dl.geewan.com/ )   极玩k2固件
* [PSG1218.trx](https://github.com/hanwckf/rt-n56u/releases )  **斐讯k2 padavan 固件**
* HC5661-sysupgrade-20140911-95d8bc22-ssh [极路由官网修砖](http://www.hiwifi.com/service_faq?id=62&article_id=34)
* `router_bin_recover`   **tp_link路由器的备份配置文件bin**
* http://tftpd32.jounin.net/  tftp服务器
* `http://www.brendangregg.com/` Linux tutorial
* [LEDE 下载地址](http://firmware.koolshare.cn/LEDE_X64_fw867/)
* [RouterPassView](https://www.nirsoft.net/utils/router_password_recovery.html)
* [koolss_2.2.2_x64.tar.gz openwrt](https://github.com/codewindy/Mikrotik-Phicomm-Backup/blob/master/koolss_2.2.2_x64.tar.gz) 离线安装 
* [v2ray_2.3.7_x64.tar.gz openwrt](https://github.com/codewindy/Mikrotik-Phicomm-Backup/blob/master/v2ray_2.3.7_x64.tar.gz) 离线安装
![2020-05-18_211705v2ray.png](https://i.loli.net/2020/05/18/EWYZBStAOx9wkDi.png)
# hap ac2 optimized
* [RBD52G-5HacD2HnD](https://codewindy.github.io/2020/04/18/RouterOS-Optimized/) `RouterOS 无线优化和常用配置`

*  优化hap ac2无线参数
*  **`C- is center of frequency ` `e - is extension channel `**  example : frequency is 5100 and in eCee will be see (5080-e,5100-C,5120-e,5140-e)
* fragmentation-threshold命令用来配置指定射频模板中的报文分段门限参数。缺省情况下，**`报文分段门限参数为2346Byte`**。应用场景配置合理的报文分段门限参数可以提高信道带宽的利用率。报文分段门限的设置需要用户根据实际情况进行选择，根据目前的发展趋势，建议用户采用较大值的门限。当报文分段门限设置过小时，报文就被分为多段传输，而在无线传输中，每传送一次都有较大的额外开销，因此信道利用率低,当报文分段门限设置过大时，长报文就不容易被分段，导致传输的时间长，出错的概率大，而一旦出错就要重传，因此会造成信道带宽的浪费。
* ts-cts模式：当AP向某个客户端发送数据的时候，AP会向客户端发送一个RTS报文，这样在AP覆盖范围内的所有设备在收到RTS后都会在指定的时间内不发送数据。目的客户端收到RTS后，发送一个CTS报文，这样在该客户端覆盖范围内所有的设备都会在指定的时间内不发送数据。使用rts-cts方式实现冲突避免需要发送两个报文，报文开销较大。
  ```shell
    /interface wireless
    set [ find default-name=wlan1 ] country=china mode=ap-bridge ssid=tpy wireless-protocol=802.11
    /interface wireless security-profiles
    set [ find default=yes ] authentication-types=wpa2-psk eap-methods="" group-key-update=1h mode=dynamic-keys supplicant-identity=MikroTik \
        wpa-pre-shared-key=a1234567 wpa2-pre-shared-key=a1234567
    add authentication-types=wpa2-psk disable-pmkid=yes eap-methods="" group-key-update=1h mode=dynamic-keys name=eda supplicant-identity="" \
        wpa-pre-shared-key=a1234567 wpa2-pre-shared-key=a1234567
    /interface wireless
    set [ find default-name=wlan2 ] adaptive-noise-immunity=ap-and-client-mode band=5ghz-onlyac channel-width=20/40/80mhz-eeCe country=malaysia disabled=no \
        distance=indoors frequency=5300 hw-fragmentation-threshold=2346 hw-protection-mode=rts-cts installation=indoor keepalive-frames=disabled mode=\
        ap-bridge multicast-buffering=disabled multicast-helper=full security-profile=eda ssid=eda wireless-protocol=802.11 wps-mode=disabled
    /interface wireless nstreme
    set wlan2 enable-polling=no
  ```
## hotspot config
* `wifite_deauth.zip` **修改源码实现扫描附近Wi-Fi并deauth已>连接的client配合RouterOS的hotspot镜像相同的ssid并开启packet sniffer来抓取密码**
* ![IMG_9219 _1_.png](https://i.loli.net/2020/08/31/zO68KxwlGdZaSyi.png)
* 客户端连接到hotspot后根据上图的提示输入Wi-Fi密码，此时已经配置好抓包的RouterOS可以获取到明文密码，通过导出到wireshark并在其中搜索`http contains POST`即可快速定位到该提交的密码数据包  
## 伪造PPPOE服务器来恢复宽带账号密码
* [RouterOS_PPPOE_PacketSniffer](https://codewindy.github.io/2018/05/01/RouterOS_PPPOE_PacketSniffer/)
