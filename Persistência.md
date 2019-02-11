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

O ATLAS foi descontínuado desde a versão SPLINE0.3.2, a integração estava com algumas falhas e a documentação que a equipe do SPLINE disponibilizava era insuficiente. A previsão é que uma nova API resolva esses problemas na versão 0.4, enquanto isso... Descrevemos nesse documento uma forma de habilitar o ATLAS, fazendo apenas algumas alterações nos arquivos do SPLINE.

Há tendência é que eles voltem a oferecer suporte, mas por enquanto devemos respeitar as versões que funcionaram corretamente, segue:

- Spark 2.3.0.2.6.5.0
- Apache Atlas 0.8.0.2.6.5.0
- Spline 0.3.1
- Kafka 1.0.0.2.6.5.0

Primeiro, você deve alterar o arquivo atlas-application-.properties para estabelecer a conexão do Spline com o Atlas kafka topic, da mesma maneira que você configuraria qualquer outra conxão do Atlas.

Depois, copie o arquivo spline-meta-model.json para a pasta models do Atlas, é um arquivo JSON que mostra ao Atlas como o Spline enviará as informações para ele persistir em sua linhagem. (na Hortonworks o caminho é : /usr/hdp/current/atlas/models)


#### Executar script PySpark com SPLINE persistindo no Atlas:

```
spark-submit --conf "spark.driver.extraJavaOptions=-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" --jars nome_do_jar_completo.jar script_pyspark.py
```

ou

```
spark-submit --driver-java-options "-Dspline.mode=BEST_EFFORT -Dspline.persistence.factory=za.co.absa.spline.persistence.atlas.AtlasPersistenceFactory -Datlas.kafka.bootstrap.servers=caminho_do_bootstrap_server:porta_do_bootstrap_server" --jars nome_do_jar_completo.jar script_pyspark.py
```

> Utilizado: -Datlas.kafka.bootstrap.servers=sandbox-hdp.hortonworks.com:6667


-------------------------

Como explicado na sessão anterior, não é preciso enviar o JAR completo na hora da execução com ```spark-submit```, aliás, quando é executado o SPLINE persistindo no Atlas é aconcelhável que não seja realmente enviado, pois o JAR fica muito grande e pode causar um certo delay de execução, ocasionando possíveis erros, portanto, na próxima sessão será apresentado uma forma de __executar o SPLINE sem a necessidade de enviar o JAR completo ou algum JAR__.
<br>
-------------------------
<br>


Provavelmente você teria 3 perguntas básicas, pois eu teria:

