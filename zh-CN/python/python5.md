---
name: Python综合案例2
---

## Python数据分析综合案例

---

### 1. 案例问题

---

某论坛网站的准备根据其日志数据统计和分析用户的访问行为，其日志数据保存在`access.log`文件中，总计有548160行，日志数据的基本格式如下图所示。

![](/docs/images/data1.png)

现需要根据日志数据进行统计分析，解决以下问题：

（1） 统计出网站当日的PV（Page View，总访问量），当日的注册用户数量，当日访问的独立IP数量等数据。

（2）找出当日访问该网站次数最多的十个IP，并按照访问次数降序排列。

（3）分时间段统计该网站访问量，并利用数据可视化技术画出折线图。


### 2. 问题分析与工具选择

---

上述问题是一个经典的大数据分析问题，首先日志文件是一个包含有50多万行的半结构化数据，其中每一行都包含有IP地址、访问时间、访问的URL、返回值状态码、返回页面字节数等信息。对于上述统计分析问题，直接分析日志文件通常无从下手，因此，我们需要借助专业的大数据分析方法和工具进行统计分析。

#### 2.1 分析方法

处理上述问题，通常需要经过以下几步：

1. 数据导入
2. 数据清洗
3. 数据统计
4. 数据分析
5. 数据可视化

#### 2.2 工具选择

本问题需要使用的工具包括：

- Java : 大数据基础语言环境
- Hadoop : HDFS文件系统
- MapReduce : 分布式计算模型 
- Hive ：数据仓库工具
- Sqoop ：数据迁移工具
- MySQL ：关系数据库
- Python ：大数据分析基础语言环境
- matplotlib : 数据可视化基础库
- seaborn : 数据可视化高级库


### 3. 数据导入

---

由于只处理单文件，因此可以直接在shell中使用hdfs命令进行数据导入。在shell中输入如下命令。

``` bash
hdfs dfs -mkdir -p /project/bbs/data
hdfs dfs -put access.log /project/bbs/data
```

上述命令，首先在hdfs中建立目录`/project/bbs/data`，该目录用来存放日志数据，然后利用put命令把`access.log`文件导入到hdfs当中。

可以通过命令`hdfs dfs -ls /project/bbs/data`确认hdfs中是否有对应的文件，其结果如下图所示。

![](/docs/images/data2.png)

### 4. 数据清洗

---

#### 4.1 需要清洗的数据

1. 根据统计分析的目标，我们发现最终要分析的结果都不包含返回值状态码、返回页面字节数等信息，因此我们需要将这部分数据过滤掉。
2. 对于时间数据，为了便于后续的统计分析，我们需要把文件中每一行的时间格式统一为"yyyyMMddHHmmss"这种格式。其中y表示年，M表示月，d表示日，H表示小时，m表示分钟，s表示秒。这种格式更加便于后续处理。
3. 用户每访问一个页面url，该页面可能会同时请求一些静态数据，如图片、字体文件、样式表文件等，在日志文件中也会增加对应的访问记录，但由于实际上用户只访问了一个页面，这些静态资源的访问并不能计入PV，因此需要将`"GET /staticsource/"`开头的访问记录过滤掉，又因为GET和POST字符串对统计分析也没有意义，因此也可以将其过滤掉；

#### 4.2 数据清洗程序

为了完成上述数据清洗，通过编写MapReduce程序完成这项任务，保存为`LogCleanJob.java`。其代码如下。

