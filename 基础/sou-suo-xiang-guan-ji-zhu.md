# 搜索相关技术

**实现搜索的关键就是分词和倒序索引.**

知道每行数据中包含多少个关键字的过程,就是分词.

建立一个映射表 , 把每个关键字出现在哪行数据中记录下来 , 这个过程就是建立倒序索引 .

#### 常见的开源搜索软件

**Lucene**

受欢迎的免费开源Java信息检索程序库

**Solr**

高性能,Java5开发,基于Lucene的全文搜索软件.

**ElasticSearch**

基于Lucene的搜索服务器 . 分布式多用户全文搜索哦引擎 .

RESTful Web接口 . Java开发 , Apache许可条款下开源 .

第二流行.

**Sphinx**

基于SQL的全文检索引擎 , 结合MySQL , PostgreSQL做全文搜索 .

可以提供比数据库本身更专业的搜索功能 .

提供PHP,Python等一些脚本语言的API接口 .

**CoreSeek**

中文全文检索软件 , 基于Sphinx研发并独立发布 .

配置简单 , 性能高效 , 整合了Sphinx和中文分词 . 但最大的缺点是稳定版本暂不支持实时索引 .

**CoreSeek的工作流程**

![](/assets/coreseek.png)

1. Indexer模块从MySQL中拉取数据
2. Indexer模块用经过中文分词后的数据建立索引
3. 客户端向Search模块发起搜索请求
4. Search模块查找索引中的数据
5. Search模块得到索引中符合要求的数据的id等数据
6. 把数据返回给客户端



