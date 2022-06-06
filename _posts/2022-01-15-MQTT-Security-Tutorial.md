---
layout: post
title: 怎么连接上具有TLS/SSL协议的MQTTS服务器
categories: [blog ]
tags: [Tutorial ]
description: "基于Arduino与ESP32讲解如何连接MQTTS服务器"
---
# 前言
***作者水平有限，内容可能存在不正确的地方，欢迎批评指正。**

最近在做项目开发的时候需要用到MQTT，结果在连接的时候发现无论如何也连接不上。

于是仔细研究了一下文档，发现我要连接的服务器居然还有一层TLS/SSL协议，导致了连接失败。

在查阅了相关资料后，发现中文的相关资料较少，于是乎写下这篇文章以分享。



# 什么是MQTTS

据我理解，类似于HTTPS，MQTTS也是在MQTT协议的基础上添加了TLS/SSL加密层对传输内容进行加密，避免明文传输时容易发生信息被窃取的情况。



# Arduino-ESP32实现MQTTS连接

在Arduino的ESP32硬件库中，已经支持了TLS/SSL协议的连接，于是实现MQTTS仅需要添加MQTT库即可。在这里我使用的是pubsubclient库，版本是2.8。



创建用于TLS/SSL连接的客户端与用于MQTT连接的对象：

```C++
#include <WiFi.h>
#include <WiFiClientSecure.h>
#include "PubSubClient.h"

WiFiClientSecure espClient;
PubSubClient client(espClient);
```



进行WIFI连接：

```c++
// Set Esp32 to WIFI mode
WiFi.mode(WIFI_STA);
// Connect To the WIFI
WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
```



设置CA证书（这里演示的CA证书是涂鸦服务器中国区的根证书）：

```C++
// root CA for mqtt Server
const char* tuya_root_ca= \
    "-----BEGIN CERTIFICATE-----\n" \
    "MIIDxTCCAq2gAwIBAgIBADANBgkqhkiG9w0BAQsFADCBgzELMAkGA1UEBhMCVVMx\n" \
    "EDAOBgNVBAgTB0FyaXpvbmExEzARBgNVBAcTClNjb3R0c2RhbGUxGjAYBgNVBAoT\n" \
    "EUdvRGFkZHkuY29tLCBJbmMuMTEwLwYDVQQDEyhHbyBEYWRkeSBSb290IENlcnRp\n" \
    "ZmljYXRlIEF1dGhvcml0eSAtIEcyMB4XDTA5MDkwMTAwMDAwMFoXDTM3MTIzMTIz\n" \
    "NTk1OVowgYMxCzAJBgNVBAYTAlVTMRAwDgYDVQQIEwdBcml6b25hMRMwEQYDVQQH\n" \
    "EwpTY290dHNkYWxlMRowGAYDVQQKExFHb0RhZGR5LmNvbSwgSW5jLjExMC8GA1UE\n" \
    "AxMoR28gRGFkZHkgUm9vdCBDZXJ0aWZpY2F0ZSBBdXRob3JpdHkgLSBHMjCCASIw\n" \
    "DQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAL9xYgjx+lk09xvJGKP3gElY6SKD\n" \
    "E6bFIEMBO4Tx5oVJnyfq9oQbTqC023CYxzIBsQU+B07u9PpPL1kwIuerGVZr4oAH\n" \
    "/PMWdYA5UXvl+TW2dE6pjYIT5LY/qQOD+qK+ihVqf94Lw7YZFAXK6sOoBJQ7Rnwy\n" \
    "DfMAZiLIjWltNowRGLfTshxgtDj6AozO091GB94KPutdfMh8+7ArU6SSYmlRJQVh\n" \
    "GkSBjCypQ5Yj36w6gZoOKcUcqeldHraenjAKOc7xiID7S13MMuyFYkMlNAJWJwGR\n" \
    "tDtwKj9useiciAF9n9T521NtYJ2/LOdYq7hfRvzOxBsDPAnrSTFcaUaz4EcCAwEA\n" \
    "AaNCMEAwDwYDVR0TAQH/BAUwAwEB/zAOBgNVHQ8BAf8EBAMCAQYwHQYDVR0OBBYE\n" \
    "FDqahQcQZyi27/a9BUFuIMGU2g/eMA0GCSqGSIb3DQEBCwUAA4IBAQCZ21151fmX\n" \
    "WWcDYfF+OwYxdS2hII5PZYe096acvNjpL9DbWu7PdIxztDhC2gV7+AJ1uP2lsdeu\n" \
    "9tfeE8tTEH6KRtGX+rcuKxGrkLAngPnon1rpN5+r5N9ss4UXnT3ZJE95kTXWXwTr\n" \
    "gIOrmgIttRD02JDHBHNA7XIloKmf7J6raBKZV8aPEjoJpL1E/QYVN8Gb5DKj7Tjo\n" \
    "2GTzLH4U/ALqn83/B2gX2yKQOC16jdFU8WnjXzPKej17CuPKf1855eJ1usV2GDPO\n" \
    "LPAvTK33sefOT6jEm0pUBsV/fdUID+Ic/n4XuKxe9tQWskMJDE32p2u0mYRlynqI\n" \
    "4uJEvlz36hz1\n" \
    "-----END CERTIFICATE-----\n";

// Set the CA Certificant
espClient.setCACert(tuya_root_ca);
```



在设置完CA证书后，MQTT连接就与正常连接方式无异了，可按照pubsubclient的demo进行代码编写。

设置MQTT服务器信息并连接（同样是涂鸦的MQTT服务器，用户名密码和设备ID我就不放出来啦）：

```C++
#define MQTT_BROKER "m1.tuyacn.com"
#define MQTT_PORT 8883

// Set properties of MQTT
client.setServer(MQTT_BROKER, MQTT_PORT);
client.setCallback(MQTTCallback);
// Connect to MQTT Server
client.connect(MQTT_DEVICE_ID, MQTT_USERNAME, MQTT_PASSWORD);
```

至此，已经成功连接上MQTTS服务器。



# 如何获取MQTTS服务器的CA证书？

如果MQTTS服务器提供商在手册上没有写明的话（比如涂鸦），可以尝试使用服务商Web官网的CA根证书。因为TLS/SSL客户端在验证服务器证书的时候会逐级向上验证证书，直到查询到符合条件的证书才会建立连接。如果我们这里使用根证书（最上级）的话，覆盖的范围会更大一些。

比如我在这里使用的是涂鸦的物联网服务器，但是死活找不到CA证书。于是我打开了涂鸦的官网（使用Chrome浏览器），看到了网址栏左边锁头了吗？表示这个网站使用了HTTPS。

右键这个锁头>点击“这个连接是安全的”>点击“证书有效”即打开了这个网站的证书。

点击证书路径选项卡，可以看到证书的层级。双击最上面的证书就打开了这个网站的根CA证书。

点击详细信息选项卡，点击复制到文件，在格式选择中选择"Base64编码X.509"，到处成功后用记事本打开.cer文件就可以获取到证书啦！

