# CLUENER2020

本项目是计算语言学课程作业的代码实现。数据集来自CLUENER2020任务。

## Dataset

实验数据来自CLUENER2020，是一个中文细粒度命名实体识别数据集，是基于开源文本分类数据集THUCNEWS，选出部分数据进行细粒度标注得到的。该数据集的训练集、验证集和测试集的大小分别为10748，1343，1345，平均句子长度37.4字，最长50字。由于测试集不直接提供，作业考虑到leaderboard上提交次数有限，要求使用验证集作为模型表现评判的测试集。

CLUENER2020共有10个不同的类别，包括：组织(organization)、人名(name)、地址(address)、公司(company)、政府(government)、书籍(book)、游戏(game)、电影(movie)、职位(position)和景点(scene)。

原始数据分别位于train.json和test.json文件中，文件中的每一行是一条单独的数据，一条数据包括一个原始句子以及其上的标签，具体形式如下：

```
{
	"text": "浙商银行企业信贷部叶老桂博士则从另一个角度对五道门槛进行了解读。叶老桂认为，对目前国内商业银行而言，",
	"label": {
		"name": {
			"叶老桂": [
				[9, 11],
				[32, 34]
			]
		},
		"company": {
			"浙商银行": [
				[0, 3]
			]
		}
	}
}

```

## Requirements

This repo was tested on Python 3.6+ and PyTorch 1.5.1. The main requirements are:

- tqdm
- scikit-learn
- pytorch >= 1.5.1
- 🤗transformers == 2.2.2

To get the environment settled, run:

```
pip install -r requirements.txt
```

## Pretrained Model Required

需要提前下载BERT的预训练模型，包括

- pytorch_model.bin
- vocab.txt

放置在./pretrained_bert_models对应的预训练模型文件夹下，其中

**bert-base-chinese模型：**[下载地址](https://storage.googleapis.com/bert_models/2018_11_03/chinese_L-12_H-768_A-12.zip) 。

注意，以上下载地址仅提供tensorflow版本，需要[huggingface suggest](https://huggingface.co/transformers/converting_tensorflow_models.html)将其转换为pytorch版本。

**chinese_roberta_wwm_large模型：**[下载地址](https://github.com/ymcui/Chinese-BERT-wwm#%E4%BD%BF%E7%94%A8%E5%BB%BA%E8%AE%AE) 。

## Parameter Setting

### 1.model parameters

在./experiments/clue/config.json中设置了Roberta模型的基本参数，而在./pretrained_bert_models下的两个预训练文件夹中，config.json除了设置Roberta基本参数外，还设置了'X'模型（如LSTM）参数，可根据需要进行更改。

### 2.other parameters

环境路径以及其他超参数在./config.py中进行设置。

## Usage

打开指定模型对应的目录，命令行输入：

```
python run.py
```

模型运行结束后，最优模型pytorch_model.bin和训练log保存在./experiments/clue/路径下。在测试集中的bad case保存在./case/bad_case.txt中。

## Attention

目前，当前模型的train.log已保存在./experiments/clue/路径下，如要重新运行模型，请先将train.log移出当前路径，以免覆盖。

