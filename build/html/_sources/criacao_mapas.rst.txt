.. _criacao-mapas:

=======================
2. Criação dos mapas
=======================

De modo a ser possível partilhar na Internet, e de forma interativa, os dados georreferenciados :ref:`no passo anterior <georrefenciacao-dados>` (``covid_2020.geojson``), foram criados quatro ficheiros HTML correspondentes a cada mês em estudo (``set.html``, ``out.html``, ``nov.html`` e ``dez.html``), com recurso ao `Leaflet <https://leafletjs.com/>`__, uma biblioteca *open-source* de mapas, em linguagem JavaScript.

Como código-base dos mapas, foi utilizado o HTML relativo ao tutorial do Leaflet `para criar um mapa coroplético interativo <https://leafletjs.com/examples/choropleth/>`__.

2.1. Importar GeoJSON
=====================

Antes da importação do GeoJSON para cada HTML, foi necessário efetuar uma alteração no mesmo, de modo a ser reconhecido pelo Leaflet. Para tal, e com recurso a um editor de texto, a primeira linha foi substituida pelo seguinte código:

.. code-block:: JSON

    data = {

Posteriormente, foi importado o GeoJSON, e respetivas *features*, para cada HTML.

.. code-block:: javascript

    src="data/covid_2020.geojson"

.. code-block:: javascript

    geojson = L.geoJson(data, {
        style: style,
        onEachFeature: onEachFeature
    }).addTo(map);

No caso de um ficheiro em formato GeoJSON, o navegador *web* irá imediatamente fazer *download* de toda a informação, desenhando o contorno da geometria consoante o *zoom* efetuado; aliado ao facto de ser um ficheiro em formato de texto, são as principais vantagens da utilização de um GeoJSON, em prol da utilização de um serviço WMS, por exemplo.

2.2. Adicionar mapa de base
===========================

De forma a espacializar a área em estudo e visualizar os territórios vizinhos, foi adicionado um mapa de base; neste caso, da ESRI.

.. code-block:: javascript

    L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/Canvas/World_Dark_Gray_Base/MapServer/tile/{z}/{y}/{x}', {
        attribution: 'Tiles &copy; Esri &mdash; Esri, DeLorme, NAVTEQ',
        maxZoom:16
    }).addTo(map);

2.3. Definir fronteiras, centro e *zoom* do mapa
================================================

Como os dados são relativos apenas a Portugal Continental, podem ser definidas fronteiras no mapa, restringindo a visualização do mapa pela área que é definida. Para tal, recorreu-se ao seguinte código:

.. code-block:: javascript

    var northWest = L.latLng(42.278,-13.082),
        southEast = L.latLng(36.824,-2.665),
        maxBoundArea = L.latLngBounds(northWest, southEast);

onde a área de fronteira (variável ``maxBoundArea``) corresponde ao ponto definido a noroeste e a sudeste. Sucessivamente, foi adicionada a restrição ao mapa.

.. code-block:: javascript

    var map = L.map('map', {
        center: [39.414020619, -8.075006228],
        zoom: 6,
        maxBounds: maxBoundArea
    });

Aqui, foram também definidas as coordenadas referentes ao centro do mapa (neste caso, corresponde ao Centro Geodésico de Portugal), e o *zoom* pré-definido (neste caso, de acordo com o tamanho definido para cada mapa no :ref:`index <criacao-index>`, e de modo a ser possível visualizar imediatamente toda a informação, definiu-se o *zoom* a nível 6, numa escala de 1 [maior *zoom*] a 16 [menor *zoom*]).

.. _classes:

2.4. Definir classes
====================

As classes foram definidas de acordo com o critério do Centro Europeu de Prevenção e Controlo de Doenças (ECDC), para classificar as áreas consoante a taxa de incidência de COVID-19. Contudo, esse critério tem em consideração o cálculo da taxa de incidência por 100 mil habitantes, em vez de 10 mil habitantes, conforme foi considerado na análise elaborada neste trabalho. Para adaptar o critério a 10 mil habitantes, dividiu-se o limite de cada classe do critério ECDC por 10.

+--------------------------+-------------------------+--------------------------------+
| Critério ECDC            | Critério ECDC adaptado  | Designação do risco            |
| (casos por 100 mil hab.) | (casos por 10 mil hab.) |                                |
+==========================+=========================+================================+
| > 960                    | > 96                    | Extremamente Elevado           |
+--------------------------+-------------------------+--------------------------------+
| 480 - 960                | 48 - 96                 | Muito Elevado                  |
+--------------------------+-------------------------+--------------------------------+
| 240 - 480                | 24 - 48                 | Elevado                        |
+--------------------------+-------------------------+--------------------------------+
| 120 - 240                | 12 -24                  | Moderado                       |
+--------------------------+-------------------------+--------------------------------+
| < 120                    | < 12                    | Baixo a Moderado               |
+--------------------------+-------------------------+--------------------------------+

