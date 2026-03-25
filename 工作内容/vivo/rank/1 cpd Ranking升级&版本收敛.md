### 背景

cpd、游戏方向，经过常年的迭代，开发出了许多rank版本。线上迁移不完善，至今各个方向保留了许多不同版本的rank(2-6)。

### 问题

1  监控告警等也存在非常多。 常年得不到根治。一个需求在不同框架上重复开发多次，配置多个监控。一个主机磁盘或者内存有问题，发给了10几个rank告警

2 rank网卡带宽占用高，不支持单机多实例部署，CPU利用率不高；

3. rank命名不规范，需要清除历史无效的节点，干扰项过多；

4. 机器异构问题。当前链路机器存在A1、V5、V6等诸多机型机器，机器负载不均匀

### 解决方案

**方案一：**

|   |   |   |   |   |
|---|---|---|---|---|
|提出时间|模块|问题|现象|进展|
|2025.12.23|ranking|带宽占用高，服务从混部改成单独部署cpu利用率跑不起来|从22台混部物理机，改成5台容器部署，CPU利用率17%，网卡利用率60%+，p99 从120上涨到200ms。||

  

**方案二：**

1 将不同机型进行分割后，机器间会互相影响，负载低的时候，存在失败率。

![[Pasted image 20260325110737.png]]

**方案三：**

机器异构解决：
![[Pasted image 20260325110752.png]]
服务上线质量：
![[Pasted image 20260325110815.png]]
配置增加

![[Pasted image 20260325110959.png]]

cpc当前引入的abt来解决。内部自身分流。只是做工程链路上的检查

1 将mixer开启vns+wrr路由，解决机型异构问题，同时rank设置多条流水线，配置权重。

2 所有服务合并到一个服务，解决带宽和cpu利用率不高问题。

3 服务只保留如下，清理无效节点：

CPD：ranking-major, ranking-push，cpd-recall

搜索： ranking-search 

内销： ranking-inner

**rank、push、召回在线：**

|   |   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|---|
|业务|服务|场景|abt|rank版本|分支|目标版本|目标服务|负责人|排期|备注|
|CPD|rank5-701|701 首页|701|**rank5.5**|v5.5|ranking|ranking-marjor|黄志浩|4.1-6.30||
|rank5-min|706 更新页|706|**rank5.5**|v5.5|ranking|ranking-marjor|黄志浩、陈翔|4.1-6.30||
|708 下载前|708|
|709 下载后合并|709|
|739 首页banner|739|
|746 流量引导|746|
|9000  rom场景(云文件夹，i管家，短信安装)|9000|
|9002 cpd推荐小场景|9002|
|9003 cpd相关推荐小场景|9003|
|9005|9005|
|9004 rom激励场景(i主题激励，电子书激励，积分，小说，浏览器小说激励)|9050|
|9051 浏览器小场景-拦截|9051|
|9052 浏览器小场景-热版|9052|
|rank5-741|pltv 全场景|741|**rank5**|upgrade_vns|ranking|ranking-pltv|周清华|4.1-6.30|1 整理rank5合并到ranking方案 – 4.15|
|pcnt 全场景|
|cvr2 全场景|
|rank5-wash|9006|9006|**rank5.5**|v5.5|ranking|ranking-minor|黄志浩|4.1-6.30||
|ranking-push|846 商业push|846|**ranking**|cpd/push|ranking|ranking-push|谢宝勇|6.30||
|ranking-marjor|702 激活页|702|**ranking**|master|ranking|ranking-marjor|黄志浩|done||
|705 新详情页|705|**ranking**|master|ranking|ranking-marjor|黄志浩|done||
|ranking-minor|713|713|**ranking**|master|ranking|ranking-minor|黄志浩|4.1-6.30||
|cpd-recall|召回主路所有场景|召回全场景|**rank5.5**|master|ranking|cpd-recall|黄志浩|3.16-4.30|召回主路直接迁移到ranking|
|search|rank5-lu|StoreHotWord|1202|**ranking**|master|ranking|ranking-search(rank5-lu)|黄志浩|7.1-9:30||
|LuHotWord|1202|
|LuSugword|1202|
|LuWordDiversion|1202|
|LuGlobalSearch|1202|
|LuInformation|1202|
|LuAlliance|1202|
|rank5-search-wash|9  aSSOSearchGameWash||**rank5.5**|v5.5|ranking|ranking-search|黄志浩|7.1-9:30||
|10 storeSearchGameWash||
|search-wash|6 storeSearchDLWash||**rank5**|master|ranking|ranking-search|黄志浩|7.1-9:30||
|4 storeSearchWash:5.13||
|745 globalSearchTabAppWash|745|
|3 aSSOSearchWash||
|rank5-small|4 storeSearch (商店搜索接在小场景？)||**rank5.5**|v5.5|ranking|search-ranking|黄志浩|7.1-9:30||
|4 homeHistoryRecommend (商店搜索接在小场景？)||
|3 aSSOSearch  (商店搜索接在小场景？)||
|5 browserSearchWash|745|
|9 storeSearchGame||
|10 aSSOSearchGame||
|754 purchaseFlowSearch  (商店搜索接在小场景？)||
|731 applicationAdvice|731|
|745 globalSearchTabApp|745|
|745 globalSearch|745|
|745 globalSearchTabOnline|745|
|750 hotAppV3|750|
|51 globalAISearchWash|745|
|52 browserAISearchWash|745|
|small-rank (游戏场景请求搜索算法？)|gamecenter.search.mix||**rank5**|master|ranking|ranking-search|黄志浩、张琨|4.1-6.30|rank2协议、推动张坤迁移，高优。 只预估了 一个ctr。|
|gamecenter.search.association||
|rank5-store|3 aSSOSearch|3|**rank5.5**|v5.5||ranking-search|黄志浩|7.10-9.30|包含alpha预估流量，预充流量，精排流量|
||4 storeSearch|4|
|search-ranking|764 storeSearchAllianceDSP|4|**ranking**|master|ranking|ranking-search(search-ranking)|黄志浩|done||
|763 aSSOSearchAllianceDSP|3|
|755 globalSearchShoppRank|731|
|5 browserSearch|5|
|735 vRecommendB|731|
|753 applicationAdvice|731|
|5 h5_storeSearch|5|
|733 globalSearchRecommendBanner|731|
|732 globalSearchRecommend|731|
|734 vRecommendA|731|
|2897899||
|ranking_small|9052 browserFileDownload|9052|**ranking**|master|ranking|ranking-search(ranking-small)|黄志浩|7.1-9:30||
|9052 browserTopDownload|
|9052 h5_index|
|9052 h5_searchActive|
|9052 h5_appPage|
|内销|rank4-common-sched|内销所有场景样本迁移|所有场景|**rank4**|master|ranking|ranking-inner|景朝、传伟|=|行动计划：<br><br>先将全场景的样本落下、user、item|
|rank4-common-sched|内销所有场景|所有场景|**ranking**|master|ranking|ranking-inner|景朝、传伟||行动计划：<br><br>新场景接入、无一致性比对。|
|ranking-inner|商店、智慧push、官网push|push|**ranking**|master|ranking|ranking-inner|景朝、传伟||行动计划：|

  