1. Se algum link não funcionar, como eu poderei buscar por conta própria as dependências? Resposta: Acesse o [Search Maven](https://search.maven.org/ "Search Maven") e busque qualquer dependência, lembrando que as do Spline são específicas, então, não fugirá do que está a cima, ao encontrar a dependência o site disponibilizará várias formas de download, incluindo o JAR.

2. Porque todas as dependências do Spline foram utilizadas na versão 0.3.1? Resposta: Isso será explicado na sessão sobre o Atlas.

3. Aonde eu coloco todos esses JARs baixados? Resposta: Talvez a pergunta mais importante, é claro, precisamos colocar em algum local, esse local deve ser o core do Spark, é necessário que você encontre a pasta de JARs que fica dentro da instalação do seu Spark e colar lá junto com os vários outros JARs que já estão lá, abaixo será mostrado o meu caso, talvez sirva para você:

```
cp /root/JARs/spline-commons-0.3.1.jar /usr/hdp/current/spark2-client/jars/spline-commons-0.3.1.jar
cp /root/JARs/spline-core-0.3.1.jar /usr/hdp/current/spark2-client/jars/spline-core-0.3.1.jar
cp /root/JARs/spline-core-spark-adapter-api-0.3.1.jar /usr/hdp/current/spark2-client/jars/spline-core-spark-adapter-api-0.3.1.jar
cp /root/JARs/spline-core-spark-adapter-2.3-0.3.1.jar /usr/hdp/current/spark2-client/jars/spline-core-spark-adapter-2.3-0.3.1.jar
cp /root/JARs/spline-model-0.3.1.jar /usr/hdp/current/spark2-client/jars/spline-model-0.3.1.jar
cp /root/JARs/spline-persistence-api-0.3.1.jar /usr/hdp/current/spark2-client/jars/spline-persistence-api-0.3.1.jar
cp /root/JARs/spline-persistence-hdfs-0.3.1.jar /usr/hdp/current/spark2-client/jars/spline-persistence-hdfs-0.3.1.jar
cp /root/JARs/spline-persistence-mongo-0.3.1.jar /usr/hdp/current/spark2-client/jars/spline-persistence-mongo-0.3.1.jar
cp /root/JARs/spline-persistence-atlas-0.3.1.jar /usr/hdp/current/spark2-client/jars/spline-persistence-atlas-0.3.1.jar
cp /root/JARs/atlas-notification-0.8.2.jar /usr/hdp/current/spark2-client/jars/atlas-notification-0.8.2.jar
cp /root/JARs/atlas-typesystem-0.8.2.jar /usr/hdp/current/spark2-client/jars/atlas-typesystem-0.8.2.jar
cp /root/JARs/atlas-common-0.8.2.jar /usr/hdp/current/spark2-client/jars/atlas-common-0.8.2.jar
cp /root/JARs/atlas-intg-0.8.2.jar /usr/hdp/current/spark2-client/jars/atlas-intg-0.8.2.jar
cp /root/JARs/json4s-native_2.11-3.2.11.jar /usr/hdp/current/spark2-client/jars/json4s-native_2.11-3.2.11.jar
cp /root/JARs/slf4s-api_2.11-1.7.25.jar /usr/hdp/current/spark2-client/jars/slf4s-api_2.11-1.7.25.jar
cp /root/JARs/jettison-1.3.7.jar /usr/hdp/current/spark2-client/jars/jettison-1.3.7.jar
cp /root/JARs/mongo-java-driver-3.2.2.jar /usr/hdp/current/spark2-client/jars/mongo-java-driver-3.2.2.jar
cp /root/JARs/casbah-commons_2.11-3.1.1.jar /usr/hdp/current/spark2-client/jars/casbah-commons_2.11-3.1.1.jar
cp /root/JARs/casbah-core_2.11-3.1.1.jar /usr/hdp/current/spark2-client/jars/casbah-core_2.11-3.1.1.jar
cp /root/JARs/casbah-query_2.11-3.1.1.jar /usr/hdp/current/spark2-client/jars/casbah-query_2.11-3.1.1.jar
cp /root/JARs/salat-core_2.11-1.11.2.jar /usr/hdp/current/spark2-client/jars/salat-core_2.11-1.11.2.jar
cp /root/JARs/salat-util_2.11-1.11.2.jar /usr/hdp/current/spark2-client/jars/salat-util_2.11-1.11.2.jar
cp /root/JARs/joda-time-2.3.jar /usr/hdp/current/spark2-client/jars/joda-time-2.3.jar
cp /root/JARs/json4s-ext_2.11-3.2.11.jar /usr/hdp/current/spark2-client/jars/json4s-ext_2.11-3.2.11.jar
cp /root/JARs/kafka-clients-1.1.0.jar /usr/hdp/current/spark2-client/jars/kafka-clients-1.1.0.jar
cp /root/JARs/spline-sample-0.3.1.jar /usr/hdp/current/spark2-client/jars/spline-sample-0.3.1.jar
```

Caso queira, poderá continuar enviando seu JAR na chamada do ```spark-submit```, mas seu JAR pequeno dessa vez, ou seja, apenas com o código feito em Scala sem dependências, pois as dependências já estão no core do Spark, mas eu acredito que seria mais interessante já colocar o seu JAR pequeno dentro do core do Spark, assim você realmente não precisará enviar nada para o ```spark-submit```.

Então, gere seu JAR e copie no mesmo lugar:

```
cp /root/JARs/meu_jar_pequeno.jar /usr/hdp/current/spark2-client/jars/meu_jar_pequeno.jar
```

# Diferença da Persistência em Atlas

A persistência em Atlas tem dois pequenos diferenciais dos demais.

1. Os desenvolvedores do Spline pararam de oferecer suporte ao Atlas na versão 0.3.1, portanto, caso você queira utilizar o Spline persistindo no Atlas é necessário que você utilize as dependências 0.3.1, por isso na sessão __Acabando com o JAR Completo__ é apresentado o download apenas das dependências 0.3.1.

2. Para utilizar o Atlas é preciso colocar um arquivo de modelo de persistência dentro da pasta models do Atlas, é um arquivo JSON que mostra ao Atlas como o Spline enviará as informações para ele persistir em sua linhagem.

A boa notícia é que você pode fazer o download do [Atlas Metal Model](https://search.maven.org/remotecontent?filepath=za/co/absa/spline/spline-persistence-atlas/0.3.1/spline-persistence-atlas-0.3.1-atlas-meta-model.json "Baixar Agora"), caso queira procurar por conta própria, apenas entre no [Search Maven](https://search.maven.org/ "Search Maven") e busque a dependência do __spline-persistence-atlas-0.3.1__, lá terá o modelo para ser baixado.

Veja se serve para você, mas no meu caso, foi copiado assim:

```
cp /root/Model/spline-meta-model.json /usr/hdp/current/atlas-client/models/spline-meta-model.json
```

Um passo muito importante que não deve ser esquecido, após copiar esse arquivo de modelo, reinicie o Atlas, sério, isso não funcionará se não for reiniciado, então sim, você poderá utilizar o Spline persistindo no Atlas.
