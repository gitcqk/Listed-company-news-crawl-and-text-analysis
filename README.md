# 上市公司新闻文本分析与分类预测

 ![image](https://github.com/DemonDamon/Listed-company-news-crawl-and-text-analysis/blob/master/docs/images/FINNEWS-HUNTER.jpg)

-------------------------------

## 简介

上市公司新闻文本分析与分类预测的基本步骤如下：

 - 从新浪财经、每经网、金融界、中国证券网、证券时报网上，爬取上市公司（个股）的历史新闻文本数据（包括时间、网址、标题、正文）
 - 从Tushare上获取沪深股票日线数据（开、高、低、收、成交量和持仓量）和基本信息（包括股票代码、股票名称、所属行业、所属地区、PE值、总资产、流动资产、固定资产、留存资产等）
 - 对抓取的新闻文本按照，去停用词、加载新词、分词的顺序进行处理
 - 利用前两步中所获取的股票名称和分词后的结果，抽取出每条新闻里所包含的（0支、1支或多支）股票名称，并将所对应的所有股票代码，组合成与该条新闻相关的股票代码列表，并在历史数据表中增加一列相关股票代码数据
 - 从历史新闻数据库中抽取与某支股票相关的所有新闻文本，利用该支股票的日线数据（比如某一天发布的消息，在设定N天后如果价格上涨则认为是利好消息，反之则是利空消息）给每条新闻贴上“利好”和“利空”的标签，并存储到新的数据库中（或导出到CSV文件）
 - 实时抓取新闻数据，判断与该新闻相关的股票有哪些，利用上一步的结果，对与某支股票相关的所有历史新闻文本（已贴标签）进行文本分析（构建新的特征集），然后利用SVM（或随机森林）分类器对文本分析结果进行训练（如果已保存训练模型，可选择重新训练或直接加载模型），最后利用训练模型对实时抓取的新闻数据进行分类预测

开发环境`Python-v3(3.6)`：

 - gensim==3.2.0
 - jieba==0.39
 - scikit-learn==0.19.1
 - pandas==0.20.0
 - numpy==1.13.3+mkl
 - scipy==0.19.0
 - pymongo==3.6.0
 - beautifulsoup4==4.6.0
 - tushare==1.1.1
 - requests==2.18.4
 - gevent==1.2.1

## 文本处理(`text_processing.py`)

 - 文本处理包括去停用词处理、加载新词、中文分词、去掉出现次数少的分词
 - 生成字典和Bow向量，并基于Gensim转化模型（LSI、LDA、TF-IDF）转化Bow向量
 - 计算文本相似度
 - 打印词云

## 文本挖掘（`text_mining.py`）

 - 从新闻文本中抽取特定信息，并贴上新的文本标签方便往后训练模型
 - 从数据库中抽取与某支股票相关的所有新闻文本
 - 将贴好标签的历史新闻进行分类训练，利用训练好的模型对实时抓取的新闻文本进行分类预测

## 新闻爬取（`crawler_cnstock.py`，`crawler_jrj.py`，`crawler_nbd.py`，`crawler_sina.py`，`crawler_stcn.py`）

 - 分析网站结构，多线程（或协程）爬取上市公司历史新闻数据

## Tushare数据提取（`crawler_tushare.py`）

 - 获取沪深所有股票的基本信息，包括股票代码、股票名称、所属行业、所属地区等

## 用法

 - 配好运行环境以及安装MongoDB，最好再安装一个MongoDB的可视化管理工具Studio 3T
 - 先运行`run_crawler_cnstock.py`，`run_crawler_jrj.py`，`run_crawler_nbd.py`，`run_crawler_sina.py`，`run_crawler_stcn.py`这5个py文件，而且可能因为对方服务器没有响应而重复多次运行这几个文件才能抓取大量的历史数据
 - 接着运行`run_crawler_tushare.py`从Tushare获取基本信息和股票价格
 - 最后运行`run_main.py`文件，其中有4个步骤，除了第1步初始化外，其他几步最好单独运行
 - 注意：所有程序都必须在文件所在目录下运行

## 更新目标

 由于之前的项目代码是在初学Python的时候写的，很多写法都是入门级别，因此为了提高整体项目的质量，除了优化代码细节和已有的功能模块之外，还加入了多个功能模块，来支撑未来更加智能化和个体化的金融分析与交易。
 - 完成初步构想，重构该项目，将项目分成8大模块，分别是`数据获取模块`，`数据清洗与预处理模块`，`大数据可视化模块`，`基于机器学习的文本挖掘模块`，`金融知识图谱构建模块`，`任务导向多轮对话模块`，`金融交易模块`，`通用服务模块`
 (备注：项目在完善之后会重新更名为`Finnews Hunter`，命名的来源是出于对《全职猎人》的喜爱，与项目本质的结合，其中`Finnews`是`Financial News`的简写。上面提到的8个模块，分别由《全职猎人》中的本人最喜爱的8位角色命名，分别是
 - `数据获取模块`               => `Gon`
 - `数据清洗与预处理模块`       => `Killua` 
 - `大数据可视化模块`           => `Kurapika`
 - `基于机器学习的文本挖掘模块` => `Leorio`  
 - `金融知识图谱构建模块`       => `Hisoka`
 - `任务导向多轮对话模块`       => `Chrollo`
 - `金融交易模块`               => `Illumi`
 - `通用服务模块`               => `Kite`)
 
 ## 更新日志
 

