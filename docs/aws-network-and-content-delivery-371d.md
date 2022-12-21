# AWS 网络和内容交付

> 原文：<https://dev.to/vikashagrawal/aws-network-and-content-delivery-371d>

# VPC(虚拟私有云)

这是虚拟专用数据中心，我们在其中配置子网、az 和访问控制列表。当我们创建 AWS 帐户时，每个地区都会创建默认的 VPC。
安全是任何环境中最重要的方面，因此当我们创建 VPC 时，它会创建网络访问控制列表和安全组。

每个地区有 5 个 VPC 的限制，但可以通过请求支持中心来增加。
“VPC 向导”提供以下配置:

```
o   VPC with a Single Public Subnet
o   VPC with Public and Private Subnets
o   VPC with Public and Private Subnets and Hardware VPN Access
o   VPC with a Private Subnet Only and Hardware VPN Access 
```

安全组跨越多个 AZ
VPC 对等:

```
o   Allows you to connect a subnet in 1 VPC to another subnet in another VPC.
o   There peering happens through private IP address as if all are on the same private subnet.
o   Peering can happen with VPC in other AWS account.
o   Peering happens within the single region.
o   For Peering to happen, there shouldn’t be any CIDR block overlapping or matching, e.g. VPC ‘A’ with IP 10.0.0.0/16 can’t peer with VPC ‘B’ with IP 10.0.0.0/24.
o   No transitive peering, means in the below diagram B cannot talk to C, for the talk between B and C to happen either it has to be done through ‘A’ or by peering between B and C 
```

网络访问控制列表

```
o   Custom Network Access Control List denies everything by default while default NACL and Security Group allows everything by default.
o   NACL is stateless (means any inbound traffic rule is not allowed by default in outbound) while SG is stateful.
o   1 subnet can be associated with only 1 NACL but 1 NACL can have multiple subnets. Let's say 1 NACL has deny rule for 1 particular traffic and other NACL has allow rule for the same traffic on the same rule number, then there will be conflict if these 2 are attached to the same subnet.
o   NACL is evaluated in numerical order as defined in “Rule#”, so if Rule# 99 has HTTP(8) with deny from my PC IP and Rule# 100 has HTTP(8) with allow from all then access to the associated subnet wouldn’t be allowed while if Rule# 101 has HTTP(8) with deny from my PC IP and Rule# 100 has HTTP(8) with allow from all then access to the associated subnet would be allowed. 
```

子网

```
o   With VPC, you can create public facing subnet and have your EC2 instances deployed, which has got the web application and at the same time, you can define a private subnet, where you can deploy your DB.
o   Any subnet created will be private by default.
o   Whenever we create the subnet, it gives 5 less than the expected CIDR IP machines. 
```

互联网网关只能连接到 1 个 VPC，每个 VPC 只能有 1 个 IG，因此 IG 和 VPC 之间是一对一的映射。路由表目标应该指向 IG，如果有多个 IG，那么它可能会导致混乱，如果 IG 是共享的，那么它是一个安全漏洞。在给定的自定义 VPC 中创建 ELB 时，每个 AZ 应该只有 1 个子网，并且应该至少有 2 个 AZ 的子网。
·VPC 流量日志:

```
o   It captures the information about the IP traffic going to and from N/W interfaces in your VPC.
o   It can be viewed in Cloud Watch logs.
o   It can be created in the 3 following levels:
 VPC
 Subnet
 N/W Interface
o   Flow logs for the VPC, which is peered to another VPC, can be enabled if both the VPC are in the same AWS A/C.
o   After the flow log is created, the configuration like IAM Role, Group etc can’t be changed.
o   Following traffic is not monitored:
 Traffic to and from 169.254.169.254 (For instance meta data)
 Traffic to the reserved IP address for the default VPC router.
 DHCP traffic
 Traffic generated by windows instance for Amazon Windows license Activation.
 Traffic generated by instances when they contact Amazon DNS server. 
```

NAT(网络地址转换)

```
o   While creating NAT instances, disable source/destinations checks on the instances.
o   It must be on the public subnet.
o   There must be a route of the private subnet to the NAT instances, in order for this to work.
o   The amount of traffic NAT instances can support, depends upon the instance size.
o   NAT instance in a public subnet in your VPC to enable instances in the private subnet to initiate outbound IPv4 traffic to the Internet or other AWS services, but prevent the instances from receiving inbound traffic initiated by someone on the Internet. 
```

# 云锋

CloudFront 有助于在更靠近用户的位置缓存媒体、网站，包括动态、静态、流式和交互式内容。
“边缘位置”是缓存内容的位置。
边缘位置也用于写入
对象被缓存用于 TTL
清除缓存将会收费。
CDN 将分发来自 S3、EC2、Elastic load balancer、Route53
等多个来源的内容。当您创建 web 分发时，您需要指定 CloudFront 向何处发送其分发到边缘位置的文件请求。CloudFront 支持以下来源:

```
o   Amazon S3 buckets
o   HTTP or web servers hosted in EC2 or in your private web servers 
```

