=======================================
1. Preparação do material necessário
=======================================

1.1. Instalar o servidor *web*
==============================

Um servidor *web* é caracterizado por disponibilizar e aceitar pedidos através do protocolo HTTP, possibilitando a consulta de aplicações; mais concretamente, os ficheiros que estão contidos nas mesmas. Para tal, foi instalado o Apache Tomcat, que permite a consulta de aplicações em linguagem Java.

No terminal do Ubuntu, foi instalada a versão 9 do Apache Tomcat através do comando:

.. code-block:: console
    
    sudo apt install tomcat9

Posteriormente, foi criada, na pasta ``/usr``, uma aplicação denominada 'websig', para alojar os ficheiros resultantes dos mapas e do *index*. Contudo, para aceder à mesma, foi necessário criar um ficheiro na diretoria do Apache Tomcat, que referencie a aplicação; para tal, na diretoria ``/etc/tomcat9/Catalina/localhost``, foi criado um ficheiro, com o nome ``websig.xml``, referenciando a pasta respetiva à aplicação.

.. code-block:: html

    <Context
        docBase="/usr/websig/"
        path="/websig"
    />

1.2. Calcular a taxa de incidência de COVID-19
================================================

A taxa de incidência de uma determinada doença é calculada através da divisão entre o número de casos dessa doença, e o total de população em risco (neste caso, a população residente). No caso da COVID-19, esta métrica é multiplicada por 10 mil ou 100 mil habitantes, e é calculada por unidade territorial; deste modo, é possivel constatar quais os territórios onde a taxa de incidência é mais elevada ou mais baixa, e estabelecer medidas de controlo da propagação da doença caso essa taxa ultrapasse um determinado valor.

Para este trabalho, a métrica foi calculada por 10 mil habitantes, de setembro a dezembro de 2020, e as unidades territoriais foram os distritos de Portugal Continental.

Através do ficheiro disponibilizado pelo docente, a taxa de incidência foi obtida dividindo a coluna correspondente ao total de casos por mês, por distrito, e a coluna correspondente à população residente em 2018, por distrito. No final, obtiveram-se as seguintes colunas, no ficheiro ``covid_2020.xls``:

.. _colunas:

+------------+------------------------------------------------------------------------+
| Coluna     | Designação                                                             |
+============+========================================================================+
| distrito   | Distritos de Portugal Continental                                      |
+------------+------------------------------------------------------------------------+
| set        | Número de casos de COVID-19 em setembro de 2020                        |
+------------+------------------------------------------------------------------------+
| out        | Número de casos de COVID-19 em outubro de 2020                         |
+------------+------------------------------------------------------------------------+
| nov        | Número de casos de COVID-19 em novembro de 2020                        |
+------------+------------------------------------------------------------------------+
| dez        | Número de casos de COVID-19 em dezembro de 2020                        |
+------------+------------------------------------------------------------------------+
| pop_2018   | População residente em 2018                                            |
+------------+------------------------------------------------------------------------+
| casos10k_s | Número de casos de COVID-19 em setembro de 2020, por 10 mil habitantes |
+------------+------------------------------------------------------------------------+
| casos10k_o | Número de casos de COVID-19 em outubro de 2020, por 10 mil habitantes  |
+------------+------------------------------------------------------------------------+
| casos10k_n | Número de casos de COVID-19 em novembro de 2020, por 10 mil habitantes |
+------------+------------------------------------------------------------------------+
| casos10k_d | Número de casos de COVID-19 em dezembro de 2020, por 10 mil habitantes |
+------------+------------------------------------------------------------------------+

.. centered:: *Tabela 1:* Colunas do ficheiro ``covid_2020.xls``.

.. _georrefenciacao-dados:

1.3. Georreferenciar os dados
=============================

A georreferenciação de informação proveniente de tabelas permite obter uma visualização espacial da informação, observando potenciais dinâmicas territoriais que, à escala das tabelas, não é possível observar de uma forma imediata.

A `Carta Administrativa Oficial de Portugal (CAOP) <https://www.dgterritorio.gov.pt/cartografia/cartografia-tematica/caop>`__ contém os limites administrativos das diversas unidades territoriais do país, em formato *shapefile*; neste caso, foram utilizados os limites correspondentes a 2020, para Portugal Continental.

Num ambiente de Sistemas de Informação Geográfica (SIG), como o QGIS ou o ArcMap, foi importado o *shapefile* descarregado. Como os dados estão ao nível das freguesias, foi necessário realizar um ``Dissolve`` pela coluna 'Distrito', de modo a obter apenas os distritos, resultando no ficheiro ``distritos.shp``.

Posteriormente, foi efetuado um ``Join`` entre os distritos e a tabela com o número de casos de COVID-19 por 10 mil habitantes (``covid_2020.xls``), utilizando a coluna ``distrito``. No final, foi exportado o resultado dessa junção, em formato GeoJSON, com o nome ``covid_2020.geojson``.