``` java
import java.net.URI;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

public class LogCleanJob extends Configured implements Tool {

    public static void main(String[] args) {
        Configuration conf = new Configuration();
        try {
            int res = ToolRunner.run(conf, new LogCleanJob(), args);
            System.exit(res);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @Override
    public int run(String[] args) throws Exception {
        final Job job = new Job(new Configuration(),
                LogCleanJob.class.getSimpleName());
        // 设置为可以打包运行
        job.setJarByClass(LogCleanJob.class);
        FileInputFormat.setInputPaths(job, args[0]);
        job.setMapperClass(MyMapper.class);
        job.setMapOutputKeyClass(LongWritable.class);
        job.setMapOutputValueClass(Text.class);
        job.setReducerClass(MyReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(NullWritable.class);
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        // 清理已存在的输出文件
        FileSystem fs = FileSystem.get(new URI(args[0]), getConf());
        Path outPath = new Path(args[1]);
        if (fs.exists(outPath)) {
            fs.delete(outPath, true);
        }
        
        boolean success = job.waitForCompletion(true);
        if(success){
            System.out.println("Clean process success!");
        }
        else{
            System.out.println("Clean process failed!");
        }
        return 0;
    }

    static class MyMapper extends
            Mapper<LongWritable, Text, LongWritable, Text> {
        LogParser logParser = new LogParser();
        Text outputValue = new Text();

        protected void map(
                LongWritable key,
                Text value,
                org.apache.hadoop.mapreduce.Mapper<LongWritable, Text, LongWritable, Text>.Context context)
                throws java.io.IOException, InterruptedException {
            final String[] parsed = logParser.parse(value.toString());

            // step1.过滤掉静态资源访问请求
            if (parsed[2].startsWith("GET /static/")
                    || parsed[2].startsWith("GET /uc_server")) {
                return;
            }
            // step2.过滤掉开头的指定字符串
            if (parsed[2].startsWith("GET /")) {
                parsed[2] = parsed[2].substring("GET /".length());
            } else if (parsed[2].startsWith("POST /")) {
                parsed[2] = parsed[2].substring("POST /".length());
            }
            // step3.过滤掉结尾的特定字符串
            if (parsed[2].endsWith(" HTTP/1.1")) {
                parsed[2] = parsed[2].substring(0, parsed[2].length()
                        - " HTTP/1.1".length());
            }
            // step4.只写入前三个记录类型项
            outputValue.set(parsed[0] + "\t" + parsed[1] + "\t" + parsed[2]);
            context.write(key, outputValue);
        }
    }

    static class MyReducer extends
            Reducer<LongWritable, Text, Text, NullWritable> {
        protected void reduce(
                LongWritable k2,
                java.lang.Iterable<Text> v2s,
                org.apache.hadoop.mapreduce.Reducer<LongWritable, Text, Text, NullWritable>.Context context)
                throws java.io.IOException, InterruptedException {
            for (Text v2 : v2s) {
                context.write(v2, NullWritable.get());
            }
        };
    }

    /*
     * 日志解析类
     */
    static class LogParser {
        public static final SimpleDateFormat FORMAT = new SimpleDateFormat(
                "d/MMM/yyyy:HH:mm:ss", Locale.ENGLISH);
        public static final SimpleDateFormat dateformat1 = new SimpleDateFormat(
                "yyyyMMddHHmmss");

        public static void main(String[] args) throws ParseException {
            final String S1 = "27.19.74.143 - - [30/May/2013:17:38:20 +0800] \"GET /static/image/common/faq.gif HTTP/1.1\" 200 1127";
            LogParser parser = new LogParser();
            final String[] array = parser.parse(S1);
            System.out.println("样例数据： " + S1);
            System.out.format(
                    "解析结果：  ip=%s, time=%s, url=%s, status=%s, traffic=%s",
                    array[0], array[1], array[2], array[3], array[4]);
        }

        /**
         * 解析英文时间字符串
         * 
         * @param string
         * @return
         * @throws ParseException
         */
        private Date parseDateFormat(String string) {
            Date parse = null;
            try {
                parse = FORMAT.parse(string);
            } catch (ParseException e) {
                e.printStackTrace();
            }
            return parse;
        }

        /**
         * 解析日志的行记录
         * 
         * @param line
         * @return 数组含有5个元素，分别是ip、时间、url、状态、流量
         */
        public String[] parse(String line) {
            String ip = parseIP(line);
            String time = parseTime(line);
            String url = parseURL(line);
            String status = parseStatus(line);
            String traffic = parseTraffic(line);

            return new String[] { ip, time, url, status, traffic };
        }

        private String parseTraffic(String line) {
            final String trim = line.substring(line.lastIndexOf("\"") + 1)
                    .trim();
            String traffic = trim.split(" ")[1];
            return traffic;
        }

        private String parseStatus(String line) {
            final String trim = line.substring(line.lastIndexOf("\"") + 1)
                    .trim();
            String status = trim.split(" ")[0];
            return status;
        }

        private String parseURL(String line) {
            final int first = line.indexOf("\"");
            final int last = line.lastIndexOf("\"");
            String url = line.substring(first + 1, last);
            return url;
        }

        private String parseTime(String line) {
            final int first = line.indexOf("[");
            final int last = line.indexOf("+0800]");
            String time = line.substring(first + 1, last).trim();
            Date date = parseDateFormat(time);
            return dateformat1.format(date);
        }

        private String parseIP(String line) {
            String ip = line.split("- -")[0].trim();
            return ip;
        }
    }
}
```

