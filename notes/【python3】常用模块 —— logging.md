# logging 模块
转载自：https://www.cnblogs.com/wenwei-blog/p/7196658.html

-----

### 日志级别

```
logging.debug(msg, *args, **kwargs)

logging.info(msg, *args, **kwargs)

logging.warning(msg, *args, **kwargs)

logging.error(msg, *args, **kwargs)

logging.critical(msg, *args, **kwargs)
```
日志级别排序 CRITICAL > ERROR > WARNING > INFO > DEBUG > NOTSET

**示例：**

```python3
import logging
logging.info('this is info message')
logging.debug('this is debug message')
logging.warning('this is warning message')
logging.error('this is error message')
logging.critical('this is critical message')
```
**结果：**

```
WARNING:root:this is warning message
ERROR:root:this is error message
CRITICAL:root:this is critical message
```
默认只打印info级别以上的信息，即打印warning, error, critical级别的信息

----
### logging.basicConfig()
> 该函数用来修改日志的输出格式和方式，将修改后的日志内容写入到一个文件中

**可选参数：**

```
filename  # 指定日志文件名

filemode # 指定日志文件打开的模式（w 或 a）

level  # 指定日志级别，默认logging.WARNING(必须大写)

datefmt  # 指定时间格式，forma 参数中有asctime的话，需要使用datefmt指定格式

format  # 指定输出的格式和内容，format的参考信息如下
```
format输出格式参数：

```
%(levelno)s:    打印日志级别的数值
%(levelname)s:  打印日志级别名称
%(pathname)s:   打印当前执行程序的路径，其实就是sys.argv[0]
%(filename)s:   打印当前执行程序名
%(funcName)s:   打印日志的当前函数
%(lineno)d:     打印日志的当前行号
%(asctime)s:    打印日志的时间
%(thread)d:     打印线程ID
%(threadName)s: 打印线程名称
%(process)d:    打印进程ID
%(message)s:    打印日志信息
```
datefmt 参数和 time() 模块中的参数一致

**示例：**

```
import logging
logging.basicConfig(level=logging.DEBUG, format='%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s', datefmt='%Y/%m/%d %H:%M:%S', filename='test.log', filemode='w')

logger = logging.getLogger(__name__)  # 每个程序输出前都要获取一个logger，通常对应程序的模块名（后有详解）
logging.debug('this is debug message')
logging.info('this is info message')
logging.warning('this is warning message')
```
输出结果(在文件test.log中)：

```
2018/04/27 10:40:09 test.py[line:6] DEBUG this is debug message
2018/04/27 10:40:09 test.py[line:7] INFO this is info message
2018/04/27 10:40:09 test.py[line:8] WARNING this is warning message
```
----
### logging 高级功能
若要对logging进行更灵活的控制，需要了解 logger, Handler, Formatter, Filter的概念：

**logger：** 提供了应用程序可以直接使用的接口；

**handler：** 将(logger创建的)日志记录发送到合适的目的输出；

**filter：** 提供了细度设备来决定输出哪条日志记录；

**formatter：** 决定日志记录的最终输出格式。

#### logger

每个程序在输出信息之前都要获得一个Logger。
Logger通常对应了程序的模块名，比如：
> 聊天工具的图形界面模块可以这样获得它的Logger：
	  LOG=logging.getLogger(”chat.gui”)
而核心模块可以这样：
	LOG=logging.getLogger(”chat.kernel”)

**Logger.setLevel(lel):**
指定最低的日志级别，低于lel的级别将被忽略。debug是最低的内置级别，critical为最高
**Logger.addFilter(filt)、Logger.removeFilter(filt):**
添加或删除指定的filter
**Logger.addHandler(hdlr)、Logger.removeHandler(hdlr)：**
增加或删除指定的handler
**Logger.debug()、Logger.info()、Logger.warning()、Logger.error()、Logger.critical()：**
可以设置的日志级别


#### handler
handler对象：负责发送相关的信息到指定目的地。

Python的日志系统有多种Handler可以使用。有些Handler可以把信息输出到控制台，有些Logger可以把信息输出到文件，还有些 Handler可以把信息发送到网络上。如果觉得不够用，还可以编写自己的Handler。可以通过addHandler()方法添加多个多handler

