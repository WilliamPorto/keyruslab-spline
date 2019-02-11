3. Em Python devemos criar o __Spark Context__, chamar o método criado em Scala utilizando a JVM e passando o Spark Context __em formato de objeto Java__, receber o retorno do Spark Context no mesmo formato com o Spline habilitado e então __transformar o Spark Context em objeto Python__ para fazer o Job com a linhagem de dados do Spline habilitado:

```python
from pyspark.sql import SparkSession

spark_p = SparkSession.builder\
	              .appName('Spline')\
	              .config('spark.sql.warehouse.dir', '/user/hive/warehouse')\
	              .getOrCreate()

sc = spark_p.sparkContext

spark_j = sc._jvm.com.spline.Spline.sparkLineage(spark_p._jsparkSession)
spark = SparkSession(sc, spark_j)

# spark...
```