上述Java程序可分为`LogParser`类和`MapReduce`代码。其中利用`LogParser`类将日志文件的每行划分成5个部分，分别是`ip, time, url, status, traffic`，然后利用`MapReduce`再对这5部分进行处理。对于url过滤掉静态资源请求、去除前缀和后缀，对于time进行时间格式的转换，最终返回处理后的`ip, time, url`三项数据，写入到参数指定的hdfs路径下。

#### 4.3 执行数据清洗任务

首先编译并打包Java程序。

``` bash
mkdir cleaner
javac LogCleanJob.java -d cleaner/
cd cleaner
jar -cvf cleaner.jar ./*.class
```

经过上述编译和打包，最终得到`mycleaner.jar`包，如下图所示。

![](/docs/images/data3.png)

在得到jar包以后就可以执行数据清洗任务了，命令如下。

``` bash
hadoop jar /usr/local/files/cleaner/cleaner.jar LogCleanJob /project/bbs/data/access.log /project/bbs/cleaned/accesslog
```

该命令指定了运行jar包的入口类，以及hdfs的输入文件路径和输出文件路径，通过该命令执行清洗任务，并将新的数据保存在hdfs的`/project/bbs/cleaned/accesslog/`路径下。

#### 4.4 查看cleaned数据

通过下列命令查看清洗之后的数据。

``` bash
hdfs dfs -ls /project/bbs/cleaned/accesslog
hdfs dfs -cat /project/bbs/cleaned/accesslog/part-r-00000 | head -20
```

清洗之后的数据如下图所示。可以看到该文件每行包含ip地址、访问时间、访问url三部分，与数据清洗之前相比，结构化程度明显提高，我们可以很方便地利用此文件进行数据分析。

![](/docs/images/data4.png)

此外，也可以通过hadoop的web接口：http://localhost:50070/explorer.html#/ 下载和查看清洗后的数据。

### 5. 数据分析

---

在数据预处理完成后，可以利用hive建立数据仓库，进行数据分析。

#### 5.1 建立bbs表

首先输入hive进行hive环境，然后执行下列hql语句，通过文件'/project/bbs/cleaned/accesslog'建立bbs表，bbs包含了accesslog文件的所有信息，其中字段ip表示ip地址、字段atime表示访问时间、字段url表示访问的url地址。

``` sql
CREATE EXTERNAL TABLE bbs(ip string, atime string, url string) ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t' LOCATION '/project/bbs/cleaned/accesslog';
```

数据仓库建立以后，可通过select语句查询数据。

``` sql
select * from bbs limit 10
```

查询结果如图。

![](/docs/images/data5.png)


#### 5.2 统计PV

对上述bbs表的ip列进行查询即可，为了便于以后使用，这里将PV值保存到表bbs_pv中。

``` sql
CREATE TABLE bbs_pv AS SELECT COUNT(ip) AS PV FROM bbs;
```

对表`bbs_pv`执行查询即可获取pv值。

``` sql
select * from bbs_pv
```

查询结果如下图，可以看到当日网站的总访问量为169857.

![](/docs/images/data6.png)

#### 5.3 统计注册用户数量

