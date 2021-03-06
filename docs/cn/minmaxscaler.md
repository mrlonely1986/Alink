## 功能介绍

归一化是对数据进行归一的组件, 将数据归一到min和max之间。

## 参数说明


<!-- OLD_TABLE -->
<!-- This is the start of auto-generated parameter info -->
<!-- DO NOT EDIT THIS PART!!! -->
| 名称 | 中文名称 | 描述 | 类型 | 是否必须？ | 默认值 |
| --- | --- | --- | --- | --- | --- |
| selectedCols | 选择的列名 | 计算列对应的列名列表 | String[] | ✓ |  |
| min | 归一化的下界 | 归一化的下界 | Double |  | 0.0 |
| max | 归一化的上界 | 归一化的上界 | Double |  | 1.0 |
| outputCols | 输出结果列列名数组 | 输出结果列列名数组，可选，默认null | String[] |  | null |<!-- This is the end of auto-generated parameter info -->


## 脚本示例

#### 脚本


```
data = np.array([
            ["a", 10.0, 100],
            ["b", -2.5, 9],
            ["c", 100.2, 1],
            ["d", -99.9, 100],
            ["a", 1.4, 1],
            ["b", -2.2, 9],
            ["c", 100.9, 1]
])
             
colnames = ["col1", "col2", "col3"]
selectedColNames = ["col2", "col3"]

df = pd.DataFrame({"col1": data[:, 0], "col2": data[:, 1], "col3": data[:, 2]})
inOp = dataframeToOperator(df, schemaStr='col1 string, col2 double, col3 long', op_type='batch')


sinOp = dataframeToOperator(df, schemaStr='col1 string, col2 double, col3 long', op_type='stream')
                   

model = MinMaxScaler()\
           .setSelectedCols(selectedColNames)\
           .fit(inOp)

model.transform(inOp).print()

model.transform(sinOp).print()

StreamOperator.execute()
```

#### 脚本运行结果

```
  col1      col2      col3
0    a  0.547311  1.000000
1    b  0.485060  0.080808
2    c  0.996514  0.000000
3    d  0.000000  1.000000
4    a  0.504482  0.000000
5    b  0.486554  0.080808
6    c  1.000000  0.000000
```