.. centered:: *Tabela 2:* Classes de risco de incidência de COVID-19, em cada critério.

De seguida, foi atribuída uma cor consoante o intervalo de valores definido, numa escala de amarelo a vermelho; à ausência de valor, foi atribuída a cor cinzenta.

.. _cor-classes:

.. code-block:: javascript

    function getColor(value) {
        return value > 96 ? '#bd0026' :
                value > 48  ? '#f03b20' :
                value > 24  ? '#fd8d3c' :
                value > 12  ? '#fecc5c' :
                value > 0   ? '#ffffb2' :
                            '#e6e6e6';
    }

2.5. Editar estilo das *features*
=================================

Posteriormente, foi alterado o estilo de todos os distritos, relativamente ao contorno e preenchimento dos mesmos:

.. code-block:: javascript

    function style(feature) {
        return {
            weight: 1,
            opacity: 1,
            color: '#333335',
            dashArray: '3',
            fillOpacity: 1,
            fillColor: getColor(feature.properties.casos10k_s)
        };
    }

onde a cor preenchida corresponde a uma das definidas no :ref:`passo anterior <cor-classes>`, tendo como base os valores da coluna escolhida (no exemplo, corresponde à coluna do número de casos de COVID-19 em setembro de 2020, por 10 mil habitantes; :ref:`ver as outras colunas <colunas>`).

Também foi alterado o estilo de todos os distritos, relativamente ao contorno e preenchimento dos mesmos, quando um deles é selecionado.

.. code-block:: javascript

    function highlightFeature(e) {
        var layer = e.target;

        layer.setStyle({
            weight: 4,
            color: '#838383',
            dashArray: '',
            fillOpacity: 1
        });

        if (!L.Browser.ie && !L.Browser.opera && !L.Browser.edge) {
            layer.bringToFront();
        }

        info.update(layer.feature.properties);
    }

Neste caso, e comparando com o estilo de cada distrito quando não se encontra selecionado, o contorno perde o tracejado, ganha maior espessura, e o cinzento torna-se mais intenso.

2.6. Editar estilo das legendas
===============================

Em cada mapa, estão representadas duas legendas à direita: uma na parte superior, contendo o título do mapa e a informação em cada distrito, e uma na parte inferior, contendo as classes do mapa.

No caso da primeira, o código é o seguinte:

.. code-block:: javascript

    info.update = function (props) {
		this._div.innerHTML = '<h4>Taxa de incidência COVID-19<br>(setembro de 2020)</h4>' +  (props ?
			'<b>' + props.Distrito + '</b><br />' + props.casos10k_s.toFixed(1) + ' casos / 10 mil habitantes'
			: 'Selecionar um distrito');
	};

onde, sem um distrito selecionado, a informação a aparecer é o título, seguida da mensagem 'Selecionar um distrito'; com um distrito selecionado, a informação a aparecer é o título, o nome do distrito, o respetivo número de casos por 10 mil habitantes, arredondado a uma casa decimal (no exemplo acima, para setembro), seguido do texto ' casos / 10 mil habitantes'.

No caso da segunda, o código é o seguinte:

.. code-block:: javascript

    var div = L.DomUtil.create('div', 'info legend'),
		grades = [0, 12, 24, 48, 96],
		labels = [],
		from, to;

	for (var i = 0; i < grades.length; i++) {
		from = grades[i];
		to = grades[i + 1];

		labels.push(
			'<i style="background:' + getColor(from + 1) + '"></i> ' +
			from + (to ? ' &ndash; ' + to : ' <'));
	}

	div.innerHTML = '<h5>Casos por 10 mil habitantes</h5>' + labels.join('<br>');
	return div;

onde foi definido o título 'Casos por 10 mil habitantes', as classes :ref:`referidas anteriormente <classes>`, a função com as cores de cada classe :ref:`atribuídas anteriormente <cor-classes>`, e a forma como a legenda é estruturada.

2.7. Adicionar fonte dos dados nos mapas
========================================

Por último, foi adicionada a fonte da informação representada nos mapas; neste caso, o número de casos de COVID-19 por distrito, a população residente em Portugal Continental, no ano de 2018, por distrito, e os limites administratidos dos distritos em Portugal Continental, no ano de 2020.

.. code-block:: javascript

    map.attributionControl.addAttribution('Número de casos &copy; <a href="https://covid19.min-saude.pt/">DGS</a>');
    map.attributionControl.addAttribution('População residente &copy; <a href="https://www.ine.pt/">INE</a>');
    map.attributionControl.addAttribution('Limites dos distritos &copy; <a href="https://www.dgterritorio.gov.pt/cartografia/cartografia-tematica/caop">DGT</a>');
