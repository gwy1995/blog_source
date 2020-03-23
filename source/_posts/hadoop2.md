---
title: hadoop streaming
tags:
  - hadoop
  - Big Data
date: 2018-11-26 13:57:58
---
hadoop streaming可以将数据以stdio的形式传入mapreduce程序，从而使用者可以脱离java语言去写mapreduce程序。

hadoop streaming使用demo

<!-- more -->
## demo
- 下载对应版本的[hadoop streaming的jar包][hadoop_streaming]

- 输入文件
`/tmp/text1`
```text
A B C D E F
B A H C D E I
C B E G A J
D A B E
E H A B C D G
F A J G
G C E F I
H B J E
I G B
J H C F
```

- map和reduce程序
`mapper.py`
```python
#!/usr/bin/env python
import sys

x=list()
for line in sys.stdin:
    line = line.strip()
    line=line.split(" ")
    x+=line
for v in x:
    print(v)
```

`reducer.py`
```python
#!/usr/bin/env python
import sys
import itertools

x=dict()
for line in sys.stdin:
    k = line.strip()
    v=x.get(k,0)+1
    x[k]=v
for k,v in x.items():
    print(k,v)
```

- 测试命令
```shell
cat text1 | python mapper.py| python reducer.py
```

- hadoop streaming运行命令
```shell
bin/hadoop jar hadoop-streaming-2.7.7.jar -files "./tmp/mapper.py","./tmp/reducer.py" -input /tmp/text1 -output /tmp/output -mapper "python mapper.py" -reducer "python reducer.py"
```

- 输出结果
`/tmp/output`
```text
('A', 6)
('C', 6)
('B', 7)
('E', 7)
('D', 4)
('G', 5)
('F', 4)
('I', 3)
('H', 4)
('J', 4)
```


[hadoop_streaming]: https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-streaming