#### 电影评论（二分类问题）
1. 加载数据集
代码如下, 首次运行时会从aws下载互联网电影数据库（IMDB）的数据集，包含50000条两极分化的评论数据。
```python
from keras.datasets import imdb
# 提取数据中最常出现的1000个单词
(train_data, train_labels), (test_data, test_labels) = imdb.load_data(num_words=1000) 
 ```
其中，*_data和*—labels分别表示评论列表（单词转换为了整数索引，张量运算不支持字符串）和评价（0->正面，1->负面）；
查看原评论的方式如下：
```python
word_index = imdb.get_word_index()  # 单词->整数映射词典
reverse_word_index = dict(
    [(value, key) for (key, value) in word_index.items()]
)
decoded_review = ' '.join(
    [reverse_word_index.get(i - 3 , '?') for i in train_data[0]]) #索引减3是因为0,1,2位保留索引
print decoded_review
```

2. 填充数据
```python
import numpy as np

def vectorize_sequences(sequences, dimensions=1000):
    results = np.zeros((len(sequences), dimensions))
    for i, sequence in enumerate(sequences):
        results[i, sequence] = 1.
    return results

x_train = vectorize_sequences(train_data)
x_test = vectorize_sequences(test_data)

y_train = np.asarray(train_labels).astype('float32')
y_test = np.asarray(test_labels).astype('float32')
 ```
 
3. 构建网络
1,2 层均选用了relu（rectified linera unit, 该函数将负值归0）激活函数（output = relu(dot(W, input) + b），16个隐藏单元（神经元）;
```python
from keras import models
from keras import layers

model = models.Sequential()
model.add(layers.Dense(16, activation='relu', input_shape=(1000,)))
model.add(layers.Dense(16, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))
```

4. 数据分为训练集和验证集
为了在训练过程中监控模型在未知数据上的精度，将样本数据中留出10000条作为验证集，如下：
```python
x_val = x_train[:10000]
partial_x_train = x_train[10000:]

y_val = y_train[:10000]
partial_y_train = y_train[10000:]
```

5. 训练模型
```python
model.compile(optimizer='rmsprop',
             loss='binary_crossentropy',
             metrics=['accuracy'])

history = model.fit(partial_x_train,
                   partial_y_train,
                   epochs=20,
                   batch_size=512,
                   validation_data=(x_val, y_val))
```
上面表示，使用rmsprop作为优化器（一种SGD——随机梯度下降算法实现），binary_crossentropy作为损失函数；迭代20轮，每次批量取出512条样本数据；

6. 绘图验证
- 绘制训练损失和验证损失图
```python
import matplotlib.pyplot as plt

history_dict = history.history
loss_values = history_dict['loss']
val_loss_values = history_dict['val_loss']

epochs = range(1, len(loss_values) + 1)

plt.plot(epochs, loss_values, 'bo', label='Training loss')
plt.plot(epochs, val_loss_values, 'b', label='Validation loss')
plt.title('Training and validation loss')
plt.xlabel('Epochs')
plt.ylabel('Loss')
plt.legend()

plt.show()
```
- 绘制训练精度和验证精度（正确率）
```python
acc_values = history_dict['acc']
val_act_values = history_dict['val_acc']

epochs = range(1, len(acc_values) + 1)

plt.plot(epochs, acc_values, 'bo', label='Training acc')
plt.plot(epochs, val_act_values, 'b', label='Validation acc')
plt.title('Training and validation accuracy')
plt.xlabel('Epochs')
plt.ylabel('Accuracy')
plt.legend()

plt.show()
```
7. 模型预测
```python
model.predict(x_test)
```
