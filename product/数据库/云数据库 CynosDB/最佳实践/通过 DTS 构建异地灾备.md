﻿TDSQL-C MySQL 版支持通过 DTS 数据传输服务构建异地灾备流程，满足异地容灾，保障数据库平稳运行。
## 场景说明
如果您的 TDSQL-C MySQL 版集群部署在单个地域中，可能会因为断电、网络中断等不可抗因素而导致服务中断。

针对这种情况，您可以在另一个地域构建灾备中心，以提高服务可用性。 使用 DTS 数据传输服务可以在业务中心和灾备中心之间持续同步数据更新，并保持地域间副本同步。如果业务地域发生故障，您可以将用户请求切换到灾备地域。

通过构建异地灾备架构，当某个数据中心出现灾难性事故时，可以将发生异常的数据中心的流量划拨到其他数据中心，实现跨地域的快速故障转移，保证业务的正常运行。

![](https://qcloudimg.tencent-cloud.cn/raw/bdcb3cf36e74ce26da15b67a0090c7c5.png)

## 建立异地灾备流程
**步骤一：购买集群**
登录 [TDSQL-C MySQL 版购买页](https://buy.cloud.tencent.com/cynosdb?regionId=8) 分别在不同地域购买两套 TDSQL-C MySQL 版集群，其中一套集群为业务中心，另一套集群为灾备中心。
>?关于数据同步的源集群和目标集群的版本等环境要求，以及数据同步的前提条件，您可参见 [TDSQL-C MySQL 同步至 TDSQL-C MySQL](https://cloud.tencent.com/document/product/571/59962)。
>
**步骤二：使用 DTS 数据传输服务搭建异地灾备流程。**
1. 登录 [数据同步购买页](https://buy.cloud.tencent.com/replication)，选择相应配置，单击**立即购买**。
<table>
<thead><tr><th>参数</th><th>描述</th></tr></thead>
<tbody><tr>
<td>计费模式</td><td>支持包年包月和按量计费。</td></tr>
<tr>
<td>源实例类型</td><td>选择 TDSQL-C MySQL，购买后不可修改。</td></tr>
<tr>
<td>源实例地域</td><td>选择源实例所在地域，购买后不可修改。</td></tr>
<tr>
<td>目标实例类型</td><td>选择 TDSQL-C MySQL，购买后不可修改。</td></tr>
<tr>
<td>目标实例地域</td><td>选择目的实例所在地域，购买后不可修改。</td></tr>
<tr>
<td>规格</td><td>请根据业务诉求选择规格，规格越高，性能越好。详情请参考 <a href="https://cloud.tencent.com/document/product/571/18736">计费概述</a>。</td></tr>
</tbody></table>
2. 购买完成后，返回 [数据同步列表](https://console.cloud.tencent.com/dts/replication)，可看到刚创建的数据同步任务，刚创建的同步任务需要进行配置后才可以使用。
3. 在数据同步列表，单击**操作**列的**配置**，进入配置同步任务页面。
![](https://qcloudimg.tencent-cloud.cn/raw/2b43905d0cd301c675ecc695ee2baf1c.png)
4. 在配置同步任务页面，配置源端实例、帐号密码，配置目标端实例、帐号和密码，测试连通性后，单击**下一步**。
<img src="https://qcloudimg.tencent-cloud.cn/raw/dfd1dbb7e5612ea63c6dc2acc5517a03.png"  style="zoom:80%;">
<img src="https://qcloudimg.tencent-cloud.cn/raw/103254daaa98763531a80ffc5f9edbf6.png"  style="zoom:80%;">
<br>因源数据库部署形态和接入类型的交叉场景较多，各场景同步操作步骤类似，如下仅提供典型场景的配置示例，其他场景请用户参考配置。
<br>
TDSQL-C MySQL 同步至 TDSQL-C MySQL
<table>
<thead><tr><th width="10%">设置项</th><th width="15%">参数</th><th width="75%">描述</th></tr></thead>
<tbody><tr>
<td rowspan=2 >任务设置</td>
<td>任务名称</td>
<td>DTS 会自动生成一个任务名称，用户可以根据实际情况进行设置。</td></tr>
<tr>
<td>运行模式</td><td>支持立即执行和定时执行两种模式。</td></tr>
<tr>
<td rowspan=7 >源实例设置</td>
<td>源实例类型</td>
<td>购买时所选择的源实例类型，不可修改。</td></tr>
<tr>
<td>源实例地域</td>
<td>购买时选择的源实例所在地域，不可修改。</td></tr>
<tr>
<td>服务提供商</td>
<td>自建数据库（包括云服务器上的自建）或者腾讯云数据库，请选择“普通”；第三方云厂商数据库，请选择对应的服务商。<br>本场景选择“普通”。</td></tr>
<tr>
<td>接入类型</td>
<td>请根据您的场景选择，本场景选择“云数据库”，不同接入类型的准备工作请参考 <a href="https://cloud.tencent.com/document/product/571/59968">准备工作概述</a>。<ul>
<li>公网：源数据库可以通过公网 IP 访问。</li>
<li>云主机自建：源数据库部署在 <a href="https://cloud.tencent.com/document/product/213">腾讯云服务器 CVM</a> 上。</li>
<li>专线接入：源数据库可以通过 <a href="https://cloud.tencent.com/document/product/216">专线接入</a> 方式与腾讯云私有网络打通。</li>
<li>VPN接入：源数据库可以通过 <a href="https://cloud.tencent.com/document/product/554">VPN 连接</a> 方式与腾讯云私有网络打通。</li>
<li>云数据库：源数据库属于腾讯云数据库实例。</li>
<li>云联网：源数据库可以通过 <a href="https://cloud.tencent.com/document/product/877">云联网</a> 与腾讯云私有网络打通。</li><li>私有网络 VPC：源数据和目标数据库都部署在腾讯云上，且有 <a href="https://cloud.tencent.com/document/product/215">私有网络</a>。如果需要使用私用网络 VPC接入类型，请 <a href="https://console.cloud.tencent.com/workorder/category">提交工单</a> 申请。</li></ul></td></tr>
<tr>
<td>实例 ID</td><td>源实例 ID。可在 <a href="https://console.cloud.tencent.com/cynosdb/mysql/ap-guangzhou/cluster/cynosdbmysql-iimys9mv/detail">集群列表</a> 查看源实例信息。</td></tr>
<tr>
<td>帐号</td><td>源实例帐号，帐号权限需要满足要求。</td></tr>
<tr>
<td>密码</td><td>源实例帐号的密码。</td></tr>
<tr>
<td rowspan=6>目标实例设置</td>
<td>目标实例类型</td><td>购买时选择的目标实例类型，不可修改。</td></tr>
<tr>
<td>目标实例地域</td><td>购买时选择的目标实例地域，不可修改。</td></tr>
<tr>
<td>接入类型</td><td>根据您的场景选择，本场景选择“云数据库”。</td></tr>
<tr>
<td>实例 ID</td><td>选择目标实例 ID。</td></tr>
<tr>
<td>帐号</td><td>目标实例帐号，帐号权限需要满足要求。</td></tr>
<tr>
<td>密码</td><td>目标实例帐号的密码。</td></tr>
</tbody></table>

5. 在设置同步选项和同步对象页面，将对数据初始化选项、数据同步选项、同步对象选项进行设置，在设置完成后单击**保存并下一步**。
>?
>- 当**初始化类型**仅选择**全量数据初始化**，系统默认用户在目标库已经创建了表结构，不会进行表结构同步，也不会校验源库和目标库是否有同名表，所以当用户同时在**已存在同名表**中选择**前置校验并报错**，则校验并报错功能不生效。
>- 如果用户在同步过程中确定会对某张表使用 rename 操作（例如将 table A rename 为 table B），则**同步对象**需要选择 table A 所在的整个库（或者整个实例），不能仅选择 table A，否则系统会报错。
>
![](https://qcloudimg.tencent-cloud.cn/raw/a51e8d2f0a1cc2713c6e950f12f559e0.png)
<table>
<thead><tr><th>设置项</th><th>参数</th><th>描述</th></tr></thead>
<tbody>
<tr>
<td rowspan=2>数据初始化选项</td>
<td>初始化类型</td>
<td><ul><li>结构初始化：同步任务执行时会先将源实例中表结构初始化到目标实例中。<li>全量数据初始化：同步任务执行时会先将源实例中数据初始化到目标实例中。仅选择<b>全量数据初始化</b>的场景，用户需要提前在目标库创建好表结构。<br>默认两者都勾上，可根据实际情况取消。</td></tr>
<tr>
<td>已存在同名表</td>
<td><ul><li>前置校验并报错：存在同名表则报错，流程不再继续。<li>忽略并继续执行：全量数据和增量数据直接追加目标实例的表中。</td></tr>
<tr>
<td rowspan=2>数据同步选项</td>
<td>冲突处理机制</td>
<td><ul><li>冲突报错：在同步时发现表主键冲突，报错并暂停数据同步任务。<li>冲突忽略：在同步时发现表主键冲突，保留目标库主键记录。<li>冲突覆盖：在同步时发现表主键冲突，用源库主键记录覆盖目标库主键记录。</td></tr>
<tr>
<td>同步操作类型</td><td>支持操作：Insert、Update、Delete、DDL。勾选“DDL 自定义”，可以根据需要选择不同的 DDL 同步策略。详情请参考 <a href="https://cloud.tencent.com/document/product/571/63955">设置 SQL 过滤策略</a>。</td></tr>
<tr>
<td rowspan=3>同步对象选项</td>
<td>源实例库表对象</td><td>选择待同步的对象，支持基础库表、视图、存储过程和函数。<br>高级对象的同步是一次性动作，仅支持同步在任务启动前源库中已有的高级对象，在任务启动后，新增的高级对象不会同步到目标库中。更多详情，请参考 <a href="https://cloud.tencent.com/document/product/571/74612">同步高级对象</a>。</td></tr>
<tr>
<td>已选对象</td><td><ul><li>支持库表映射（库表重命名），将鼠标悬浮在库名、表名上即显示编辑按钮，单击后可在弹窗中填写新的名称。</li><li>选择高级对象进行同步时，建议不要进行库表重命名操作，否则可能会导致高级对象同步失败。</li></ul></td></tr>
<tr>
<td>是否同步 Online DDL 临时表</td>
<td>如果使用 gh-ost、pt-osc 工具对源库中的表执行 Online DDL 操作，DTS 支持将 Online DDL 变更产生的临时表迁移到目标库。<ul><li>勾选 gh-ost，DTS 会将 gh-ost 工具产生的临时表名（`_表名_ghc`、`_表名_gho`、`_表名_del`）迁移到目标库。</li>
<li>勾选 pt-osc， DTS 会将 pt-osc 工具产生的临时表名（`_表名_new`、 `_表名_old`）迁移到目标库。</li></ul>更多详情请参考 <a href="https://cloud.tencent.com/document/product/571/75890">同步 Online DDL 临时表</a>。</td></tr>
</tbody></table>
6. 在校验任务页面，完成校验并全部校验项通过后，单击**启动任务**。
    如果校验任务不通过，可以参考 [校验不通过处理方法](https://cloud.tencent.com/document/product/571/61639) 修复问题后重新发起校验任务。
 - 失败：表示校验项检查未通过，任务阻断，需要修复问题后重新执行校验任务。
 - 警告：表示检验项检查不完全符合要求，可以继续任务，但对业务有一定的影响，用户需要根据提示自行评估是忽略警告项还是修复问题再继续。
![](https://qcloudimg.tencent-cloud.cn/raw/b43706045f346b60ac31bba8b1c458b9.png)
7. 返回数据同步任务列表，任务开始进入**运行中**状态。
>?选择**操作**列的**更多** > **结束**可关闭同步任务，请您确保数据同步完成后再关闭任务。
>
8. （可选）您可以单击任务名，进入任务详情页，查看任务初始化状态和监控数据。

