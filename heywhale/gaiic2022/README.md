应官方要求，不能开源代码，这里把代码都删了，大家看看思路就行。。

## 思路说明
- 方案:当作13个类别的多标签分类来做

- TextCNN提取文本特征，简单两层FC处理图像特征，二者concate后经过一个分类头即可，torch.nn.MultiLabelSoftMarginLoss()

- 8G显卡就能训,单模单折89.4
```python
class_dict={'图文': ['符合','不符合'], 
            '版型': ['修身型', '宽松型', '标准型'], 
            '裤型': ['微喇裤', '小脚裤', '哈伦裤', '直筒裤', '阔腿裤', '铅笔裤', 'O型裤', '灯笼裤', '锥形裤', '喇叭裤', '工装裤', '背带裤', '紧身裤'],
            '袖长': ['长袖', '短袖', '七分袖', '五分袖', '无袖', '九分袖'], 
            '裙长': ['中长裙', '短裙', '超短裙', '中裙', '长裙'], 
            '领型': ['半高领', '高领', '翻领', 'POLO领', '立领', '连帽', '娃娃领', 'V领', '圆领', '西装领', '荷叶领', '围巾领', '棒球领', '方领', '可脱卸帽', '衬衫领', 'U型领', '堆堆领', '一字领', '亨利领', '斜领', '双层领'], 
            '裤门襟': ['系带', '松紧', '拉链'], 
            '鞋帮高度': ['低帮', '高帮', '中帮'], 
            '穿着方式': ['套头', '开衫'], 
            '衣长': ['常规款', '中长款', '长款', '短款', '超短款', '超长款'], 
            '闭合方式': ['系带', '套脚', '一脚蹬', '松紧带', '魔术贴', '搭扣', '套筒', '拉链'], 
            '裤长': ['九分裤', '长裤', '五分裤', '七分裤', '短裤'], 
            '类别': ['单肩包', '斜挎包', '双肩包', '手提包']
            }
```
## 构造负样本
具体思路为对title里面的关键词进行替换，并将对应的标签进行改变，实现代码见make_neg_samole_v1.py，思路不难，但有些细节需要注意，代码里面加了注释。

**举例：**
- 正样本:
'2021年春季微喇裤牛仔裤蓝色常规厚度九分裤女装'
- 负样本:
    - '2021年春季背带裤牛仔裤蓝色常规厚度短裤女装', 
    - '2021年春季锥形裤牛仔裤蓝色常规厚度短裤女装'
## 代码运行说明

```bash
python make_neg_sample_v1.py # 构造负样本
python train.py #五折代码，可以只训练一折
python infer.py #推理代码
```

