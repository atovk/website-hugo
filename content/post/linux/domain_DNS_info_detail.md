---
title: "域名解析类型详解 / Domain DNS info detail"
date: 2019-09-13T02:04:31+08:00
draft: false
tags: ["DNS", "domain"]
categories: ["DNS"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---

> 域名解析类型详解

## 1.主机[A]记录

- 描述: 主机地址记录。在 DNS 域名与 IP 地址之间建立映射关系

- 语法:  owner class ttl A IP_v4_address

- 例子: host1.example.microsoft.com. IN A 127.0.0.1

## 2.别名[CNAME]

- 描述: 用来表示用在该区域中的其它资源记录类型中已指定名称的替补或别名 DNS 域名。

- 语法: owner ttl class AFSDB subtype server_host_name 

- 例子: aliasname.example.microsoft.com. AFSDB 1 truename.example.microsoft.com.

## 3.主机信息[HINFO]

- 描述: 用来说明映射到特定 DNS 主机名的 CPU 类型和操作系统类型的 RFC-1700 保留字符串类型，这个信息可以被应用程序通信协议使用。

- 语法: owner ttl class HINFO cpu_type os_type 

- 例子: my-computer-name.example.microsoft.com. HINFO INTEL-386 WIN32

## 4.邮箱[MB]

- 描述: 用来将指定的域邮箱名映射到这个邮箱的主机的当前区域中的主机地址记录

- 语法: owner ttl class MB mailbox_hostname 

- 例子: mailbox.example.microsoft.com. MB mailhost1.example.microsoft.com

## 5.邮箱或通信信息 MINFO

- 描述: 用来指定负责维护该记录中特定通信名单或邮箱的联系域邮箱名称。同时，还被用来指定接收与该记录中特定通信名单或邮箱有关的错误信息的邮箱

- 语法: owner ttl class MINFO responsible_mailbox error_mailbox 

- 例子: administrator.example.microsoft.com. MINFO resp-mbox.example.microsoft.com err-mbox.example.microsoft.com

## 6.邮件交换器 [MX]

- 描述: 用来向特定邮件交换器提供消息路由，该主机作为指定 DNS 域名的邮件交换器。MX 记录需要一个16-位整数来表示消息路由中的主机优先级，多个邮件交换在消息一中被指定。对于这个记录类型中的每个邮件交换主机，需要一个相应的主机地址类型记录。

- 语法: owner ttl class MX preference mail_exchanger_host 

- 例子: example.microsoft.com. MX 10 mailserver1.example.microsoft.com

## 7.指针记录 [PTR]

- 描述:用来指向域名空间中的某个位置。PTR记录通常在特殊域中来执行地址到名称镜像的反向搜索。每个记录提供要指向域名称空间的某个其它位置的简单数据。 

- 语法: owner ttl class PTR targeted_domain_name 

- 例子: 1.0.0.10.in-addr.arpa. PTR host.example.microsoft.com.

## 8.服务记录 [SRV]

- 描述: SRV 资源记录允许管理员使用单一 DNS 域的多个服务器，容易的用管理功能将 TCP/IP 服务从一个主机移到另一个主机，并且将服务提供的程序主机分派为服务的主服务器，将其它的分派为辅助

- 语法: service.protocol.name ttl class SRV preference weight port target 

- 例子: ldap.tcp.ms-dcs SRV 0 0 389 dc1.example.microsoft.com SRV 10 0 389 dc2.example.microsoft.com

## 9.已知服务记录 [WKS]

- 描述: 用来描述一个特定 IP 地址上特定通讯协议支持的 TCP/IP 服务，它提供 TCP 和 UDP 可使用性信息。如果服务器同时支持 TCP 和 UDP 的已知服务，或者有多个支持服务的 IP 地址，多个 WKS 记录会被使用

- 语法: owner ttl class WKS address protocol service_list 

- 例子: example.microsoft.com. WKS 10.0.0.1 TCP ( telnet smtp ftp ) 　　在"起始颁发机构" SOA 中，记录了这个 Zone 中 DNS 服务器是那一台主机，也记录着负责本 zone 的管理员的邮件地址，如果以后在安装邮件服务器需要修改该信息时，注意将邮件地址中的 "@" 符改为句点 "." ，因为 "@" 是保留字，代表 zone；另外，要使用域完整名称 FQDN，不要漏掉最后的句点。可以通过 "zone→属性→起始颁发机构"对管理员邮件地址进行修改。
