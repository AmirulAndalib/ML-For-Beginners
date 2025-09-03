<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "9579f42e3ff5114c58379cc9e186a828",
  "translation_date": "2025-09-03T18:02:43+00:00",
  "source_file": "4-Classification/2-Classifiers-1/README.md",
  "language_code": "zh"
}
-->
# 美食分类器 1

在本课中，您将使用上一课保存的数据集，该数据集包含关于美食的平衡且干净的数据。

您将使用这个数据集和多种分类器来_根据一组食材预测某种国家美食_。在此过程中，您将进一步了解算法如何用于分类任务。

## [课前测验](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/21/)
# 准备工作

假设您已经完成了[第1课](../1-Introduction/README.md)，请确保在根目录的 `/data` 文件夹中存在一个名为 _cleaned_cuisines.csv_ 的文件，以供这四节课使用。

## 练习 - 预测国家美食

1. 在本课的 _notebook.ipynb_ 文件夹中，导入该文件以及 Pandas 库：

    ```python
    import pandas as pd
    cuisines_df = pd.read_csv("../data/cleaned_cuisines.csv")
    cuisines_df.head()
    ```

    数据看起来如下：

|     | Unnamed: 0 | cuisine | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood | yam | yeast | yogurt | zucchini |
| --- | ---------- | ------- | ------ | -------- | ----- | ---------- | ----- | ------------ | ------- | -------- | --- | ------- | ----------- | ---------- | ----------------------- | ---- | ---- | --- | ----- | ------ | -------- |
| 0   | 0          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 1   | 1          | indian  | 1      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 2   | 2          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 3   | 3          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 0      | 0        |
| 4   | 4          | indian  | 0      | 0        | 0     | 0          | 0     | 0            | 0       | 0        | ... | 0       | 0           | 0          | 0                       | 0    | 0    | 0   | 0     | 1      | 0        |
  

1. 接下来，导入更多的库：

    ```python
    from sklearn.linear_model import LogisticRegression
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.metrics import accuracy_score,precision_score,confusion_matrix,classification_report, precision_recall_curve
    from sklearn.svm import SVC
    import numpy as np
    ```

1. 将 X 和 y 坐标分成两个数据框用于训练。`cuisine` 可以作为标签数据框：

    ```python
    cuisines_label_df = cuisines_df['cuisine']
    cuisines_label_df.head()
    ```

    它看起来如下：

    ```output
    0    indian
    1    indian
    2    indian
    3    indian
    4    indian
    Name: cuisine, dtype: object
    ```

1. 使用 `drop()` 删除 `Unnamed: 0` 列和 `cuisine` 列。将剩余的数据保存为可训练的特征：

    ```python
    cuisines_feature_df = cuisines_df.drop(['Unnamed: 0', 'cuisine'], axis=1)
    cuisines_feature_df.head()
    ```

    您的特征看起来如下：

|      | almond | angelica | anise | anise_seed | apple | apple_brandy | apricot | armagnac | artemisia | artichoke |  ... | whiskey | white_bread | white_wine | whole_grain_wheat_flour | wine | wood |  yam | yeast | yogurt | zucchini |
| ---: | -----: | -------: | ----: | ---------: | ----: | -----------: | ------: | -------: | --------: | --------: | ---: | ------: | ----------: | ---------: | ----------------------: | ---: | ---: | ---: | ----: | -----: | -------: |
|    0 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    1 |      1 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    2 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    3 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      0 |        0 | 0 |
|    4 |      0 |        0 |     0 |          0 |     0 |            0 |       0 |        0 |         0 |         0 |  ... |       0 |           0 |          0 |                       0 |    0 |    0 |    0 |     0 |      1 |        0 | 0 |

现在您可以开始训练模型了！

## 选择分类器

现在数据已经清理完毕并准备好训练，您需要决定使用哪种算法来完成任务。