该论坛的用户注册页面为`member.php`，而当用户点击注册时请求的又是`member.php?mod=register`的url。因此，这里我们只需要统计出日志中访问的URL是member.php?mod=register的即可，HQL代码如下。

``` sql
CREATE TABLE bbs_reguser AS SELECT COUNT(ip) AS REGUSER FROM bbs WHERE INSTR(url,'member.php?mod=register')>0;
```

对表`bbs_reguser`执行查询即可获取注册用户数量。sql语句和上述bbs_pv表查询类似，其结果为28，即该论坛当日的注册用户数为28人。

#### 5.4 统计当日访问的独立IP数量

同一IP无论访问了几个页面，独立IP数均为1。因此，这里我们只需要统计日志中处理的独立IP数即可，在SQL中我们可以通过DISTINCT关键字，在HQL中也是通过这个关键字, HQL代码如下。

``` sql
CREATE TABLE bbs_ip AS SELECT COUNT(DISTINCT ip) AS IP FROM bbs;
```

对表`bbs_ip`执行查询即可获取独立IP数量。sql语句和上述bbs_pv表查询类似，其结果为10411，即该论坛当日的访问的独立IP数量为10411.

#### 5.5 合并上述统计指标

在hive中，可以将上述三个统计指标进行合并，将其放在一张表当中，以便于汇总每日网站访问数据。借助一张汇总表将刚刚统计到的结果整合起来，通过表连接结合，HQL代码如下。

``` sql
CREATE TABLE bbs_stat AS SELECT a.pv, b.reguser, c.ip FROM bbs_pv a JOIN bbs_reguser b ON 1=1 JOIN bbs_ip c ON 1=1;
```

### 6. 数据导出

虽然利用hive已经可以进行数据分析，但仍有很多业务需求需要用到更为规范的关系数据库处理。此时我们可以借助sqoop工具，将hive数据仓库中的数据导出到MySQL当中。

#### 6.1 建立mysql用户

在mysql中新建一个用户并赋权限，sql语句如下。

``` sql
grant all on *.* to hive@localhost identified by 'hive'; 
flush privileges;
```

#### 6.2 建立数据库和表

新建一个数据库bbs，并建表`bbs_logs_stat`和`bbs_logs`，`bbs_logs_stat`用来保存bbs日志的统计数据， `bbs_logs`用来保存bbs日志的原始数据。

``` sql
create database bbs;
use bbs;

CREATE TABLE `bbs_logs_stat` (
  `pv` int(11) DEFAULT NULL,
  `reguser` int(11) DEFAULT NULL,
  `ip` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `bbs_logs` (
  `ip` varchar(20) DEFAULT NULL,
  `logtime` varchar(20) DEFAULT NULL,
  `logurl` text
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

#### 6.3 从hive仓库导出数据

利用sqoop分别将hive仓库的bbs数据导出到mysql中，对于语句如下。

``` bash
# 导出bbs_logs_stat表
sqoop export --connect jdbc:mysql://127.0.0.1:3306/bbs?useSSL=false --username hive -P --table bbs_logs_stat --fields-terminated-by '\001' --export-dir '/user/hive/warehouse/bbs_stat'

# 导出bbs_logs表
sqoop export --connect "jdbc:mysql://127.0.0.1:3306/bbs?useSSL=false&characterEncoding=utf-8" --username hive -P --table bbs_logs --fields-terminated-by '\t' --export-dir '/project/bbs/cleaned/accesslog'
```

#### 6.4 从mysql中查看数据

在mysql中编写sql语句查看数据是否导入成功。

``` sql
select * from bbs_logs_stat;

select * from bbs_logs limit 10;

