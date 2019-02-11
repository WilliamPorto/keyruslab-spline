# SPLINE percistence 

Inicialmente o SPLINE tinha três alternativas para fazer o registro desse data lineage, MONGOdb, HDFS e ATLAS.


## MONGOdb:

#### Executar script PySpark com Spline persistindo no MongoDB
```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.mongo.MongoPersistenceFactory -Dspline.mongodb.url=mongodb://caminho_do_banco:porta_do_banco -Dspline.mongodb.name=nome_do_banco" --jars nome_do_jar_completo.jar script_pyspark.py
```
ou

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.mongo.MongoPersistenceFactory -Dspline.mongodb.url=mongodb://caminho_do_banco:porta_do_banco -Dspline.mongodb.name=nome_do_banco" --jars nome_do_jar_completo.jar script_pyspark.py
```

> Utilizado: -Dspline.mongodb.url=mongodb://127.0.0.1:27017 -Dspline.mongodb.name=db_spline

## HDFS:

#### Executar script PySpark com Spline persistindo no HDFS:

```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory" --jars nome_do_jar_completo.jar script_pyspark.py
```

por exemplo:

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory" --jars nome_do_jar_completo.jar script_pyspark.py
```

#### Executar script PySpark com Spline persistindo no MongoDB e HDFS:

```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.api.composition.ParallelCompositeFactory -Dspline.mongodb.url=mongodb://caminho_do_banco:porta_do_banco -Dspline.mongodb.name=nome_do_banco -Dspline.persistence.composition.factories=za.co.absa.spline.persistence.mongo.MongoPersistenceFactory,za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory" --jars nome_do_jar_completo.jar script_pyspark.py
```

ou

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.api.composition.ParallelCompositeFactory -Dspline.mongodb.url=mongodb://caminho_do_banco:porta_do_banco -Dspline.mongodb.name=nome_do_banco -Dspline.persistence.composition.factories=za.co.absa.spline.persistence.mongo.MongoPersistenceFactory,za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory" --jars nome_do_jar_completo.jar script_pyspark.py
```

#### Executar script PySpark com Spline persistindo no Atlas e HDFS:

```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.api.composition.ParallelCompositeFactory -Dspline.persistence.composition.factories=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory,za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" --jars nome_do_jar_completo.jar script_pyspark.py
```

ou

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.api.composition.ParallelCompositeFactory -Dspline.persistence.composition.factories=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory,za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" --jars nome_do_jar_completo.jar script_pyspark.py
```
<br>
<br>
## ATLAS:

O ATLAS foi descontínuado desde a versão spline0.3.2, a integração estava com algumas falhas e a documentação que a equipe do SPLINE disponibilizava era insuficiente. A previsão é que uma nova API resolva esses problemas na versão 0.4, enquanto isso... Descrevemos nesse documento uma forma de habilitar o ATLAS, fazendo apenas algumas alterações nos arquivos do SPLINE.
#### Executar script PySpark com Spline persistindo no Atlas:

```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" --jars nome_do_jar_completo.jar script_pyspark.py
```

ou

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" --jars nome_do_jar_completo.jar script_pyspark.py
```

> Utilizado: -Datlas.kafka.bootstrap.servers=sandbox-hdp.hortonworks.com:6667


-------------------------

Como explicado na sessão anterior, não é preciso enviar o JAR completo na hora da execução com ```spark-submit```, aliás, quando é executado o Spline persistindo no Atlas é aconcelhável que não seja realmente enviado, pois o JAR fica muito grande e pode causar um certo delay de execução, ocasionando possíveis erros, portanto, na próxima sessão será apresentado uma forma de __executar o Spline sem a necessidade de enviar o JAR completo ou algum JAR__.
<br>
-------------------------
<br>