**rank离线：**

不区分方向，只保留一个服务，一个版本rank-snapshot

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|业务方向|cmdb服务名|版本|服务类型|负责人|排期|
|**cpd**|rank4-sched-snapshot|rank4|离线|黄志浩|预计Q4|
|**cpd**|rank5-snapshot-single|rank5|离线|黄志浩|预计Q4|
|**cpd**|rank5-snapshot-multi|rank5|离线|黄志浩|预计Q4|
|**search**|rank4-full-sched|rank4|离线|黄志浩|预计Q4|
|**search**|rank4-upgrade-snapshot|rank4|离线|黄志浩|预计Q4|
|**search**|rank4-browser-snapshot|rank4|离线|黄志浩|预计Q4|
|**search**|rank4-small-snapshot|rank4|离线|黄志浩|预计Q4|
|**search**|small-rank-snapshot|rank5|离线|黄志浩|预计Q4|
|**search**|cpc-rank-snapshot|rank5|离线|黄志浩|预计Q4|
|**search**|rank4-context-sched|rank4|离线|黄志浩|预计Q4|

### 代码规范：

特别强调命名规范：[https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming.html](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming.html)

vscode设置搜索format，把这段格式填入{ IndentWidth: 4, TabWidth: 4,BasedOnStyle: Google, ColumnLimit: 100,  PointerAlignment: Left, DerivePointerAlignment: false}

![[Pasted image 20260325111035.png]]
### 业务治理

1 将网卡带宽瓶颈，导致机器利用率不高问题，一并处理。 

  

告警治理： 告警必须要和cicd的名字能对上，方便值班同学能立马找到流水线进行快速处理。 toucan_705_147 应该是toucan-cvr-705-147。但是这里又涉及到abt的改动，待讨论商榷。

🟡【监控系统-自定义监控-告警-首次告警】  
检测规则: 服务质量告警-rank链路服务失败率: sdk_fail_rate-一般异常(>=0.2) 且 req_qps-一般异常(>=0.2)  
空间名称:应用广告  
异常对象(当前值): ranking_minor#toucan_705_147#713(sdk_fail_rate-0.2314,req_qps-1764.7667)  
开始时间: 2026-03-21 12:22:00  
告警时间: 2026-03-21 12:24:19  
持续时间: 2m19s  
异常比例: 0.6667% (1/150)  
异常级别: 一般  
告警通道: v消息  
问题ID: 38618136  
查看详情:[https://vivo.cn/4jNEzM](https://vivo.cn/4jNEzM)  
备注：

### 升级的规范

区分工程和算法的收益。

1 增加service路由不需要在各个模块服务添加。 之前出现流量接入，但是服务路由没有配置问题。

2 

  

  

cr、代码、监控、上线的规范、约束、整体质量、收益。

合并过程问题：

1 配置冲突

2 共性的问题

3 机制的建设

  

4 合并后，代码质量的管理、上线后版本的问题的处理。 长时间不发版的问题，该如何规避。 

是用实验集群(cpc对齐)、还是回归测试等去规避。

5 代码当前分方向，使用目录去做业务隔离的，是否合理？

6 策略、配置等无效管理的配置

  

### 上线规范

1 rank迁移，功能迭代，上线前都需要回归测试。