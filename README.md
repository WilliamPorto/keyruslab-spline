# SPLINE

A intenção deste documento é compartilhar conhecimento e algumas soluções que podem ser complementares às funcionalidades do SPLINE.

SPLINE (de **S**park **L**ineage) é um projeto que ajuda pessoas a terem insights sobre dados processados pelo Apache Spark.

O projeto consiste em duas partes:

#### Uma biblioteca principal que trabalha com a parte dos drivers, capturando e registrando a linhagem de dados dos jobs que estao sendo executados.

* O projeto do SPLINE foi desenvolvido somente para o Spark Scala. [Segue documentação](https://github.com/WilliamPorto/keyruslab-spline/blob/master/Integração.md "Segue documentação") de como integrar o SPLINE com o PySpark.

#### E uma Web User Interface para a visualização dos dados registrados.

* A Web User Interface do próprio SPLINE utiliza como base o MongoDB e pode ser verificada no site oficial para saber como subi-la.

* Já a utilização do Atlas deixa um pouco a desejar no site oficial, devido a falta de alguns exemplos e passos importantes para a implementação. [Segue uma descrição](https://github.com/WilliamPorto/keyruslab-spline/blob/master/Persistência.md "Segue uma descrição") de como trabalhar com as diferentes alternativas de persistência e utilização do Atlas como persistência de linhagem.

__Para esse projeto foi utilizado a Hortonworks Sandbox HDP 2.6.5 na Oracle VM VirtualBox com: CentOS Linux 7 (Core) - Python 2.7.5 - Spark 2.3.0.2.6.5.0 - Apache Atlas 0.8.0.2.6.5.0 - Spline 0.3.1 - Kafka 1.0.0.2.6.5.0__

#### Referências

[Spline Oficial](https://absaoss.github.io/spline/ "Spline Oficial")

#### Downloads

[Downloads](https://github.com/WilliamPorto/keyruslab-spline/tree/master/Downloads "Downloads")
