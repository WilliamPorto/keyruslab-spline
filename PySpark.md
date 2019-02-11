# Integrate SPLINE com PySpark


1. Criar um projeto em Scala que importa o __SparkSession__ e o __SparkLineageInitializer__. Nesse projeto deve-se criar um método que recebe um __SparkSession__, habilita a linhagem do __SPLINE__ e retorna um __SparkSession__, tudo isso como objeto Java:
```scala
package com.spline

import org.apache.spark.sql.SparkSession
import za.co.absa.spline.core.SparkLineageInitializer._

object Spline {

    def sparkLineage(spark: SparkSession): SparkSession = {

       spark.enableLineageTracking()
       spark

    }

}
```
2. [Gere um JAR completo](https://github.com/WilliamPorto/keyruslab-spline/blob/master/fatJAR.md "Gere um JAR completo") do código a cima com todas as dependências do SPLINE utilizando o SBT.
3. Em Python devemos criar o __Spark Context__, chamar o método criado em Scala utilizando a JVM e passando o Spark Context __em formato de objeto Java__, receber o retorno do Spark Context no mesmo formato com o SPLINE habilitado e então __transformar o Spark Context em objeto Python__ para fazer o Job com a linhagem de dados do SPLINE habilitado:

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
4. Execute o seu Script em Python com ```spark-submit``` __enviando junto o JAR completo__ criado, ou apenas __copie seu JAR dentro da pasta JARs do Spark__ e execute seu Script com o PySpark Shell ou até com o ```spark-submit``` sem a necessidade de enviar seu JAR completo, para ambos, vide sessão específica.
