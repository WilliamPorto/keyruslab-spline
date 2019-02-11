# Persistências do SPLINE 

Inicialmente o SPLINE tinha três alternativas para fazer o registro da linhagem de dados, MONGODB, HDFS e ATLAS.


## MONGODB:

#### Executar script PySpark com SPLINE persistindo no MongoDB
```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.mongo.MongoPersistenceFactory -Dspline.mongodb.url=mongodb://caminho_do_banco:porta_do_banco -Dspline.mongodb.name=nome_do_banco" --jars nome_do_jar_completo.jar script_pyspark.py
```
ou

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.mongo.MongoPersistenceFactory -Dspline.mongodb.url=mongodb://caminho_do_banco:porta_do_banco -Dspline.mongodb.name=nome_do_banco" --jars nome_do_jar_completo.jar script_pyspark.py
```

> Utilizado: -Dspline.mongodb.url=mongodb://127.0.0.1:27017 -Dspline.mongodb.name=db_spline

## HDFS:

#### Executar script PySpark com SPLINE persistindo no HDFS:

```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory" --jars nome_do_jar_completo.jar script_pyspark.py
```

por exemplo:

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory" --jars nome_do_jar_completo.jar script_pyspark.py
```

## PERSISTÊNCIA PARALELA:

#### Executar script PySpark com SPLINE persistindo no MongoDB e HDFS:

```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.api.composition.ParallelCompositeFactory -Dspline.mongodb.url=mongodb://caminho_do_banco:porta_do_banco -Dspline.mongodb.name=nome_do_banco -Dspline.persistence.composition.factories=za.co.absa.spline.persistence.mongo.MongoPersistenceFactory,za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory" --jars nome_do_jar_completo.jar script_pyspark.py
```

ou

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.api.composition.ParallelCompositeFactory -Dspline.mongodb.url=mongodb://caminho_do_banco:porta_do_banco -Dspline.mongodb.name=nome_do_banco -Dspline.persistence.composition.factories=za.co.absa.spline.persistence.mongo.MongoPersistenceFactory,za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory" --jars nome_do_jar_completo.jar script_pyspark.py
```

#### Executar script PySpark com SPLINE persistindo no Atlas e HDFS:

```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.api.composition.ParallelCompositeFactory -Dspline.persistence.composition.factories=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory,za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" --jars nome_do_jar_completo.jar script_pyspark.py
```

ou

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.api.composition.ParallelCompositeFactory -Dspline.persistence.composition.factories=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory,za.co.absa.spline.persistence.hdfs.HdfsPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" --jars nome_do_jar_completo.jar script_pyspark.py
```

## ATLAS:

A persistência em Atlas tem dois pequenos diferenciais dos demais.

1. Os desenvolvedores do Spline pararam de oferecer suporte ao Atlas na versão 0.3.1, portanto, caso você queira utilizar o Spline persistindo no Atlas é necessário que você utilize as dependências 0.3.1. A previsão é que uma nova API resolva esses problemas na versão 0.4, enquanto isso, é preciso seguir esse tutorial.

2. Para utilizar o Atlas é preciso colocar um arquivo de modelo de persistência dentro da pasta models do Atlas, é um arquivo JSON que mostra ao Atlas como o Spline enviará as informações para ele persistir em sua linhagem.

A boa notícia é que você pode fazer o download do [Atlas Metal Model](https://search.maven.org/remotecontent?filepath=za/co/absa/spline/spline-persistence-atlas/0.3.1/spline-persistence-atlas-0.3.1-atlas-meta-model.json "Baixar Agora"), caso queira procurar por conta própria, apenas entre no [Search Maven](https://search.maven.org/ "Search Maven") e busque a dependência do __spline-persistence-atlas-0.3.1__, lá terá o modelo para ser baixado.

Veja se serve para você, mas no meu caso, foi copiado o arquivo de modelo para dentro da pasta de instalação do Atlas assim:

```
cp /root/Model/spline-meta-model.json /usr/hdp/current/atlas-client/models/spline-meta-model.json
```

__Um passo muito importante que não deve ser esquecido, após copiar esse arquivo de modelo, reinicie o Atlas, sério, isso não funcionará se não for reiniciado, então sim, você poderá utilizar o Spline persistindo no Atlas.__

Importante dizer que devemos respeitar as versões que funcionaram corretamente, segue:

- Spark 2.3.0.2.6.5.0
- Apache Atlas 0.8.0.2.6.5.0
- Spline 0.3.1
- Kafka 1.0.0.2.6.5.0

#### Executar script PySpark com SPLINE persistindo no Atlas utilizando o JAR Completo:

```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" --jars nome_do_jar_completo.jar script_pyspark.py
```

ou

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" --jars nome_do_jar_completo.jar script_pyspark.py
```

#### Executar script PySpark com SPLINE persistindo no Atlas sem o JAR Completo:

```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" script_pyspark.py
```

ou

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" script_pyspark.py
```

> Utilizado: -Datlas.kafka.bootstrap.servers=sandbox-hdp.hortonworks.com:6667


-------------------------

Como explicado na sessão anterior, não é preciso enviar o JAR completo na hora da execução com ```spark-submit```, aliás, quando é executado o SPLINE persistindo no Atlas é aconcelhável que não seja realmente enviado, pois o JAR fica muito grande e pode causar um certo delay de execução, ocasionando possíveis erros, portanto, na próxima sessão será apresentado uma forma de __executar o SPLINE sem a necessidade de enviar o JAR completo ou algum JAR__.
<br>
-------------------------
<br>
