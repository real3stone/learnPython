[my_github](https://github.com/real3stone)


【day 3&4 orm & models】

Error 1:
  报错：Event loop is closed
  原因：没有关闭mysql连接池

Error 2：
  报错：AttributeError: 'Connection' object has no attribute '_writer'”
  原因：models模块中User定义中数据库的编码应该写为'utf8'，而非'utf-8'