**Handler.setLevel(lel):** 指定被处理的信息级别，低于lel级别的信息将被忽略
**Handler.setFormatter()：** 给这个handler选择一个格式
**Handler.addFilter(filt)、Handler.removeFilter(filt)：** 新增或删除一个filter对象


每个Logger可以附加多个Handler。接下来我们就来介绍一些**常用的Handler：**
##### **1) logging.StreamHandler**

使用这个Handler可以向类似与sys.stdout或者sys.stderr的任何文件对象(file object)输出信息。它的构造函数是：
**StreamHandler([strm])**
其中strm参数是一个文件对象。默认是sys.stderr


##### 2) logging.FileHandler

和StreamHandler类似，用于向一个文件输出日志信息。不过FileHandler会帮你打开这个文件。它的构造函数是：
**FileHandler(filename[,mode])**
filename是文件名，必须指定一个文件名。
mode是文件的打开方式。参见Python内置函数open()的用法。默认是’a'，即添加到文件末尾。

##### 3) logging.handlers.RotatingFileHandler

这个Handler类似于上面的FileHandler，但是它可以管理文件大小。当文件达到一定大小之后，它会自动将当前日志文件改名，然后创建 一个新的同名日志文件继续输出。比如日志文件是chat.log，当chat.log达到指定的大小之后，RotatingFileHandler自动把 文件改名为chat.log.1。不过，如果chat.log.1已经存在，会先把chat.log.1重命名为chat.log.2。最后重新创建 chat.log，继续输出日志信息。
它的构造函数是：
**RotatingFileHandler( filename[, mode[, maxBytes[, backupCount]]])**
其中filename和mode两个参数和FileHandler一样。	
**maxBytes**用于指定日志文件的最大文件大小。如果maxBytes为0，意味着日志文件可以无限大，这时上面描述的重命名过程就不会发生。
**backupCount**用于指定保留的备份文件的个数。比如，如果指定为2，当上面描述的重命名过程发生时，原有的chat.log.2并不会被更名，而是被删除。


##### 4) logging.handlers.TimedRotatingFileHandler

这个Handler和RotatingFileHandler类似，不过，它没有通过判断文件大小来决定何时重新创建日志文件，而是间隔一定时间就 自动创建新的日志文件。重命名的过程与RotatingFileHandler类似，不过新的文件不是附加数字，而是当前时间。它的构造函数是：
**TimedRotatingFileHandler( filename [,when [,interval [,backupCount]]])**
其中filename参数和backupCount参数和RotatingFileHandler具有相同的意义。
**interval**是时间间隔。
**when**参数是一个字符串。表示时间间隔的单位，不区分大小写。它有以下取值：

S 秒

M 分

H 小时

D 天

W 每星期（interval==0时代表星期一）

midnight 每天凌晨


**屏幕输出**
```
import logging
logger = logging.getLogger() #定义对应的程序模块名name，默认是root
#logger.setLevel(logging.DEBUG) #指定最低的日志级别
ch = logging.StreamHandler() #日志输出到屏幕控制台
ch.setLevel(logging.WARNING) #设置日志等级

fh = logging.FileHandler('access.log')#向文件access.log输出日志信息
fh.setLevel(logging.INFO) #设置输出到文件最低日志级别

#create formatter
formatter = logging.Formatter('%(asctime)s %(name)s- %(levelname)s - %(message)s') #定义日志输出格式

#add formatter to ch and fh
ch.setFormatter(formatter) #选择一个格式
fh.setFormatter(formatter)

logger.addHandler(ch) #增加指定的handler
logger.addHandler(fh)
# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')
```
文件输出
```
import logging,os
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__))) #获取上级目录的绝对路径
log_dir = BASE_DIR + '/log/record.log'
def get_logger():
    fh = logging.FileHandler(log_dir,encoding='utf-8') #创建一个文件流并设置编码utf8
    logger = logging.getLogger() #获得一个logger对象，默认是root
    logger.setLevel(logging.DEBUG)  #设置最低等级debug
    fm = logging.Formatter("%(asctime)s --- %(message)s")  #设置日志格式
    logger.addHandler(fh) #把文件流添加进来，流向写入到文件
    fh.setFormatter(fm) #把文件流添加写入格式
    return logger
```

-------------------
