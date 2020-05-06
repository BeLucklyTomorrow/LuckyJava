## Java – Date and Time APIs

Java主要使用两个包支持日期和时间特性`java.time`和`java.util`。

在java8中新加了`java.time`，新加的类旨在解决`java.util.Date`and `java.util.Calendar` 类遗留的缺点。

### 1. Legacy Date Time API

#### 1.1 Classes

java8之前主要的类：

- System.currentTimeMillis()：表示自1970年1月1日起的当前日期和时间，以毫秒为单位.
- java.util.Date：表示时间中的特定瞬间，精度为毫秒.
- java.util.Calendar：一个抽象类，它提供了在实例之间进行转换和以不同方式操作日历字段的方法.
- java.text.SimpleDateFormat：一个具体的类，用于以对语言环境敏感的方式格式化和解析日期，以及任何预定义的和任何用户定义的模式.
- java.util.TimeZone：表示时区偏移量，并计算出夏令时.

#### 1.2 Challanges

虽然这些api非常适合简单的用例，但是Java社区仍然不断地抱怨以有效的方式使用这些类的问题。因此，许多其他第三方库(例如Joda-Time或Apache Commons中的类)更受欢迎。

- A `Date` class 应该表示 a date, 但它也代表一个具有小时、分钟和秒的实例.
- But `Date` 没有任何关联的时区. 它自动拾取默认时区。您不能代表某个时区的日期.
- Classes 是可变的. 因此，开发人员在传递函数之前克隆日期会增加额外的负担，这可能会使其发生突变.
- 日期格式化类也不是线程安全的. 如果没有额外的同步，格式化程序实例就不能使用，否则代码可能会中断.
- For some reason, there is another class `java.sql.Date` which has timzone information.
- 用其他时区创建日期非常棘手，通常会导致不正确的结果.
- Its classes use a zero-index for months, which is a cause of many bugs in applications over the years

### 2. New Date Time API in Java 8

新的日期API试图解决遗留类的上述问题。主要包括以下几类：

- java.time.LocalDate：表示ISO日历中的年月日，并用于表示没有时间的日期。它可以用来表示仅日期信息，例如出生日期或结婚日期。
- java.time.LocalTime：只处理时间。它可用于表示人的时间，如电影时间，或本地图书馆的开合时间。
- java.time.LocalDateTime：处理日期和时间，没有时区。它是一种组合。*本地日期*与*当地时间*.
- java.time.ZonedDateTime：结合*本地日期时间*类中的区域信息*佐尼*类它代表一个完整的日期时间戳和时区信息。
- java.time.OffsetTime：处理时间与来自格林尼治/UTC的相应时区偏移，没有时区ID。
- java.time.OffsetDateTime：处理日期和时间与格林尼治/UTC对应的时区偏移，没有时区ID。
- java.time.Clock：在任何给定时区提供对当前时刻、日期和时间的访问。虽然时钟类的使用是可选的，但这个特性允许我们测试您的代码用于其他时区，或者使用固定时钟，时间不改变。
- java.time.Instant：表示时间线上的纳秒的开始（因为EPOCH），并用于生成表示机器时间的时间戳。在历元之前发生的瞬间具有负值，并且在历元之后发生的时刻具有正值。
- java.time.Duration：在两个瞬间之间的差异，以秒或纳秒为单位测量，并且不使用基于日期的结构，如年、月和日，尽管该类提供了转换为天、小时和分钟的方法。
- java.time.Period：定义基于日期的值（年、月、日）之间的日期差异。
- java.time.ZoneId：指定时区标识符并提供用于转换之间的规则*瞬间*和A*本地日期时间*.
- java.time.ZoneOffset：指定格林尼治/UTC时间段的偏移量。
- java.time.format.DateTimeFormatter：提供许多预定义的格式化程序，或者我们可以定义我们自己的格式。它提供*PARSE（）*或*格式（）*方法来解析和格式化日期时间值。

### 3. Common Usecases

#### 3.1. Get Current Date and Time

所有的日期-时间类都有一个factory方法now()，这是在Java 8中获取当前日期和时间的首选方法.

```java
LocalTime currentTime = LocalTime.now();                //13:33:43.557
 
LocalDate currentDate = LocalDate.now();                //2020-05-03
 
LocalDateTime currentDateTime = LocalDateTime.now();    //2020-05-03T13:33:43.557
```

#### 3.2. Parse Date and Time

日期解析是在Date time类中的DateTimeFormatter类和parse()方法的帮助下完成的.

```java
String dateString = "2020-04-08 12:30";
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
LocalDateTime parsedDateTime = LocalDateTime.parse(dateString, formatter);
 
System.out.println(parsedDateTime);     //2020-04-08T12:30
```

#### 3.3. Format Date and Time

日期格式化是在Date time类中的DateTimeFormatter类和format()方法的帮助下完成的.

```java
//Format a date
LocalDateTime myDateObj = LocalDateTime.now();
DateTimeFormatter myFormatObj = DateTimeFormatter.ofPattern("dd-MM-yyyy HH:mm");
 
String formattedDate = myDateObj.format(myFormatObj);
System.out.println(formattedDate);  //  03-05-2020 13:46
```

