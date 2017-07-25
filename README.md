# zeppelin-highcharts-heatmap

Heatmap Charts for Apache Zeppelin using highcharts.js

## Compatibility

| Chart Version | Zeppelin Version |
| :---: | :---: |
| ALL | 0.7.0+ |

## Install

```shell
# $ZEPPELIN_HOME=/opt/zeppelin
cd $ZEPPELIN_HOME/local_repo/
git clone https://github.com/lonly197/zeppelin-highcharts-heatmap.git
cd zeppelin-highcharts-heatmap
npm install
cp zeppelin-highcharts-heatmap.json $ZEPPELIN_HOME/helium/
```

Place zeppelin-highcharts-heatmap.json in local registry (default location is ZEPPELIN_HOME/helium.)
And enable visualization from Helium menu.

## Usage

- **xAxis**: `categorical`
- **yAxis**: `categorical`
- **colorAxis**: `number`

## Screenshots 

![](https://raw.githubusercontent.com/lonly197/zeppelin-highcharts-heatmap/master/screenshots/heatmap-usage.gif)

## Example Paragraph for Testing

```scala
import org.apache.commons.io.IOUtils
import java.net.URL
import java.nio.charset.Charset

// Zeppelin creates and injects sc (SparkContext) and sqlContext (HiveContext or SqlContext)
// So you don't need create them manually

// load bank data
val bankText = sc.parallelize(
    IOUtils.toString(
        new URL("https://s3.amazonaws.com/apache-zeppelin/tutorial/bank/bank.csv"),
        Charset.forName("utf8")).split("\n"))

case class Bank(age: Integer, job: String, marital: String, education: String, balance: Integer)

val bank = bankText.map(s => s.split(";")).filter(s => s(0) != "\"age\"").map(
    s => Bank(s(0).toInt, 
            s(1).replaceAll("\"", ""),
            s(2).replaceAll("\"", ""),
            s(3).replaceAll("\"", ""),
            s(5).replaceAll("\"", "").toInt
        )
).toDF()
bank.registerTempTable("bank")
```

```
%sql 
select age, job, marital, education, avg(balance) as average
from bank
group by age, job, marital, education
limit 100
```

## License

See [https://shop.highsoft.com/faq](https://shop.highsoft.com/faq)