当您在 CloudFront 中创建或更新分发时，您可以添加一个原始访问身份(OAI)并自动更新 bucket 策略，以授予原始访问身份访问您的 bucket 的权限。或者，您可以选择手动更改存储桶策略或更改 ACL，ACL 控制存储桶中各个对象的权限。通过这个你可以从互联网上访问 S3 的私人内容。
亚马逊 CloudFront 只接受格式良好的连接，以防止许多常见的 DDoS 攻击，如 SYN floods 和 UDP 反射攻击到达您的源。DDoS 攻击在地理上被隔离在靠近攻击源的地方，这可以防止流量影响其他位置。
·分布是边缘位置的集合。
第一个用户不会更快，但同一地理位置的下一个用户会更快。
通过“价格等级”选项，您可以指定要缓存的所有边缘位置。
术语

```
o   Web Distribution: websites
o   RTMP: Media Streaming 
```

# Route53

这是域名注册。
要获得已在其他注册服务商处注册的域名，您需要要求您的注册服务商将您域名的名称服务器更新为与 AWS 中托管区域相关联的服务器。
这是一项 DNS 服务，不提供虚拟主机应用。
DNS 用于将人类友好的域名转换为互联网协议(IP)地址。
计算机使用 IP 地址在网络上相互识别。
IP 地址有 IPv4 和 IPv6 两种形式。
IP v4 是 32 位字段，而 IPv6 是 128 位字段。
IP v4 是过去，而 IPv6 是现在和未来。
·互联网号码分配地址(IANA)负责存储和管理顶级域名(。com，in 等。)
注册服务商(GoDaddy、亚马逊等)。)负责在一个或多个域下分配域名。
·域名在互联网名称与数字地址分配机构(ICANN)的服务机构 InterNIC 注册，该机构负责确保互联网域名的唯一性。
每个域名都在 DB 注册，称为 WhoIS 数据库。
由于 DNS 在端口 53 运行，因此命名为 Route53。
“授权记录开始”( SOA)存储以下信息:

```
o   Name of the server that supplied the data
o   TTL for domain name to ip address list. 
```

一旦你输入网址，会发生什么

```
o   When you type address (e.g vikash.com), it looks for the IP address.
o   If ISP didn’t cache the IP address, it goes to .com.
o   On querying to .com, it provides following information:
 URL: Vikash.com
 TTL
 NS (Name Server): ns.awsdns.com
o   The NS provides “A” (Address) record.
 “A” record is used by computer to translate domain name to IP address. 
```

有 3 种类型的记录:

```
o   “A” Record
o   CNAME (Canonical Name) is a mapping of one domain name to another domain name. E.g. m.vikash.com <=>  mobile.vikash.com
o   Alias Record: it’s used to map domain names to ELB, Cloud Front Distribution or S3 buckets.
o   CNAME is not applicable for naked domain name (e.g. vikash.com), it’s applicable for (m.vikash.com), while Alias record is applicable for naked domain name. 
```

常见的 DNS 类型:

```
o   NS
o   SOA
o   A
o   CNAMES
o   MX Records
o   PTR Records 
```

路由策略

```
o   Simple Routing Policy
 A domain name can be assigned with multiple IP address (e.g. EC2 public IP address)
 In the runtime, it can pick any of the IP addresses (EC2 instance) randomly.
 It comes under ‘A’ record.
 Only one ‘A’ record can exist (but mapped to multiple IP addresses)
o   Weighted Routing Policy
 You can assign the weightage (%) to the record.
 It gets created as ‘A’ record.
 One record will have only 1 IP address and weightage.
 You can have multiple ‘A’ records.
o   Latency Routing Policy
 Based on the user geographic location, it picks the IP addresses.
 Each of the entry is meant for one Region.
 If there are multiple IPs (multiple EC2 instances), you can have multiple IP addresses in one record set.
 There can be more than 1 ‘A’ record set.
o   Failover Routing Policy
 There will be one primary, which will be configured as part of “Health Checks” under Route53.
 Then a passive entry will be configured.
 Here there will multiple ‘A’ records and each of ‘A’ record will have single IP.
 If the primary goes down, then passive will come into action.
o   Geolocation Routing Policy
 It selects server based on the geographic location of the user and these servers may have the local language of that geographic location based customers.
 Here there will multiple ‘A’ records.
 Here we select the location not the region while creating ‘A’ record sets.
o   Multi Value Routing
 It's very similar to Simple Routing Policy
 Here it has multiple records and each record has one IP 
```

Route53 与 IAM
一样是全球性的，默认情况下有 50 个可用域名，但是这是一个软限制，可以通过联系 AWS 支持提出。

# API 网关

它可以将呼叫转移到 EC2 实例或 lambda。
您可以通过使用 API 缓存来提高 API 网关的性能，API 缓存中的响应将在定义为 TTL 的持续时间内存储。这意味着 API 将不会调用任何进一步的调用(如 EC2，lambda)
。跨源资源共享(CORS)支持从另一个域中的网页访问 API。
自动缩放。

直接连接
这是 AWS 和您的 DC、办公室等之间的直接连接。
提供私人连接。
它增加了可靠性和带宽。
与虚拟专用网不同，它不使用互联网。

DC 是一个漫长的过程，所以如果你有立即的需求，那么最好去 VPN。