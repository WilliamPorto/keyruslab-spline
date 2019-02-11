# Mapa

__Atlas Meta Model__: Modelo JSON do Atlas para persistência de linhagem

__Dependências do Spline 0.3.1__: Dependências do Spline baixadas manualmente, aqui consta inclusive as diversas dependências do Spark Adapter, ou seja, para o Spark 2.2, 2.3 e 2.4, a diferença é que o Spark 2.4 apenas tem na versão do Spline 0.3.6.

__Habilitação do Spline com Scala__: Código produzido pela Keyrus para conexão do PySpark com o Spline utilizando o Scala, utilize a chamada por meio da JVM do mesmo jeito que foi utilizado abaixo, pois o objeto, método e demais tem um nome específico.

<br>
* Em Python devemos criar o __Spark Context__, chamar o método criado em Scala utilizando a JVM e passando o Spark Context __em formato de objeto Java__, receber o retorno do Spark Context no mesmo formato com o Spline habilitado e então __transformar o Spark Context em objeto Python__ para fazer o Job com a linhagem de dados do Spline habilitado:

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