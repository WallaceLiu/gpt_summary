# GPT2 for Chinese Summary


## 项目描述
- 本项目使用 GPT2-Chinese 的模型将wiki中文的数据导入模型训练了通用模型。
- 将GPT2-chitchat的对话任务稍作修改来适用于中文摘要任务。
- 将通用模型的权重应用在摘要问题上进行进一步训练的。
- GPT2-Chinese 参考：https://github.com/Morizeyao/GPT2-Chinese
- GPT2-chitchat参考：https://link.zhihu.com/?target=https%3A//github.com/yangjianxin1/GPT2-chitchat
- 项目工作流程详见：https://zhuanlan.zhihu.com/p/113869509
- 本项目为GPT2-chitchat稍作修改的内容，在此也感谢大佬的分享。
- 由于NLPCC的摘要数据为新闻语料，涉及话题和内容较多，应用在垂直领域下效果会好一些。

## 运行环境
python3.6、 transformers==2.1.1、pytorch==1.3.1

## 项目结构
- config:存放GPT2模型的参数的配置文件
- data
    - train_with_summary.txt:默认的原始训练集文件，存放摘要语料 
    - train_tokenized.txt:对原始训练语料进行顺序tokenize之后的文件，用于model的训练
- summary_model:存放摘要生成的模型
- vocabulary:存放GPT2模型的字典
- train.py:训练代码
- interact.py:测试代码


## 模型参数(详见config/model_config_dialogue_small.json文件)
- initializer_range: 0.02
- layer_norm_epsilon: 1e-05
- n_ctx: 300
- n_embd: 768
- n_head: 12
- n_layer: 10
- n_positions: 300
- vocab_size: 13317

## Chinese Summary
Dialogue Model是基于GPT2模型的生成模型，对每条训练数据进行"顺序"拼接，然后将其输入到网络中，进行训练(该项目没有训练MMI Model的"逆序")

在训练Chinese Summary时，将上述训练数据进行如下拼接然后，将上述拼接结果作为Summary Model的输入，对模型进行训练
```python
[CLS]"四海网讯，近日，有媒体报道称：章子怡真怀孕了!报道还援引知情人士消息称，“章子怡怀孕大概四五个月，预产期是年底前后，现在已经不接工作了。”这到底是怎么回事?消息是真是假?针对此消息，23日晚8时30分，华西都市报记者迅速联系上了与章子怡家里关系极好的知情人士，这位人士向华西都市报记者证实说：“子怡这次确实怀孕了。她已经36岁了，也该怀孕了。章子怡怀上汪峰的孩子后，子怡的父母亲十分高兴。子怡的母亲，已开始悉心照料女儿了。子怡的预产期大概是今年12月底。”当晚9时，华西都市报记者为了求证章子怡怀孕消息，又电话联系章子怡的亲哥哥章子男，但电话通了，一直没有人<Paragraph>接听。有关章子怡怀孕的新闻自从2013年9月份章子怡和汪峰恋情以来，就被传N遍了!不过，时间跨入2015年，事情却发生着微妙的变化。2015年3月21日，章子怡担任制片人的电影《从天儿降》开机，在开机发布会上几张合影，让网友又燃起了好奇心：“章子怡真的怀孕了吗?”但后据证实，章子怡的“大肚照”只是影片宣传的噱头。过了四个月的7月22日，《太平轮》新一轮宣传，章子怡又被发现状态不佳，不时深呼吸，不自觉想捂住肚子，又觉得不妥。然后在8月的一天，章子怡和朋友吃饭，在酒店门口被风行工作室拍到了，疑似有孕在身!今年7月11日，汪峰本来在上海要举行演唱会，后来因为台风“灿鸿”取消了。而消息人士称，汪峰原来打算在演唱会上当着章子怡的面宣布重大消息，而且章子怡已经赴上海准备参加演唱会了，怎知遇到台风，只好延期，相信9月26日的演唱会应该还会有惊喜大白天下吧。"[SEP]"知情人透露章子怡怀孕后，父母很高兴。章母已开始悉心照料。据悉，预产期大概是12月底"[SEP]
```



