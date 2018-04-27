## inspect 模块	

[转自](https://blog.csdn.net/weixin_35955795/article/details/53053762)

----
[**inspect模块 官方文档**](https://docs.python.org/3.6/library/inspect.html?highlight=inspect#module-inspect)

**inspect模块四大功能：**

1、类型检查（type checking）

2、获取源码（getting source code）

3、获取类和方法的参数信息（inspecting classes and functions）

4、解析堆栈（examining the interpreter stack）


学习廖雪峰Python web实战时，主要用到第三个功能：
#### 总结
* `inspect.signature（fn)`： 返回一个`inspect.Signature`类型的对象，值为fn这个函数的所有参数

* `inspect.Signature`对象的`paramerters`属性： 一个`mappingproxy`（映射）类型的对象，值为一个有序字典（`Orderdict`)；

	> 这个字典里的`key`即为**参数名**，str类型
这个字典里的`value`是一个`inspect.Parameter`类型的对象

* `inspect.Parameter`对象的`kind`属性: 一个`_ParameterKind`枚举类型的对象，值为这个参数的类型（可变参数，关键词参数，etc）

* `inspect.Parameter`对象的`default`属性： 如果这个参数有默认值，即返回这个默认值，如果没有，返回一个`inspect._empty`类。


#### 代码：

```python
import inspect

def a(a, b=0, *c, d, e=1, **f):
    pass

aa = inspect.signature(a)
print("inspect.signature(fn)是:%s" % aa)
print("inspect.signature(fn)的类型：%s" % (type(aa)))
print("\n")

bb = aa.parameters
print("signature.paramerters属性是:%s" % bb)
print("ignature.paramerters属性的类型是%s" % type(bb))
print("\n")

for cc, dd in bb.items():
    print("mappingproxy.items()返回的两个值分别是：%s和%s" % (cc, dd))
    print("mappingproxy.items()返回的两个值的类型分别是：%s和%s" % (type(cc), type(dd)))
    print("\n")
    ee = dd.kind
    print("Parameter.kind属性是:%s" % ee)
    print("Parameter.kind属性的类型是:%s" % type(ee))
    print("\n")
    gg = dd.default
    print("Parameter.default的值是: %s" % gg)
    print("Parameter.default的属性是: %s" % type(gg))
    print("\n")


ff = inspect.Parameter.KEYWORD_ONLY
print("inspect.Parameter.KEYWORD_ONLY的值是:%s" % ff)
print("inspect.Parameter.KEYWORD_ONLY的类型是:%s" % type(ff))

```
**输出结果：**

```
inspect.signature（fn)是:(a, b=0, *c, d, e=1, **f)
inspect.signature（fn)的类型：<class 'inspect.Signature'>


signature.paramerters属性是:OrderedDict([('a', <Parameter "a">), ('b', <Parameter "b=0">), ('c', <Parameter "*c">), ('d', <Parameter "d">), ('e', <Parameter "e=1">), ('f', <Parameter "**f">)])
ignature.paramerters属性的类型是<class 'mappingproxy'>


mappingproxy.items()返回的两个值分别是：a和a
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:POSITIONAL_OR_KEYWORD
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: <class 'inspect._empty'>
Parameter.default的属性是: <class 'type'>


mappingproxy.items()返回的两个值分别是：b和b=0
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:POSITIONAL_OR_KEYWORD
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: 0
Parameter.default的属性是: <class 'int'>


mappingproxy.items()返回的两个值分别是：c和*c
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:VAR_POSITIONAL
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: <class 'inspect._empty'>
Parameter.default的属性是: <class 'type'>


mappingproxy.items()返回的两个值分别是：d和d
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:KEYWORD_ONLY
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: <class 'inspect._empty'>
Parameter.default的属性是: <class 'type'>


mappingproxy.items()返回的两个值分别是：e和e=1
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:KEYWORD_ONLY
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: 1
Parameter.default的属性是: <class 'int'>


mappingproxy.items()返回的两个值分别是：f和**f
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:VAR_KEYWORD
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: <class 'inspect._empty'>
Parameter.default的属性是: <class 'type'>


inspect.Parameter.KEYWORD_ONLY的值是:KEYWORD_ONLY
inspect.Parameter.KEYWORD_ONLY的类型是:<enum '_ParameterKind'>
```
