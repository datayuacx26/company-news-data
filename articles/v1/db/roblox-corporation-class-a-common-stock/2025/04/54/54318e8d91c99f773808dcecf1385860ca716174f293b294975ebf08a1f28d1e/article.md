---
schema_version: "1.0.0"
document_id: "54318e8d91c99f773808dcecf1385860ca716174f293b294975ebf08a1f28d1e"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-f52851b7bf16"
canonical_url: "https://about.roblox.com/zh-hans/newsroom/2025/04/roblox-creator-analytics"
published_at: "2025-04-15T16:00:00+00:00"
first_seen_at: "2026-07-23T23:14:17.357493+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:c674fd56117e272166b1d1f4940daeed49b39904e4420c8247a5644215bdca0d"
---

# Roblox 如何利用 Theta Sketches 扩展创作者分析功能

分享


[工程](https://about.roblox.com/zh-hans/newsroom?filter=%E5%B7%A5%E7%A8%8B)


# Roblox 如何利用 Theta Sketches 扩展创作者分析功能


作者


威利斯·肯尼迪


已发布


2025年4月15日


分析对当今的实时多人游戏至关重要。在 Roblox，我们致力于开发衡量工具，以助力创作者取得成功。我们免费且开箱即用的分析工具能让创作者即时了解其作品的增长、用户获取和留存情况，从而帮助他们取得最大成功。


构建数百万 Roblox 创作者所依赖的实时分析系统是一项重大挑战。为应对这一挑战，我们优化了分析查询引擎，使一个 120 核的处理集群能够每天处理超过 600 万次查询，这些查询来自约 30 万日活跃访问者，涉及 86 TB 的数据。 我们解决方案的核心是一套在线分析处理（OLAP）数据库，选择该方案是因为其具备可扩展性，且能与近似算法无缝集成。通过结合数据汇总技术以及[HyperLogLog](https://datasketches.apache.org/docs/HLL/HllSketches.html) 和[Theta](https://datasketches.apache.org/docs/Theta/ThetaSketches.html) Sketch 算法，我们为数百万个 Roblox 体验提供分析支持¹。


##### OLAP 分析入门指南


查询的数据越多，生成结果所需的时间就越长。当我们能够减少所需数据并加快分析过程时，创作者就能从他们的行动中获得近乎实时的洞察。我们采用的一些技术包括：


1. **列式存储：** OLAP系统[Druid](https://druid.apache.org/) 仅读取必要的列。
2. **分区与排序过滤器：** OLAP 仅读取直接指向所需数据块的相关文件。
3. **汇总：** OLAP 通过常见的分组方式对事件进行部分聚合。


特别是汇总功能，使 OLAP 能够在大型 SQL 查询引擎（如 Spark 或 Presto，延迟达数十秒）与点查询或有限 SQL（通常提供完全聚合的数据）之间进行操作。借助汇总功能，查询将按维度分组进行索引，从而大幅降低总行数。 面对数十亿甚至数万亿条原始事件时，将其汇总为数百万个分组（可在亚秒级延迟下进行聚合）往往效率更高。例如：


虽然汇总操作具备上述提到的简化优势，但某些指标难以通过这种方式处理，包括需要对原始数据进行全表排序的查询，例如唯一计数、分位数和频率查询。


幸运的是，我们可以借助基于复杂数据结构的技术来规避这些限制，这些结构存储了完整数据集的样本，并能返回具有统计学意义的近似结果。这些数据结构专为汇总技术设计，可通过并集操作将两个独立计数结合起来，类似于将两个数字相加。


##### 拆解 Roblox 分析工作负载


在 Roblox，我们为创作者提供了一个[集中式仪表盘](https://create.roblox.com/docs/production/analytics/analytics-dashboard) ，让他们能够在此获取最重要的洞察。这些包括：


- 用户参与度：日活跃用户 (DAU)、月活跃用户 (MAU)、留存率及用户漏斗
- 变现：收入、人均收入、销售额及游戏内经济
- 用户获取数据
- 缩略图个性化及实验结果
- [首页推荐](https://devforum.roblox.com/t/boost-your-discovery-with-the-improved-recommended-for-you-algorithm-and-analytics-for-creators/3587441) 分析
- 更多内容即将推出。


在构建系统时，我们专注于优化最坏情况下的查询。这类查询通常涉及大规模的唯一计数（>1亿个UUID），例如热门产品的月活跃用户数（MAU），此类查询可能导致加载时间从几秒延长至数分钟。 我们构建了一个统计近似框架，以确保查询延迟控制在两秒内。我们采用了[行业标准库](https://datasketches.apache.org/docs/Background/TheChallenge.html) 中的 HyperLogLog 和 Theta Sketch 技术，将最坏情况下的查询读取行数从超过 1 亿行减少到约 500 万行。


##### 查询引擎的选择与优化


在选定 OLAP 解决方案后，我们导入了六个月的互动数据，并对系统性能极限进行了压力测试。 借助约100个处理器核心和500 GB内存，我们发现可以在两秒内随机合并500万个二进制Theta Sketch对象（总计约100 MB）。该测试是在冷启动查询条件下进行的，数据直接从磁盘读取，未访问任何内存缓存。而Clickhouse和Duckdb默认提供的S3读取等网络存储方案，其性能表现则明显较差。


##### 克服性能挑战


在最后一次生产环境模拟测试中，我们发现了一个重要挑战：在将查询模式从单个大型查询切换为按日聚合模式后，我们的MAU查询性能亟需提升。这些查询对于我们的创作者分析可视化至关重要。


我们发现，查询结构极大地影响了 OLAP 解决方案的基础性能。具有多执行阶段的标准查询（如嵌套的“GROUP BY”语句2）往往会将大量工作推给轻量级的代理节点。


这是一种典型的“大数据”问题：查询的一部分最终会在关键的小型服务节点上执行。我们原本期望近似数据结构能像简单的计数或求和那样工作，但发现它们的实际表现截然不同。


下图说明了这一问题。它展示了历史节点如何进行部分聚合：先为每天生成一个 Theta Sketch，然后将数据推送回代理节点。随后，代理节点试图将每个庞大的每日草图合并为单个每日月度值。 对于30天的MAU数据，这意味着需要在代理上合并1,800个最大尺寸的Theta Sketches，这导致查询速度变慢、容易失败，并独占了代理的CPU资源。


我们的解决方案是减少运行OLAP的大型历史工作进程数量，以最大限度地提高依赖近似查询的数据源的数据局部性。实际上，这将原本可能需要处理超过100 MB数据的合并操作推回到了我们的历史节点上。


为了在 SQL 中实现这一点，我们使用了[内联连接](https://github.com/Roblox/data-tools/blob/main/AnalyticsForEveryone/druid_workflow/sql/query/mau_query_example.sql) ，使查询将必要信息传播到历史节点，并准备了一个包含内联结果日期列表的查询。随后，每个结果日期都可以从历史节点分段中收集相关数据。数据随后被传回代理，在那里结果被快速合并为一个结果日期到指标数据的单一映射，如下所示。


这项优化对大规模查询的性能产生了显著影响。如下图所示，针对某主要产品的按国家划分的月活跃用户（MAU）数据，平均查询性能提升了5倍（从17.53秒缩短至3.23秒）。此外，我们在消息代理（broker）上的CPU时间也减少了50倍（从16.83秒降至0.34秒）。


虽然具体结果因情况而异，但这突显了谨慎处理复杂操作（例如合并数百万个草图）的重要性。若将此类操作等同于简单的聚合操作，可能会导致严重的性能问题，特别是在常见“最后一公里”客户端聚合的系统中。


#### 汇总与理论Theta立方体


在我们平台上，平均每次分析查询的维度拆分极少，且极少涉及高基数维度（如国家）。基于这一情况，我们决定对数据进行复制，创建一张维度更少的汇总表，该表足以满足超过98%³的查询需求。在平均查询中，其性能提升了四倍。


我们还探索了 theta 立方体，即一种通过近似集合交集来弥合基础汇总表与完全原始表之间差距的通用方法。该方法解决了根本性的局限性：当查询需要涉及多层高基数维度时，汇总表便会失去其优势。这是因为每个维度都会导致汇总基数按 ∏dim（维度的乘积）成倍增长。


我们设计了一套系统，该系统能够按具有基数上限的共同维度组进行聚合，从而支持对组内任意内容执行聚合性能查询。随后，当需要跨组查找维度组合时，我们会尝试对集合执行近似 join4 操作，并返回指标结果及误差估计值。对于估计误差较高的查询，将转至原始表处理，在那里，大量过滤条件应能支持大规模的向下推优化。


这种Theta立方体方法改变了维度，导致行数的展开式变为∑dim（维度之和），而非∏dim展开式。当然，这可能会牺牲精度，这种影响与两个维度组之间的重叠大小5成正比。其根本原因直接与Theta Sketches如何存储底部K式排序列表有关，这种方式会最大化两个具有高内在重叠度的集合之间的碰撞。


由于我们可以快速计算出该误差率，这也强烈表明读取原始表很可能具有良好的性能。在重叠数据相对于并集而言较小的情况下（例如德国的日语使用者），原始表中的大量行会被过滤掉。这将带来高效的下推优化。 一个同时采用维度组、近似连接和基于误差的原始表读取的系统，将真正最大化近似友好型查询的汇总性能。


对于 Roblox 而言，该方案将在我们下一阶段的规模扩展中更具适用性——可能用于动态漏斗分析或自定义事件分析——而当前简单的汇总副本已能满足现阶段的需求。


##### 构建自助服务平台


随着数据经纪人的优化完成，我们开始着手构建用于接入和查询添加到 OLAP 解决方案中的数据集的工具。我们为数据摘要函数构建了一个开源的[Spark 和 Trino UDAF](https://github.com/Roblox/data-tools) 库，使 Spark 能够使用与我们的 OLAP6 相同的二进制数据摘要格式。这使得大部分计算工作负载仍保留在 Spark 中，并有助于在 Roblox 范围内标准化近似计算，对于某些数据集，计算成本有望降低多达 80%。


我们通过批处理作业调度器的内部扩展简化了数据导入流程，并定义了[DataFrame](https://github.com/Roblox/data-tools/blob/main/AnalyticsForEveryone/druid_workflow/cube/schemas/Engagement.js)[风格的 API](https://github.com/Roblox/data-tools/blob/main/AnalyticsForEveryone/druid_workflow/cube/schemas/Engagement.js) ，引导开发者确定明确的度量和维度，从而降低开放式查询的影响。此外，我们还开源了一些[示例工作流](https://github.com/Roblox/data-tools/tree/main/AnalyticsForEveryone/druid_workflow) ，展示如何在 OLAP 中加载和查询这些数据。


经过优化的分析数据集现已为创作者提供深度洞察。这些优化使平均性能提升了 4 倍，最坏情况下的性能提升了 50 倍。自助服务平台使我们的创作者分析团队能够持续为开发者迭代开发新的数据集。我们非常期待看到各种规模的开发者利用这些工具，在 Roblox 上创造令人惊叹的体验。


1 基于过去 60 天内有过任何访问
行为的独立用户群体计算得出 2 类似于这个简单的[MAU 查询](https://github.com/Roblox/data-tools/blob/main/AnalyticsForEveryone/druid_workflow/sql/query/mau_query_example.sql)
3 结果来自 2025
年 3 月 21 日至 28 日 4 执行方式如下：SELECT c.experience_id, c.country, p.platform, THETA_INTERSECT(c.user_theta, p.user_theta) from (select experience_id, country, user_theta from theta_cube where agg_level = country) c union (select experience_id, platform, user_theta from theta_cube where agg_level = platform) p
5[https://datasketches.apache.org/docs/Theta/ThetaSketchSetOpsAccuracy.html](https://datasketches.apache.org/docs/Theta/ThetaSketchSetOpsAccuracy.html)
6 通过 Druid SQL 函数` COMPLEX_DECODE_BASE64('HLLSketch', sketch_col_name`` )` .
