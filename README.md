####  1. 构造数据集 ：

原始数据集是NLPCC已经处理好的三元组：

```
<question id=1>	《机械设计基础》这本书的作者是谁？
<triple id=1>	机械设计基础 ||| 作者 ||| 杨可桢，程光蕴，李仲生
<answer id=1>	杨可桢，程光蕴，李仲生
==================================================
<question id=2>	《高等数学》是哪个出版社出版的？
<triple id=2>	高等数学 ||| 出版社 ||| 武汉大学出版社
<answer id=2>	武汉大学出版社
==================================================
```

通过 2-construct_dataset_ner.py 构造命名实体识别的数据集， 由于只需要识别实体，所以只标记实体和非实体，供三个类别["O","B-LOC","I-LOC"] ：

```
《 O
机 B-LOC
械 I-LOC
设 I-LOC
计 I-LOC
基 I-LOC
础 I-LOC
》 O
这 O
本 O
书 O
的 O
作 O
者 O
是 O
谁 O
？ O
```

 3-construct_dataset_attribute.py  随机采样5个负样本，其中问题(样本)自己的属性为正例，用于训练Bert分类模型，判断问题要问的是不是这个属性。

将三元组（实体-属性-答案）存入数据库



#### 2. 训练模型 ：

主要有两个模型：

模型1：BertCrf 用 BertForTokenClassification + crf 模型  用于识别出问题中的实体，是查询数据库的基础。

运行文件 NER_main.py 训练

模型2：BertForSequenceClassification , 用于句子分类。把问题 和 属性拼接到一起，判断问题要问的是不是这个属性。

运行文件 SIM_main.py 训练



#### 3. 最终效果：

两个模型在测试集上性能达到97%左右，问答基本完成


#### 4. 环境配置与文件介绍：

构造数据集 通过 1_split_data.py 切分数据

通过 2-construct_dataset_ner.py 构造命名实体识别的数据集

通过 3-construct_dataset_attribute.py 构造属性相似度的数据集

通过 4-print-seq-len.py 看看句子的长度

通过 5-triple_clean.py 构造干净的三元组

通过 6-load_dbdata.py 通过创建数据库 和 上传数据

CRF_Model.py 条件随机场模型

BERT_CRF.py bert+条件随机场

NER_main.py 训练命令实体识别的模型

SIM_main.py 训练属性相似度的模型

test_NER.py 测试命令实体识别

test_SIM.py 测试属性相似度

test_pro.py 测试整个项目

主要依赖版本：

torch.**version** 1.2.0

transformers.**version** 2.0.0

带有命令运行的py文件的命令都在 该py文件的最上方

