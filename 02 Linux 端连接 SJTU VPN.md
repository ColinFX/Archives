# Linux 端连接 SJTU VPN

本文阐述如何在 Linux 端通过 strongswan 安装、配置并使用 SJTU VPN。

本地系统：Ubuntu 18.04.6 LTS

快照日期：2022年3月20日

## 安装和配置

1. 点开任务栏下端 Show Applications，直接键入搜索 `Software & Updates`，在 Ubuntu Software 选项卡中勾选 Downloadable from the Internet 下所有选项。

2. `colinfx@local:~$ sudo apt install --upgrade strongswan strongswan-swanctl`

3. `colinfx@local:~$ sudo apt install libstrongswan-extra-plugins libcharon-extra-plugins`

4. `colinfx@local:~$ sudo apt install resolvconf curl`

5. `colinfx@local:~$ ln -s /etc/ssl/certs/* /etc/ipsec.d/cacerts/`

6. `colinfx@local:~$ sudo nano /etc/ipsec.conf`

7. 在文件中加入以下内容：

    ```
    conn "sjtu-student"
        keyexchange=ikev2
        left=%config
        leftsourceip=%config4,%config6
        leftauth=eap-peap
        right=stu.vpn.sjtu.edu.cn
        rightid=@stu.vpn.sjtu.edu.cn
        rightsubnet=0.0.0.0/0,2000::/3
        rightauth=pubkey
        eap_identity="ColinFX"
        auto=add
        aaa_identity="@radius.net.sjtu.edu.cn"
    conn "sjtu-staff"
        keyexchange=ikev2
        left=%config
        leftsourceip=%config4,%config6
        leftauth=eap-peap
        ike=aes256-sha1-modp1024,3des-sha1-modp1024!
        esp=aes128-sha2_256-modp1024,3des-sha1-modp1024!
        right=vpn.sjtu.edu.cn
        rightid=%any
        rightsubnet=0.0.0.0/0,2000::/3
        rightauth=pubkey
        eap_identity="ColinFX"
        auto=add
        aaa_identity="@radius.net.sjtu.edu.cn"
    ```
    
    注意修改两处 `eap_identity=` 后的 jaccount 账户名称。

8. `colinfx@local:~$ sudo nano /etc/ipsec.secrets`

9. 在文件中加入以下内容：

    ```
    "ColinFX" : EAP "******"
    ```
    
    注意修改 jaccount 的账户名称和密码。
    
10. `colinfx@local:~$ sudo ipsec rereadall`

## 使用

连接 VPN：`colinfx@local:~$ sudo ipsec up "sjtu-student"`

断开 VPN：`colinfx@local:~$ sudo ipsec down "sjtu-student"`

## 参考说明

https://net.sjtu.edu.cn/info/1200/2665.htm
