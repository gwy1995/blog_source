---
title: 正则表达式--零宽断言
date: 2018-12-14 14:33:35
tags: regexp 
---
零宽断言是正则表达式匹配的一种方法，零宽的字面意思是匹配的字符宽度为零，断言是编程术语，表示为一些布尔表达式，表示相信在程序中的某个特定点该表达式值为真，因此零宽断言就是判断所匹配的正则表达式前后的字符是否符合特定的正则模式，若是，则匹配当前正则表达式，若否则不匹配。

零宽断言共有四种，分别用来匹配表达式前后出现或不出现某些模式。

<!-- more -->

1. 零宽度正预测先行断言: `(?=exp)`

举例：

匹配`111c222`中`c`前面的数字

```python
import re
s="111c222"
pattern=re.compile(r"\d+(?=c)")
pattern.findall(s)
# ['111']
```

2. 零宽度正回顾后发断言: `(?<=exp)`

举例：

匹配`111c222`中`c`后面的数字

```python
import re
s="111c222"
pattern=re.compile(r"(?<=c)\d+")
pattern.findall(s)
# ['222']
```

3. 零宽度负预测先行断言: `(?!exp)`

举例：

匹配`111c222`中后面没有`c`的数字

```python
import re
s="111c222"
pattern=re.compile(r"\d+(?!c)")
pattern.findall(s)
# ['11', '222']
```

4. 零宽度负回顾后发断言: `(?<!exp)`

举例：

匹配`111c222`中前面没有`c`的数字

```python
import re
s="111c222"
pattern=re.compile(r"(?<!c)\d+")
pattern.findall(s)
# ['111', '22']
```