select count(*) from bbs_logs;
```

如果导入成功，结果如下图所示。

![](/docs/images/data7.png)

### 7. 数据可视化

#### 7.1 环境准备

利用python连接mysql，通过python的可视化工具matplotlib和seaborn可以对前面导出的数据进行可视化分析。

由于需要连接数据库并进行数据可视化，因此需要先安装对应的python第三方工具包，命令如下。

``` bash
pip3 install pymysql
pip3 install numpy
pip3 install pandas
# pip3 install matplotlib(该命令在ubuntu下可能会失败，请用下面的命令代替)
sudo apt-get install python3-matplotlib
# pip3 install seaborn(该命令在ubuntu下可能会失败，请用下面的命令代替)
sudo apt-get install python3-seaborn
```

#### 7.2 绘制条形图

找出当日访问该网站次数最多的十个IP，并按照访问次数降序排列。并利用python可视化工具绘制条形图。其代码如下。

``` python
import pymysql
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# 打开数据库连接
db = pymysql.connect("127.0.0.1", "hive", "hive", "bbs")

sql = "select ip, count(*) as ip_count from bbs_logs group by ip \
        order by ip_count desc limit 10"


def query(db, sql):
    # 使用cursor()方法获取操作游标
    cursor = db.cursor()
    try:
        # 执行SQL语句
        cursor.execute(sql)
        # 获取所有记录列表
        results = cursor.fetchall()
    except Exception as e:
        raise e
    finally:
        return results


ips = []
ip_count = []
try:
    results = query(db, sql)
    for row in results:
        ips.append(row[0])
        ip_count.append(row[1])
        # 打印结果
except Exception as e:
    print(e)
db.close()

plt.figure(figsize=(15, 5), dpi=100)
X = np.array(ips)
y = np.array(ip_count)
df = pd.DataFrame({"ip": X, "count": y})
sns.barplot("ip", "count", data=df)
plt.show()
```

上述代码的基本过程是：

1. 连接mysql数据库
2. 编写sql语句，按照ip将表bbs_logs进行分组，然后对每组ip进行计数并倒序返回。这样就得到了访问网站次数前10的IP地址以及对应IP访问网站的次数。
3. 将ip地址保存到列表ips中，将ip访问网站的次数保存在ip_count中。
4. 以ips为x轴，以ip_count为y轴利用seaborn的barplot方法绘制条形图。

执行上述代码，得到的条形图如下图所示。

![](/docs/images/data8.png)


#### 7.3 绘制折线图

分时间段统计该网站访问量，并利用python可视化工具绘制折线图。其代码如下。

``` python
import pymysql
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# 打开数据库连接
db = pymysql.connect("127.0.0.1", "hive", "hive", "bbs")

sql = "select substring(logtime, 9, 2) as timespan, count(*)  as visit_count \
        from bbs_logs group by timespan order by timespan limit 10"


def query(db, sql):
    # 使用cursor()方法获取操作游标
    cursor = db.cursor()
    try:
        # 执行SQL语句
        cursor.execute(sql)
        # 获取所有记录列表
        results = cursor.fetchall()
    except Exception as e:
        raise e
    finally:
        return results


timespan = []
totalvisit = []
try:
    results = query(db, sql)
    for row in results:
        timespan.append(int(row[0]))
        totalvisit.append(int(row[1]))
        # 打印结果
except Exception as e:
    print(e)
db.close()

plt.figure(figsize=(10, 5), dpi=100)
X = np.array(timespan)
y = np.array(totalvisit)
df = pd.DataFrame({"timespan": X, "total_visit": y})
g = sns.pointplot("timespan", "total_visit", data=df)

plt.show()
```

上述代码与绘制条形图的过程基本类似：

1. 连接mysql数据库
2. 编写sql语句，利用mysql函数`substring(logtime, 9, 2)`取出logtime字段中表示小时的字串，然后根据小时对数据库表进行分组，并统计每组的访问量，最后按照小时正序排列。
3. 将时段信息保存到列表timespan中，将访问网站的次数保存在total_visit中。
4. 以timespan为x轴，以total_visit为y轴利用seaborn的pointplot方法绘制折线图。

执行上述代码，得到的折线图如下图所示。

![](/docs/images/data9.png)


至此，我们的网站日志分析基本完成。综合使用hadoop、MapReduce、hive、sqoop、mysql、python等工具，我们完成了对一个较大规模数据集的数据预处理、数据统计、数据分析以及数据可视化等一系列的数据处理工作。读者也可以尝试利用上述技术，结合实际的应用场景，解决特定的大数据分析与处理问题。