---
title: 模板引擎
date: 2018-12-28 15:32:05
tags: python
---
模板引擎在`web`开发中十分常见，用`python`实现的模板引擎有很多种，如`jinja2`、`mako`,很多`web`框架也自带模板引擎，如`tornado`、`django`等。模板是为了将视图与数据分离而出现的，模板引擎是为了将包含小部分逻辑和大量文本数据的工作简化而诞生的.
在研究模板引擎的时候发现的一段很有意思的话，来自[Ned Batchelder](http://aosabook.org/en/500L/a-template-engine.html)
> Most programs contain a lot of logic, and a little bit of literal textual data. Programming languages are designed to be good for this sort of programming. But some programming tasks involve only a little bit of logic, and a great deal of textual data. For these tasks, we'd like to have a tool better suited to these text-heavy problems. A template engine is such a tool.

<!-- more -->
模板引擎主要实现了这样的功能：
```python
PAGE_HTML = """
<html>
  <p>Hello, {{ username }}!</p>
  <p>here are some lessons</p>
  <ul>
    {% for lesson in lessons %}
      <li>{{ lesson }}</li>
    {% end %}
  </ul>
</html>

"""
t = Template(PAGE_HTML)
result = t.render(username="zero", lessons=["math", "physics", "computer science"])
print(result)
# result
"""
<html>
  <p>Hello, zero!</p>
  <p>here are some lessons</p>
  <ul>
    
      <li>math</li>
    
      <li>physics</li>
    
      <li>computer science</li>
    
  </ul>
</html>
"""
```

模板引擎的工作原理与编译原理有些相似，主要分几个步骤：

### 1. 解析模板：
读入模板文件，将字符流解析成一个个`token`,主要根据一些定义好的语法标记，如`{ { expression } }`,`{ % block % }`,`{# comment #}`,将文本切分为一个个标记，如上面的模板将切分成:
```python
[
    '\n<html>\n  <p>Hello, ', # text
    '{ { username } }', # expression
    '!</p>\n  <p>here are some lessons</p>\n  <ul>\n    ', # text
    '{ % for lesson in lessons % }', # block start
    '\n      <li>', # text
    '{ { lesson } } ', # expression
    '</li>\n    ', # text
    '{ % end % }', # block end
    '\n  </ul>\n</html>\n\n' # text
]
```

### 2. 生成代码：
生成编译代码，对应每一种`token`，生成对应的实现代码，
```python
def _execute():
    _buff=list()
    _buff.append('\n<html>\n  <p>Hello, ')
    _buff.append(username)
    _buff.append('!</p>\n  <p>here are some lessons</p>\n  <ul>\n    ')
    for lesson in lessons:
        _buff.append('\n      <li>')
        _buff.append(lesson)
        _buff.append('</li>\n    ')
    _buff.append('\n  </ul>\n</html>\n\n')
    return ''.join(_buff)
```

### 3. 将数据填入上下文环境环境，执行编译代码:
```python
def render(execute_function,**context):
    namespace = dict()
    namespace.update(context)
    code = compile(execute_function(), "<string>", "exec", dont_inherit=True)
    exec(code, namespace)
    execute = namespace["_execute"]
    return execute()
```
