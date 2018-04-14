[my_github](https://github.com/real3stone)


【day 3&4 orm & models】

Error 1:
  报错：Event loop is closed
  原因：没有关闭mysql连接池
  后续：一会好一会坏，还是没有搞懂连接池的运行原理

Error 2：
  报错：AttributeError: 'Connection' object has no attribute '_writer'”
  原因：models模块中User定义中数据库的编码应该写为'utf8'，而非'utf-8'

Error 3：
  报错：pymysql.err.IntegrityError: (1062, "Duplicate entry 'stone@mail.com' for key 'idx_email'")
  原因：因为数据库设计时把email设为unique key了，test.py测试时不能重复插入统一邮箱地址