## 模型分享
|模型 | 百度网盘 |模型描述|
|---------|--------|--------|
|GPT2-nlpcc-summary | 链接：https://pan.baidu.com/s/1atsbABI7Lq5HQNctC11E5g <br/>提取码：grtn |摘要模型。使用nlpcc的摘要数据基于GPT2-wiki训练的摘要模型|
|GPT2-wiki | 链接：https://pan.baidu.com/s/1oo1fpuGPYR9IMCcWQzzE9w <br/>提取码：o1aq <br/>复制这段内容后打开百度网盘手机App，操作更方便哦 |通用模型。使用GPT2-Chinese训练的通用模型|

## 模型使用方法

##### 将GPT2-nlpcc-summary参数下载，放入summary_model文件夹中，运行即可。

为了直观展示，这里使用的交互式的预测形式，将需要预测的文章粘贴到控制台即可，由于中间的解码使用的是sampling的方式，所以每次生成的内容不能保证一致。可以多运行几次，取rouge得分最高的,该项目仅用于探索项目，若要实际项目中使用，还需重新训练通用模型和summary模型。


## 示例
```
发布日期：2015-04-19<Paragraph>17:36:34吉安市气象台2015年4月19日17时20分发布雷电黄色预警信号：预计未来6小时内，我市中部将有强雷电活动，局地可伴有短时强降水、雷雨大风等强对流天气，请注意加强防范。图例标准防御指南6小时内可能发生雷电活动，可能会造成雷电灾害事故。1、政府及相关部门按照职责做好防雷工作；2、密切关注天气，尽量避免户外活动。

summary:发布雷电黄色预警：预计未来6小时内，我市中部将有强雷电活动，局地可伴有短时强降水、雷雨大风等强对流天气，请注意加强防范。...
summary:组图：我市中部地区发布雷电黄色预警：预计未来6小时内，我市中部将有强雷电活动，局地可伴有短时强降水、雷雨大风等强对流天气，请注意加强防范。...
summary:组图：我市中部发布雷电黄色预警：预计未来6小时内，我市中部将有强雷电活动，局地可伴有短时强降水、雷雨大风...
summary:组图：我市中部发布雷电黄色预警：预计未来6小时内，我市中部将有强雷电活动，局地可伴有短时强降水、雷雨大风...
summary:组图：我市中部发布雷电黄色预警：预计未来6小时内，我市中部将有强雷电活动，局地可伴有短时强降水、雷雨大风...
```

```
台海网1月1日讯<Paragraph>据中评社报道，国民党12月31日下午在中常会听取副秘书长兼行管会主委林德瑞专案报告党产处理现况，合计党产总值约277亿元（新台币，下同；约54.1亿元人民币）；外界质疑党产总值约为申报5倍，包括中国大陆、海通投资资产估计有1千亿以上，林德瑞表示，此纯属臆测，就目前中投公司所有财务资料，并无中国大陆投资，更无投资1千亿元以上。国民党12月31日中常会由代理主席吴敦义主持，林德瑞依指示提出党产现况报报，回应参选党主席补选的新北市长朱立伦日前提出说要处理不当党产及外界质欵马英九处理党产的成效；中常会后当晚文传会即将党产报告放置在官网“常会特稿”栏内，证明国民党处理党产光明磊落

summary:国民党濛月國日下午在中常会听取报告；外界质疑党产总值约为申报5倍
summary:外媒称党产总值约为申报5倍，外界质疑党产总值约为申报5倍，媒体称此次
summary:外媒称党产总值约为申报5倍，外界质疑党产总值约为申报5倍，媒体称此次报道为申报5倍，外界质疑党产总值约为申报5倍。
summary:外媒称党产总值约为申报5倍，外界质疑为申报5倍，外界质疑为申报5倍。
summary:外媒称党产总值约4倍，外界质疑为申报5倍，外界质疑为申报5倍，外界质疑为申报5倍。
```


