# 中文文本分类项目

## 项目简介
本项目实现了一个基于TF-IDF特征提取和朴素贝叶斯分类器的中文文本分类系统。使用分层抽样方法保持数据分布一致性，通过验证集进行超参数调优，最终在10个新闻类别分类任务上取得良好效果。

## 主要功能
- 数据预处理与分层抽样（训练/验证/测试集按8:1:1比例划分）
- 中文文本分词（基于jieba分词库）
- TF-IDF特征工程（支持unigram+bigram组合特征）
- 多项式朴素贝叶斯分类器
- 超参数自动调优（拉普拉斯平滑系数选择）
- 模型性能评估（含混淆矩阵和详细分类报告）

## 环境要求
- Python 3.6+
- pandas
- jieba
- scikit-learn
- numpy
- scipy
- 依赖库：
  ```bash
  pip install pandas jieba scikit-learn scipy
  ```

## 快速开始
1. 准备数据
   - 将数据集文件`filtered_cnews.train.csv`放入项目根目录
   - 数据格式：CSV文件包含两列（label, text），首行为列名

2. 运行模型
   ```bash
   python main.py
   ```

3. 查看结果
   - 控制台输出各阶段处理日志
   - 最终输出测试集评估结果：
     - 分类报告（精确率/召回率/F1值）
     - 混淆矩阵
     - 宏/微平均F1值

## 实现细节
### 数据划分策略
- 分层抽样保持类别分布
- 三阶段划分流程：
  1. 先拆分测试集（10%）
  2. 再从剩余数据拆分训练集（80%）和验证集（10%）

### 特征工程
- 中文分词：jieba精确模式
- TF-IDF参数：
  ```python
  max_features=5000    # 最大特征数
  ngram_range=(1,2)    # 包含单字和双字组合
  min_df=5             # 过滤低频词
  max_df=0.9           # 过滤高频词
  ```

### 模型调优
- 候选alpha参数：[0.1, 0.5, 1.0, 1.5, 2.0]
- 验证集选择最优参数
- 最终训练合并训练集+验证集

## 项目结构
```
.
├── filtered_cnews.train.csv  # 数据集文件
├── main.py                   # 主程序文件
└── README.md                 # 说明文档
```

## 扩展方向
- 尝试其他分类器
- 加入自定义停用词表
- 优化分词策略（实体识别、新词发现）
- 调整TF-IDF参数（增加trigram特征）
