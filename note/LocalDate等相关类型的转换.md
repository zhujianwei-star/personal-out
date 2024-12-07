LocalDateTime 及 LocalDate 是 Java8 的新特性，有时为了兼容 Date 类型需要进行转换。

## LocalDateTime转LocalDate

``` java
// 直接调用toLocalDate()方法
LocalDateTime localDateTime = LocalDateTime.now();
LocalDate localDate = localDateTime.toLocalDate();
```

## LocalDateTime转Date

```java
// 在LocalDateTime转Date时，需要用到几个Java8的类
// 	ZoneId/ZoneOffset：表示时区
// 	ZoneDateTime：表示特定时区的日期和时间
// 	Instant：表示时刻，不直接对应年月日信息，需要通过时区转换
LocalDateTime localDateTime = LocalDateTime.now();
// 获取系统默认时区
ZoneId zoneId = ZoneId.systemDefault();
//时区的日期和时间 
ZonedDateTime zonedDateTime = localDateTime.atZone(zoneId); 
//获取时刻 
Date date = Date.from(zonedDateTime.toInstant()); 
System.out.println("格式化前：localDateTime:" + localDateTime + " Date:" + date); 
//格式化LocalDateTime、Date 
DateTimeFormatter localDateTimeFormat = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"); 
SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"); System.out.println("格式化后：localDateTime:" + localDateTimeFormat.format(localDateTime) + " Date:" + dateFormat.format(date));
```

输出结果如下：

```java
// 格式化前：
localDateTime:2020-10-27T11:35:09.969 Date:Tue Oct 27 11:35:09 CST 2020 
// 格式化后：
localDateTime:2020-10-27 11:35:09 Date:2020-10-27 11:35:09
```

## LocalDate转LocalDateTime

``` java
// 一般使用atTime()方法进行赋值
// 获取一天的开始时间，即00:00:00
LocalDate localDate = LocalDate.now();
LocalDateTime localDateTime1 = localDate.atStartOfDay(); 
// 获取传入时间的类型即8:20:33
LocalDateTime localDateTime2 = localDate.atTime(8,20,33); 
LocalDateTime localDateTime3 = localDate.atTime(LocalTime.now());
```

## LocalDate转Date

``` java
// 先调用 atStartOfDay() 方法转 LocalDateTime 再转 Date
LocalDate localDate = LocalDate.now(); 
ZoneId zoneId = ZoneId.systemDefault(); 
Date date = Date.from(localDate.atStartOfDay().atZone(zoneId).toInstant());
```

## Date转LocalDateTime

``` java
// 先转 ZonedDateTime 再转 LocalDateTimeDate date = new Date();
ZoneId zoneId = ZoneId.systemDefault();
LocalDateTime localDateTime = date.toInstant().atZone(zoneId).toLocalDateTime();
```

## Date转LocalDate

``` java 
// 跟 LocalDateTime 同理Date date = new Date();
ZoneId zoneId = ZoneId.systemDefault();
LocalDate localDate = date.toInstant().atZone(zoneId).toLocalDate();
```

## LocalDateTime与String的相互转换

``` java
DateTimeFormatter df = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime time = LocalDateTime.now();
// LocalDateTime转String
String localTime = df.format(time);
// String转LocalDateTime
LocalDateTime ldt = LocalDateTime.parse("2017-09-28 17:07:05",df);
```

## LocalDate转String

``` java
LocalDate date = LocalDate.now();
DateTimeFormatter fmt = DateTimeFormatter.ofPattern("yyyy-MM-dd");
// LocalDate转String
String dateStr = date.format(fmt);
System.out.println("LocalDate转String:"+dateStr);
// String转LocalDate
String str = "2017-11-21 14:41:06:612";    
LocalDate date = LocalDate.parse(str, fmt);
```