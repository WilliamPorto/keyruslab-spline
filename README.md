# SPLINE

A intenção deste documento é compartilhar conhecimento e algumas soluções que podem ser complementares às funcionalidades do SPLINE

SPLINE (de **S**park **L**ineage) é um proejto que ajuda pessoas a terem insights sobre dados processados pelo Apache Spark.

O projeto consiste em duas partes:

#### Uma biblioteca principal que trabalha com a parte dos drivers, capturando e registrando a data lineage dos jobs que estao sendo executados.

* O projeto do SPLINE foi desenvolvido somente para o Spark Scala. [Segue documentação](https://github.com/WilliamPorto/keyruslab-spline/blob/master/PySpark.md "Segue documentação") de como integrar o SPLINE com o PySpark.

#### E uma Web UserInterface para a visualização dos dados registrados.

* A documentação oficial deixa a desejar, faltam alguns exemplos e passos importantes para a implementação. [Segue uma descrição](https://github.com/WilliamPorto/keyruslab-spline/blob/master/Persistence.md "Segue uma descrição") de como trabalhar com as difetentes alternativas de persistência.
