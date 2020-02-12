# log4j
## helloworld
> ### 1.引入依赖
```
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```
> ### 2.添加log4j.properties文件放在resource路径下
```
log4j.rootLogger=DEBUG, Console ,File  
   
#Console  
log4j.appender.Console=org.apache.log4j.ConsoleAppender  
log4j.appender.Console.layout=org.apache.log4j.PatternLayout  
log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n
   
#File
log4j.appender.File = org.apache.log4j.FileAppender
log4j.appender.File.File = C://log.log
log4j.appender.File.layout = org.apache.log4j.PatternLayout
log4j.appender.File.layout.ConversionPattern =%d [%t] %-5p [%c] - %m%n
```
> ### 3.生成logger对象
```
package com.open1111.log4j;
 
import org.apache.log4j.Logger;
 
/**
 * Log4j测试类
 * @author user
 *
 */
public class Test {
 
    private static Logger logger=Logger.getLogger(Test.class); // 获取logger实例
     
    public static void main(String[] args) {
        logger.info("普通Info信息");
        logger.debug("调试debug信息");
    }
}
```
## 日志输出方式
```
log4j.rootLogger=DEBUG, Console ,File ,DailyRollingFile ,RollingFile
   
#Console  
log4j.appender.Console=org.apache.log4j.ConsoleAppender  
log4j.appender.Console.layout=org.apache.log4j.PatternLayout  
log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n
   
#File
log4j.appender.File = org.apache.log4j.FileAppender
log4j.appender.File.File = C://log2.log
log4j.appender.File.layout = org.apache.log4j.PatternLayout
log4j.appender.File.layout.ConversionPattern =%d [%t] %-5p [%c] - %m%n

#DailyRollingFile(每天生产一个log文件)
log4j.appender.DailyRollingFile = org.apache.log4j.DailyRollingFileAppender
log4j.appender.DailyRollingFile.File = C://log3.log
log4j.appender.DailyRollingFile.layout = org.apache.log4j.PatternLayout
log4j.appender.DailyRollingFile.layout.ConversionPattern =%d [%t] %-5p [%c] - %m%n

#RollingFile(超过一定字节数生产新的日志文件)
log4j.appender.RollingFile = org.apache.log4j.RollingFileAppender
log4j.appender.RollingFile.File = C://log4.log
log4j.appender.RollingFile.MaxFileSize=1KB
log4j.appender.RollingFile.MaxBackupIndex=3
log4j.appender.RollingFile.layout = org.apache.log4j.PatternLayout
log4j.appender.RollingFile.layout.ConversionPattern =%d [%t] %-5p [%c] - %m%n
```
## 指定日志文件不追加
```
log4j.appender.FIEL.Append = false
```
## 指定日志输出等级
```
log4j.rootLogger=DEBUG, Console ,DFile ,EFILE

   

#Console  
log4j.appender.Console=org.apache.log4j.ConsoleAppender  

log4j.appender.Console.layout=org.apache.log4j.TTCCLayout  

#DEBUGFile
log4j.appender.DFile = org.apache.log4j.FileAppender

log4j.appender.DFile.File = C://DEBUG.log

log4j.appender.DFile.layout = org.apache.log4j.PatternLayout

log4j.appender.DFile.layout.ConversionPattern =%d [%t] %-5p [%c] - %m%n

log4j.appender.DFile.Threshold = DEBUG

#ERRORFile
log4j.appender.EFILE = org.apache.log4j.FileAppender

log4j.appender.EFILE.File = C://ERROR.log

log4j.appender.EFILE.layout = org.apache.log4j.PatternLayout

log4j.appender.EFILE.layout.ConversionPattern =%d [%t] %-5p [%c] - %m%n

log4j.appender.EFILE.Threshold = ERROR

```
```
rootLogger里配置DEBUG，

DFile的Threshold 配置为DEBUG   

EFILE的Threshold 配置为ERROR 只输入ERROR信息；
```
## 输出指定类的日志
```
log4j.logger.MyTest=DEBUG, Console ,File  
#只输出MyTest类产生的日志

```
## 常用输出属性
```
%m 输出代码中指定的消息；

%M 输出打印该条日志的方法名；

%p 输出优先级，即DEBUG，INFO，WARN，ERROR，FATAL；

%r 输出自应用启动到输出该log信息耗费的毫秒数；

%c 输出所属的类目，通常就是所在类的全名；

%t 输出产生该日志事件的线程名；

%n 输出一个回车换行符，Windows平台为"rn”，Unix平台为"n”；

%d 输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，比如：%d{yyyy-MM-dd HH:mm:ss,SSS}，输出类似：2002-10-18 22:10:28,921；

%l 输出日志事件的发生位置，及在代码中的行数；
```
> ## 根据classname获取一个配置好了的logger对象，调用即生成以时间命名的log文件
```


 
 
import java.text.SimpleDateFormat;
import java.util.Date;
 
import org.apache.log4j.FileAppender;
import org.apache.log4j.Level;
import org.apache.log4j.Logger;
import org.apache.log4j.PatternLayout;
import org.apache.log4j.RollingFileAppender;
 
public class LoggerUtil {
   
    public static Logger getLog(Class<?> clazz) {
       
        Logger logger = Logger.getLogger(clazz);  // 生成新的Logger
        logger.removeAllAppenders(); // 清空Appender，特別是不想使用現存實例時一定要初期化
        logger.setLevel(Level.DEBUG); // 设定Logger級別。
        logger.setAdditivity(true); // 设定是否继承父Logger。默认为true，继承root输出；设定false后将不出书root。
        
        FileAppender appender = new RollingFileAppender(); // 生成新的Appender
        PatternLayout layout = new PatternLayout();
        layout.setConversionPattern("[%d{yyyy-MM-dd HH:mm:ss}] %p %l : %m%n"); // log的输出形式
        appender.setLayout(layout);
        
        appender.setFile(getTime("yyyy-MM-dd") + ".log");  // log输出路径
        appender.setEncoding("UTF-8"); // log的字符编码
        appender.setAppend(true);  //日志合并方式： true:在已存在log文件后面追加 false:新log覆盖以前的log
        appender.activateOptions();  // 适用当前配置
        
        logger.addAppender(appender); // 将新的Appender加到Logger中
        return logger;
    }
    
    private static String getTime(String format) {
		SimpleDateFormat sdf = new SimpleDateFormat(format);
		return sdf.format(new Date());
    }
}
```