Scikit-learn 将分类归类为监督学习，在这一类别中您会发现许多分类方法。[种类繁多](https://scikit-learn.org/stable/supervised_learning.html)，初看可能会让人眼花缭乱。以下方法都包含分类技术：

- 线性模型
- 支持向量机
- 随机梯度下降
- 最近邻
- 高斯过程
- 决策树
- 集成方法（投票分类器）
- 多分类和多输出算法（多分类和多标签分类，多分类-多输出分类）

> 您也可以使用[神经网络进行数据分类](https://scikit-learn.org/stable/modules/neural_networks_supervised.html#classification)，但这超出了本课的范围。

### 选择哪个分类器？

那么，应该选择哪个分类器呢？通常，可以尝试多个分类器并寻找效果较好的结果。Scikit-learn 提供了一个[并排比较](https://scikit-learn.org/stable/auto_examples/classification/plot_classifier_comparison.html)，在一个创建的数据集上比较 KNeighbors、SVC 两种方式、GaussianProcessClassifier、DecisionTreeClassifier、RandomForestClassifier、MLPClassifier、AdaBoostClassifier、GaussianNB 和 QuadraticDiscrinationAnalysis，并以可视化方式展示结果：

![分类器比较](../../../../translated_images/comparison.edfab56193a85e7fdecbeaa1b1f8c99e94adbf7178bed0de902090cf93d6734f.zh.png)
> 图表来源于 Scikit-learn 文档

> AutoML 可以轻松解决这个问题，通过在云端运行这些比较，帮助您选择最适合数据的算法。试试 [这里](https://docs.microsoft.com/learn/modules/automate-model-selection-with-azure-automl/?WT.mc_id=academic-77952-leestott)

### 更好的方法

比盲目猜测更好的方法是参考这个可下载的[机器学习备忘单](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=academic-77952-leestott)。在这里，我们发现对于我们的多分类问题，有一些选择：

![多分类问题备忘单](../../../../translated_images/cheatsheet.07a475ea444d22234cb8907a3826df5bdd1953efec94bd18e4496f36ff60624a.zh.png)
> 微软算法备忘单的一部分，详细说明了多分类选项

✅ 下载这个备忘单，打印出来，挂在墙上！

### 推理

让我们看看是否可以根据现有约束推理出不同的解决方法：

- **神经网络过于复杂**。考虑到我们的数据集虽然干净但规模较小，并且我们通过本地笔记本运行训练，神经网络对于这个任务来说过于复杂。
- **不使用二分类器**。我们不使用二分类器，因此排除 one-vs-all。
- **决策树或逻辑回归可能有效**。决策树可能有效，或者逻辑回归适用于多分类数据。
- **多分类增强决策树解决不同问题**。多分类增强决策树最适合非参数任务，例如设计排名任务，因此对我们来说不适用。

### 使用 Scikit-learn 

我们将使用 Scikit-learn 来分析数据。然而，在 Scikit-learn 中有许多方法可以使用逻辑回归。查看[可传递的参数](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html?highlight=logistic%20regressio#sklearn.linear_model.LogisticRegression)。  

基本上有两个重要参数 - `multi_class` 和 `solver` - 需要指定，当我们要求 Scikit-learn 执行逻辑回归时。`multi_class` 值应用某种行为。solver 的值决定使用哪种算法。并非所有 solver 都可以与所有 `multi_class` 值配对。

根据文档，在多分类情况下，训练算法：

- **使用 one-vs-rest (OvR) 方案**，如果 `multi_class` 选项设置为 `ovr`
- **使用交叉熵损失**，如果 `multi_class` 选项设置为 `multinomial`。（目前 `multinomial` 选项仅支持 ‘lbfgs’、‘sag’、‘saga’ 和 ‘newton-cg’ solver。）

> 🎓 这里的“方案”可以是 'ovr'（one-vs-rest）或 'multinomial'。由于逻辑回归实际上是为支持二分类设计的，这些方案使其能够更好地处理多分类任务。[来源](https://machinelearningmastery.com/one-vs-rest-and-one-vs-one-for-multi-class-classification/)

> 🎓 'solver' 定义为“用于优化问题的算法”。[来源](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html?highlight=logistic%20regressio#sklearn.linear_model.LogisticRegression)。

Scikit-learn 提供了这个表格来解释 solver 如何处理不同数据结构带来的挑战：

![solver](../../../../translated_images/solvers.5fc648618529e627dfac29b917b3ccabda4b45ee8ed41b0acb1ce1441e8d1ef1.zh.png)

## 练习 - 划分数据

我们可以专注于逻辑回归作为第一次训练尝试，因为您在之前的课程中刚刚学习过它。
通过调用 `train_test_split()` 将数据划分为训练组和测试组：

```python
X_train, X_test, y_train, y_test = train_test_split(cuisines_feature_df, cuisines_label_df, test_size=0.3)
```

## 练习 - 应用逻辑回归

由于您使用的是多分类情况，您需要选择使用什么_方案_以及设置什么_solver_。使用 LogisticRegression 并设置 multi_class 为 `ovr` 和 solver 为 `liblinear` 进行训练。

1. 创建一个逻辑回归，multi_class 设置为 `ovr`，solver 设置为 `liblinear`：

    ```python
    lr = LogisticRegression(multi_class='ovr',solver='liblinear')
    model = lr.fit(X_train, np.ravel(y_train))
    
    accuracy = model.score(X_test, y_test)
    print ("Accuracy is {}".format(accuracy))
    ```

    ✅ 尝试使用其他 solver，例如默认设置的 `lbfgs`
> 注意，当需要将数据展平时，可以使用 Pandas [`ravel`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.ravel.html) 函数。
准确率超过 **80%**！

1. 你可以通过测试一行数据（#50）来查看此模型的实际效果：

    ```python
    print(f'ingredients: {X_test.iloc[50][X_test.iloc[50]!=0].keys()}')
    print(f'cuisine: {y_test.iloc[50]}')
    ```

    结果打印如下：

    ```output
   ingredients: Index(['cilantro', 'onion', 'pea', 'potato', 'tomato', 'vegetable_oil'], dtype='object')
   cuisine: indian
   ```

    ✅ 尝试不同的行号并检查结果

1. 更深入地分析，你可以检查此预测的准确性：

    ```python
    test= X_test.iloc[50].values.reshape(-1, 1).T
    proba = model.predict_proba(test)
    classes = model.classes_
    resultdf = pd.DataFrame(data=proba, columns=classes)
    
    topPrediction = resultdf.T.sort_values(by=[0], ascending = [False])
    topPrediction.head()
    ```

    结果打印如下 - 印度菜是模型的最佳猜测，概率较高：

    |          |        0 |
    | -------: | -------: |
    |   indian | 0.715851 |
    |  chinese | 0.229475 |
    | japanese | 0.029763 |
    |   korean | 0.017277 |
    |     thai | 0.007634 |

    ✅ 你能解释为什么模型非常确定这是印度菜吗？

1. 通过打印分类报告获取更多细节，就像你在回归课程中所做的一样：

    ```python
    y_pred = model.predict(X_test)
    print(classification_report(y_test,y_pred))
    ```

    |              | precision | recall | f1-score | support |
    | ------------ | --------- | ------ | -------- | ------- |
    | chinese      | 0.73      | 0.71   | 0.72     | 229     |
    | indian       | 0.91      | 0.93   | 0.92     | 254     |
    | japanese     | 0.70      | 0.75   | 0.72     | 220     |
    | korean       | 0.86      | 0.76   | 0.81     | 242     |
    | thai         | 0.79      | 0.85   | 0.82     | 254     |
    | accuracy     | 0.80      | 1199   |          |         |
    | macro avg    | 0.80      | 0.80   | 0.80     | 1199    |
    | weighted avg | 0.80      | 0.80   | 0.80     | 1199    |

## 🚀挑战

在本课中，你使用清理后的数据构建了一个机器学习模型，可以根据一系列食材预测国家菜系。花些时间阅读 Scikit-learn 提供的多种分类数据选项。深入了解“solver”的概念，理解其背后的原理。

## [课后测验](https://gray-sand-07a10f403.1.azurestaticapps.net/quiz/22/)

## 复习与自学

深入学习逻辑回归背后的数学原理：[这节课](https://people.eecs.berkeley.edu/~russell/classes/cs194/f11/lectures/CS194%20Fall%202011%20Lecture%2006.pdf)

## 作业

[研究 solvers](assignment.md)

---

**免责声明**：  
本文档使用AI翻译服务 [Co-op Translator](https://github.com/Azure/co-op-translator) 进行翻译。尽管我们努力确保翻译的准确性，但请注意，自动翻译可能包含错误或不准确之处。应以原始语言的文档作为权威来源。对于关键信息，建议使用专业人工翻译。我们不对因使用此翻译而产生的任何误解或误读承担责任。