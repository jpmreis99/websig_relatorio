.. _criacao-index:

========================
3. Criação do *index*
========================

O ``index.html`` é caracterizado por, regra geral, ser a página principal de um *website*. Neste caso, o ``index.html`` contém os quatro mapas :ref:`criados anteriormente <criacao-mapas>` (``set.html``, ``out.html``, ``nov.html`` e ``dez.html``), de modo a ser possível efetuar uma comparação da evolução pandémica em Portugal Continental, por distrito, ao longo dos quatro meses em estudo.

3.1. Definir metadados do *website*
===================================

Primeiramente, foram definidos, dentro da *tag* ``<head>``, os elementos referentes aos metadados do *website*, nomeadamente:

- A codificação dos caracteres;

.. code-block:: html

    <meta charset="UTF-8">

- O CSS relativo ao estilo do *website*, :ref:`criado posteriormente <ficheiro-estilos>`;

.. code-block:: html

    <link rel="stylesheet" href="css/style.css">

- O título e ícone representados no separador do navegador *web*;

.. code-block:: html

    <title>Evolução da COVID-19 em Portugal Continental</title>
    <link rel="icon" href="img/logo_igot.png">

.. _icones:

- A base de dados de ícones da `Font Awesome <https://fontawesome.com/>`__, utilizada no :ref:`cabeçalho <cabecalho>` do *website*.

.. code-block:: html

    <script src="https://kit.fontawesome.com/7f6def7b3d.js" crossorigin="anonymous"></script>

3.2. Definir corpo do *website*
===============================

Posteriormente, foram definidas, dentro da *tag* ``<body>``, duas divisórias (``<div>``) no corpo do *website*, nomeadamente:

.. _cabecalho:

- O cabeçalho, definido por 'header', composto à direita pelo ícone de um vírus :ref:`da base de dados de ícones da Font Awesome <icones>`, pelo título do *website* ao centro, e pelo logotipo do Instituto de Geografia e Ordenamento do Território (IGOT) à direita;

.. code-block:: html

    <div class="header">
        <p><i class='fas fa-virus' style='float:left;font-size:32px;color:#ff0000'></i>Evolução da COVID-19 em Portugal Continental<img src="img/logo_igot.png" style="float:right;width:32px;height:32px"></p>
    </div>

- Uma grelha, definida por 'cards', onde cada subdivisão é um 'card' (ao todo, são quatro).

.. code-block:: html

    <div class="cards">
        <div class="card">
        </div>
        <div class="card">
        </div>
        <div class="card">
        </div>
        <div class="card">
        </div>
    </div>

3.3. Importar os mapas
======================

Dentro de cada subdivisão, foi importado um dos mapas :ref:`criados anteriormente <criacao-mapas>` (``set.html``, ``out.html``, ``nov.html`` e ``dez.html``), com o título respetivo a cada mês:

.. code-block:: html

    <p>setembro de 2020</p>
    <iframe src="set.html" frameborder="0" style="height:480px;width:100%;"></iframe>

.. code-block:: html

    <p>outubro de 2020</p>
    <iframe src="out.html" frameborder="0" style="height:480px;width:100%;"></iframe>

.. code-block:: html

    <p>novembro de 2020</p>
    <iframe src="nov.html" frameborder="0" style="height:480px;width:100%;"></iframe>

.. code-block:: html

    <p>dezembro de 2020</p>
    <iframe src="dez.html" frameborder="0" style="height:480px;width:100%;"></iframe>

onde todos os mapas não possuem margem, e acompanham o tamanho da janela do navegador *web*.

.. _ficheiro-estilos:

3.4. Elaborar o ficheiro de estilos
===================================

Como último passo, foi estruturado o ``style.css``, contendo o estilo de apresentação do ``index.html``. Dividido em grupos, foi definido o estilo:

- Do '<html>' e do '<body>' (abrangendo, portanto, todo o *website*), como o tipo e tamanho de letra, a cor do fundo e a largura mínima do *website*;

.. code-block:: css

    html { font-family: Arial, Helvetica, sans-serif; font-size: 16px; }
    body { min-width:780px; padding: 16px; background-color:#19191a}

- Da divisória 'header', como a altura e largura, as margens e contornos do elemento ou a cor e tamanho da letra, afetando apenas esta divisória;

.. code-block:: css

    .header {
        height: 80px;
        min-width: 600px;
        max-width: 1236px;
        margin: 0 auto;
        margin-bottom: 36px;
        background-color: #222327;
        border-style: solid;
        border-width: 2px;
        border-radius: 7px;
        border-color: white;
        padding: 0px 32px;
        text-align: center;
        color: white;
        font-size: 24px;
        font-weight: bold;
    }

- Da divisória 'card', como a disposição das subdivisões (neste caso em *grid*), o comportamento da mesma quando a largura da janela do navegador *web* é alterada (neste caso, quando a largura de cada subdivisão é inferior a 640 píxeis, a *grid* passa apenas a uma coluna, em vez de duas), e o espaçamento entre cada subdivisão;

.. code-block:: css

    .cards {
        max-width: 1304px;
        margin: 0 auto;
        display: grid;
        grid-gap: 16px;
        grid-template-columns: repeat(auto-fit, minmax(640px, 1fr));
    }

- Das subdivisórias 'card', como a cor do fundo, os contornos de cada elemento e a cor e formatação do texto, afetando apenas estas subdivisórias.

.. code-block:: css

    .card {
        background-color: #222327;
        border-style: solid;
        border-width: 2px;
        border-radius: 7px;
        border-color: white;
        text-align: center;
        color: white;
        font-weight: bold;
        height: 532px;
        padding: 2px 0px 0px 0px;
    }
