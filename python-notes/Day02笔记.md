# Day02笔记

ps：今天开始笔记全手写，小女仆写的太多太啰嗦了

>日期：11.2

---

## 必背代码

1.查看类型

>>>print(type(变量))

例：
```python
print(type(1.8))
##输出class float
print(type("1.8"))
##输出class str
print(type(True))
##输出class bool
print(type(int(True)))
##输出class int,值为1，Flase则为0
```


2.类型转换

>>>int/float/str/bool()

例：
```py
print(float(int(bool(str(1)))))
print(type(float(int(bool(str(1))))))
##输出：1.0
##    class float
##整型数据1先转化成字符串"1"，然后转换成布尔值True,然后用被转化成整型数据1，最后被转换成浮点数据1.0
```
>补充：float转int会截断小数部分，想要四舍五入用round(),特殊记：round(2.5)值为2


3.f-str格式化

>>>print(f"xx{变量1}xxx{变量2}xxx")

例：
```py

age=int(input("你今年几岁？"))

print(f"我今年{age}岁")

```

>补充：变量对应的浮点数据可以f"{变量:.nf}"控制小数位数(n填几就是几位)
>对应的整形大数据可以f"{变量：，}"千分位分格


---