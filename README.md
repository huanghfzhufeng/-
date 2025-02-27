# 酒品质分析

## 一、项目概述
本项目旨在对酒的品质进行分析，通过对酒的 11 种特征数据进行探索性数据分析（EDA），了解数据特征与酒品质之间的关系，并构建模型对酒的品质进行分类预测。

## 二、数据读取与初步分析

### 数据读取
- 导入所需的库，包括 `pandas`、`matplotlib`、`seaborn`、`numpy`、`sklearn` 等，用于数据处理、可视化和模型构建。
- 读取训练集（`Dataset.csv`）和测试集（`Testing.csv`）数据，去除 `ID` 列，因为其对分析和预测无实际作用。

### 数据概览
- 查看训练集和测试集的基本信息，包括数据类型、是否有缺失值等。数据集中所有特征均为数值型数据，且无缺失值。
- 分析数据分布情况，获取各特征的均值、标准差、最小值、最大值等统计信息。例如，固定酸度（`fixed acidity`）的均值为 6.860228，标准差为 0.860553。
- 观察酒类数量分布，发现样本不均衡，可能对少数样本的预测和分析造成困难。质量为 3 的酒数量较多，而质量为 9 的酒数量较少。

## 三、酒品质相关因素分析

### 固定酸度（fixed acidity）与酒品质的关系
- 绘制柱状图和小提琴图，以酒的质量（`quality`）为分组依据，分析固定酸度在不同质量酒中的均值和分布情况。
- 从平均值看，不同质量酒的固定酸度大体上无明显区别，但质量为 3 和 4 的酒固定酸度相对较高。从分布规律看，质量为 7、8、9 的酒在固定酸度层面上分布更均匀，可推测酒的质量越好，固定酸度分布越平均。

### 挥发性酸度（volatile acidity）与酒品质的关系
- 同样绘制柱状图和小提琴图，以质量分组计算挥发性酸度的均值并展示分布。
- 质量为 4 的酒挥发性酸度比其他酒含量更高，质量为 5、6、7、8、9 的酒在该特征上分布相似，可认为酒质量越好，挥发性酸度偏低。

### 柠檬酸（citric acid）与酒品质的关系
- 通过分组计算柠檬酸的均值并绘制图表进行分析。
- 质量为 9 的酒柠檬酸含量相较于其他质量的酒偏高，但不能明确说明柠檬酸对酒质量有明显影响。

### 酒内残余糖量（residual sugar）与酒品质的关系
- 分析酒内残余糖量在不同质量酒中的均值和分布。
- 质量为 9 和 4 的酒残糖量明显比其他酒少，质量为 7 和 8 的酒可分为同一类，其余单独为一类。从小提琴图可看出质量为 6 的酒在柠檬酸特征上可能存在离异值，且残糖量与酒的特点相关，不同酒含糖量不同。

### 氯化物含量（chlorides）与酒品质的关系
- 绘制图表分析氯化物含量与酒品质的关系。
- 大致认为质量越好的酒，氯化物含量越低。

### 游离二氧化硫（free sulfur dioxide）与酒品质的关系
- 以质量分组计算游离二氧化硫的均值并绘制图表。
- 质量为 4 的酒游离二氧化硫较少，质量为 3 的酒较多，但数据较少可能不具代表性。

### 总二氧化硫（total sulfur dioxide）与酒品质的关系
- 分析总二氧化硫在不同质量酒中的均值和分布。
- 总二氧化硫和游离二氧化硫分布规律不一致，但质量为 3 的酒总二氧化硫含量高，且总二氧化硫含量偏少可能品质更好。

### 密度（density）与酒品质的关系
- 绘制图表展示密度与酒品质的关系。
- 质量为 3 的酒密度较高，质量为 7、8、9 的高质量酒密度偏低，但特征不明显，密度在一定区间内可能较好。

### pH 值与酒品质的关系
- 分析 pH 值在不同质量酒中的均值和分布。
- 质量为 9 的酒 pH 值最高，其余质量的酒 pH 值无太大区别。

### 硫酸盐含量（sulphates）与酒品质的关系
- 绘制图表研究硫酸盐含量与酒品质的关系。
- 从硫酸盐含量未得到特别有用信息。

### 酒精含量（alcohol）与酒品质的关系
- 分析酒精含量在不同质量酒中的均值和分布。
- 品质越好的酒酒精含量貌似越高。

## 四、总体特征分析

### 箱线图分析总体特征分布
- 绘制总体特征的箱线图，观察各特征数据分布情况。
- 发现 `fixed acidity`、`citric acid`、`residual sugar`、`total sulfur dioxide`、`free sulfur dioxide`、`density` 等特征存在离异值，`chlorides` 的分布较特殊。

### 相关性矩阵分析特征相关性
- 计算特征之间的相关系数矩阵并绘制热力图。
- 密度和剩余糖量、酒精含量相关性较高，其他特征互相之间相关性不高，这种特征关系利于模型学习。

## 五、数据处理

### 重新读取数据
- 再次读取训练集和测试集数据，去除 `ID` 列，并将质量（`quality`）列从训练集中分离出来，因为其是预测目标。

### 归一化处理
- 使用 `MinMaxScaler` 对所有数值型数据进行归一化处理，将数据映射到 [0, 1] 区间，有助于提高模型收敛速度和性能表现。通过对训练集和测试集的合并进行归一化，确保两者在相同归一化范围内，避免测试集预测偏差。

### 数据分割
- 将归一化后的数据分割为训练集和测试集，准备用于模型训练和评估。

## 六、模型训练与评估

### 决策树模型训练（基线模型）
- 使用决策树模型作为基线模型，直接将处理好的数据送入模型训练，不进行训练集和验证集的划分。
- 训练完成后，使用模型对测试集进行预测，并将预测结果封装成 csv 文件提交。

### 模型优化
- **更换模型为随机森林**：使用随机森林模型替代决策树模型，其他步骤与决策树模型训练相同，模型性能得到提升（分数从 0.41 提升到 0.524）。
- **K 折交叉验证**：采用 5 折交叉验证对随机森林模型进行训练和评估，计算平均的 F1 分数，减少模型过拟合，进一步提升模型性能（分数提升到 0.525）。
- **数据重采样**：去除训练集中数量极少的质量为 3 和 9 的数据，然后对数据进行重采样，使数据分布更均衡，再使用随机森林模型训练和预测，模型性能再次提升（分数达到 0.533）。
- **特征工程**：分析特征之间的联系，对部分特征进行对数变换（如 `fixed acidity`、`volatile acidity` 等），使数据分布更接近正态分布，同时构造新的特征列（如 `alcohol mul pH` 等），丰富模型输入，经过这些处理后，模型性能得到进一步改善（平均 F1 分数为 0.5922668493588595）。

## 七、总结与展望
本项目通过对酒品质数据集的分析和处理，构建了多个模型对酒的品质进行分类预测，并通过多种优化方法不断提升模型性能。
- 代码仍有许多可优化点，如进一步调整模型参数、进行更复杂的特征工程、采用更多的数据处理方法解决样本不平衡问题、使用更高级的模型融合技术等，以提高模型的准确性和泛化能力。
- 未来可继续探索这些优化方向，提升酒品质分类预测的效果。