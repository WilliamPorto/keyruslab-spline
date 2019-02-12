# Integração do Spline com PySpark

Toda a aplicação do SPLINE gira em torno do método `spark.enableLineageTracking()`, ele habilita a LineageTracking em nosso SparkSession e a partir dai todas as operações com os dados estão sendo persistidas em algum sistema (MongoDB, HDFS, Atlas).

O problema é que o SPLINE foi desenvolvido para Scala e por isso não é possível chamar o método em um objeto org.apache.<strong>pyspark</strong>.sql.SparkSession.

A solução é criar um objeto PysparkHandler em Scala para receber o SparkSession do PySpark, e retornar uma SparkSession com o SPLINE habilitado:
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
- [Gerar](https://github.com/WilliamPorto/keyruslab-spline/blob/master/FatJAR.md "Gerar") um FatJAR do código Scala a cima  
- [Carregar](https://github.com/WilliamPorto/keyruslab-spline/blob/master/Persist%C3%AAncia.md) o FatJAR em nossa aplicação ( pyspark --jars myFileJar.jar )
- Acessar o FatJAR e seus objetos em nossa aplicação:

```python
from pyspark.sql import SparkSession

spark_p = SparkSession.builder\
	              .appName('Spline')\
	              .config('spark.sql.warehouse.dir', '/user/hive/warehouse')\
	              .getOrCreate()

sc = spark_p.sparkContext

#GET JVM FROM OUR SPARKCONTEXT
jvm=sc._jvm
#FROM org.apache.pyspark.sql.SparkSession GET A JavaSparkSession
jsparkSession = spark._jsparkSession
#GET org.apache.spark.sql.SparkSession FROM Scala Jar
ssl=jvm.mySplinePackage.PysparkHandler.setSparkListener(jsparkSession)
#GET org.apache.pyspark.sql.SparkSession
#BY CALLING SparkSession(org.apache.pyspark.sql.SparkContext,org.apache.spark.sql.SparkSession)
sparkEnabledLineage=SparkSesion(sc,ssl)

#AGORA TEMOS UM SparkSession COM O LineageTracking ENABLED
```
