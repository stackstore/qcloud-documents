CDS-Audit 部署的核心目标是把 Agent 安装到数据库服务器中，并且确保数据库服务器与 CDS-Audit 审计实例能实现网络连通，实现该目标需要进行 [VPC 配置](https://cloud.tencent.com/document/product/856/17384) 和 Agent 部署。
Agent 部署流程如下图所示，其中前五步为参数配置操作：
![0](https://main.qcloudimg.com/raw/a8ad2c956f3d8f53b55df06b56749e9c.png)
下面将为您详细介绍。
## 设置审计服务 IP
1. 通过 sysadmin 账号登录数据安全审计管理页面后，单击【Agent 配置】>【部署 Agent】进入 编辑 Agent 配置页面。
 ![1](https://main.qcloudimg.com/raw/18d171dd3d89c9deb04c8c0af53c6328.png)
2. 审计服务 IP，指定了 Agent 数据安全审计实例的 IP 地址。
 - 如您仅有一个数据安全审计且与被审计数据库在同一个地域，则只填写数据安全审计实例内网 IP 地址即可。
 - 如您有多个数据安全审计实例，请设置与被审计数据库同一地域的数据安全审计实例内网 IP。我们不推荐您在设置 Agent 连接 IP 时选择数据安全审计实例公网 IP，如不得不进行此项配置时，请与我们工作人员联系。
 
 ![2](https://main.qcloudimg.com/raw/9508d999ed8b7b25f5769db0181e7dad.png)
 
## 设置数据库信息
数据库信息帮助 Agent 准确识别流量中与数据库访问相关的会话，数据库信息提供 IP 列表和自动检测两种输入方式。
1. IP 列表
IP 列表需要管理员输入完整的数据库 IP 和开发端口，则 Agent 无论安装在数据库服务器还是应用服务器上，都能够准确定位数据库访问流量。
>**注意：**
>Agent 将只审计填入的 IP 和端口，若漏填 IP 将导致审计日志缺失。

 ![3](https://main.qcloudimg.com/raw/3ec27db0e6afa6e5fc9b0361df7d07b1.png)
2. 自动检查
Agent 若安装在数据库服务器上，可以自动遍历数据库服务器网卡，根据端口来判断开放了数据库服务的 IP ，以此定位 Agent 需要检测的 IP 流量。此类模式仅需填写数据库开放服务的端口，但是有两个限制条件：
 - Agent只能安装在数据库服务器上，
 - 数据库服务器上开放的数据库端口不能被其他应用占用。如下图所示，自动检查模式仅需填写数据库端口：

 ![4](https://main.qcloudimg.com/raw/18703f1fc19d7f9d059291da915da4d1.png)
 
## 设置审计 IP 范围
审计 IP 范围可设置 Agent 审计的源 IP 段，数盾数据安全审计支持全量审计和范围审计，若您希望部分 IP 段的访问操作不被记录，则可设置审计 IP 范围。默认审计 IP 范围为 0.0.0.0 – 255.255.255.255，即全网段审计。您可根据需求通过添加、删除、修改等操作进行设置。
![5](https://main.qcloudimg.com/raw/f6d97e1d2b24f5dae3adf25e3aa2d9cc.png)
## 设置停止审计阈值
被审计数据库若因性能导致濒临宕机时，关闭更多服务和进程能缓解宕机情况。因此，您可以设置一个基于 CPU 与内存使用率的阈值，当被审计数据库超过阈值时，Agent 将发送告警信息并停止工作，缓解数据库的性能压力。
**特别提醒：**
如您要求 Agent 任何情况都进行工作，可将负载检测开关关闭，Agent 将持续审计数据。
![6](https://main.qcloudimg.com/raw/bc4e60fe5c85232c44bedf97a6485f3a.png)
## 下载 Agent
所有配置完成后，单击【配置并下载 Agent】即可下载您设置好的 Agent 了。
## Agent 安装
下载 Agent 完成后，需要将 Agent 安装在相应服务器上才能实现审计效果。
- 如果您使用的是云服务器 + 自建数据库模式，则建议您将 Agent 安装在数据库服务器上。
- 如果您使用的是 TencentDB，则需要在连接数据库的应用服务器上安装 Agent。

安装步骤：
1. 将 CapAgent_xxx.zip 安装包上传到需要安装的机器上，如 /data 目录。
2. 使用`unzip CapAgent_xxx.zip`命令进行解压，得到 /data/CapAgent 目录。
3. 执行命令`chmod -R 755 CapAgent`。
4. 执行`cd CapAgent/script`，再执行`./install.sh`，结果如下。如未得到以下结果，请联系客服。
![](https://main.qcloudimg.com/raw/cbbbd91003cfad7da0cba7a84e9c8b56.png)

##  配置数据库实例
Agent 下载并正确安装后，审计数据即可回传，但若需在可视化报表和审计报告中查阅审计信息，还需配置数据库实例参数。
因同一业务应用可能涉及多台数据库服务器，我们将所属应用相同的数据库服务器归纳为一个实例，以方便您查看与检索审计信息。
1. 数据库实例配置由 **审计管理员** 操作，默认账户名为 useradmin（默认密码在购买时将通过站内信发送）。审计管理员登录后，在左侧导航栏单击【审计配置】>【实例配置】进入实例配置页面。
 ![7](https://main.qcloudimg.com/raw/83c35dbc28aa0976775ed90049b07f23.png)
2. 单击【添加】，在新弹出的窗口中，填写实例名称（建议以业务应用名称为基础命名）、数据库 IP、数据库端口、备注信息填写后，单击【确定】即可完成新增。
![8](https://main.qcloudimg.com/raw/d7a5bba4bd9abca4c273f6bcf162aea4.png)
实例配置完毕后，统计报表中会显示实例相关审计图，您即可充分使用产品功能。
