---
title: "Quando o Açude estiver cheio, se prepare..."
date: 2017-11-13T21:14:36-03:00
draft: false
---

<div id="text" width=300></div>

... não para ir comemorar na "sangria", mas para o período de seca no açude que vem  logo em seguida.
Dê uma olhada no gráfico abaixo nos períodos em que o açude atingiu o seu volume máximo: 1995, 2004, 2008, 2012.
Perceba que, após um curto período de tempo, cerca de 2 anos, o volume do açude começa a cair bruscamente.
Péssima administração das águas é um fator gravíssimo sim, mas o primeiro responsável por esta desagradável situação são os consumidores.
Ver que o açude está cheio tráz uma sensação de conforto e de falta de preocupação em poupar a água. Aliado com a falta de chuvas prolongadas, o volume do açude, então, começa a diminuir rapidamente.
A consciência do uso moderado da água de toda a população, incluindo indústria e agricultores, não só em períodos de baixo volume, dispensaria qualquer administração e períodos de falta de água. 

<div id="vis" width=300></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/vega/3.0.7/vega.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vega-lite/2.0.1/vega-lite.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vega-embed/3.0.0-rc7/vega-embed.js"></script>

<script>
    const spec = {
    "$schema": "https://vega.github.io/schema/vega/v3.0.json",
  "autosize": "pad",
  "padding": 5,
  "width": 900,
  "height": 450,
  "title": {
    "text": "Volume Máximo e Mínimo do Açude de Boqueirão, por ano"
  },
  "style": "cell",
  "data": [
    {
      "name": "source_0",
      "url": "https://api.insa.gov.br/reservatorios/12172/monitoramento",
      "format": {
        "type": "json",
        "property": "volumes",
        "parse": {"DataInformacao": "utc:'%d/%m/%Y'"}
      }
    },
    {
      "name": "data_0",
      "source": "source_0",
      "transform": [
        {
          "type": "formula",
          "expr": "toDate(datum[\"DataInformacao\"])",
          "as": "DataInformacao"
        },
        {
          "type": "formula",
          "expr": "toNumber(datum[\"VolumePercentual\"])",
          "as": "VolumePercentual"
        },
        {
          "type": "formula",
          "as": "year_DataInformacao",
          "expr": "datetime(year(datum[\"DataInformacao\"]), 0, 1, 0, 0, 0, 0)"
        },
        {
          "type": "aggregate",
          "groupby": ["year_DataInformacao"],
          "ops": ["max"],
          "fields": ["VolumePercentual"],
          "as": ["max_VolumePercentual"]
        },
        {
          "type": "filter",
          "expr": "datum[\"year_DataInformacao\"] !== null && !isNaN(datum[\"year_DataInformacao\"])"
        }
      ]
    },
    {
      "name": "data_1",
      "source": "source_0",
      "transform": [
        {
          "type": "formula",
          "expr": "toDate(datum[\"DataInformacao\"])",
          "as": "DataInformacao"
        },
        {
          "type": "formula",
          "expr": "toNumber(datum[\"VolumePercentual\"])",
          "as": "VolumePercentual"
        },
        {
          "type": "formula",
          "as": "year_DataInformacao",
          "expr": "datetime(year(datum[\"DataInformacao\"]), 0, 1, 0, 0, 0, 0)"
        },
        {
          "type": "aggregate",
          "groupby": ["year_DataInformacao"],
          "ops": ["min"],
          "fields": ["VolumePercentual"],
          "as": ["min_VolumePercentual"]
        },
        {
          "type": "filter",
          "expr": "datum[\"year_DataInformacao\"] !== null && !isNaN(datum[\"year_DataInformacao\"])"
        }
      ]
    }
  ],
  "marks": [
    {
      "name": "layer_0_marks",
      "type": "area",
      "style": ["area"],
      "sort": {
        "field": "datum[\"year_DataInformacao\"]",
        "order": "descending"
      },
      "from": {"data": "data_0"},
      "encode": {
        "update": {
          "interpolate": {"value": "monotone"},
          "orient": {"value": "vertical"},
          "x": {"scale": "x","field": "year_DataInformacao"},
          "y": {"scale": "y","field": "max_VolumePercentual"},
          "y2": {"scale": "y","value": 0},
          "fill": {"value": "#192E5B"}
        }
      }
    },
    {
      "name": "layer_1_marks",
      "type": "area",
      "style": ["area"],
      "sort": {
        "field": "datum[\"year_DataInformacao\"]",
        "order": "descending"
      },
      "from": {"data": "data_1"},
      "encode": {
        "update": {
          "interpolate": {"value": "monotone"},
          "orient": {"value": "vertical"},
          "x": {"scale": "x","field": "year_DataInformacao"},
          "y": {"scale": "y","field": "min_VolumePercentual"},
          "y2": {"scale": "y","value": 0},
          "fill": {"value": "#72A2C0"}
        }
      }
    }
  ],
  "scales": [
    {
      "name": "x",
      "type": "time",
      "domain": {
        "fields": [
          {"data": "data_0","field": "year_DataInformacao"},
          {"data": "data_1","field": "year_DataInformacao"}
        ],
        "sort": true
      },
      "range": [0,{"signal": "width"}]
    },
    {
      "name": "y",
      "type": "linear",
      "domain": {
        "fields": [
          {"data": "data_0","field": "max_VolumePercentual"},
          {"data": "data_1","field": "min_VolumePercentual"}
        ],
        "sort": true
      },
      "range": [{"signal": "height"},0],
      "nice": true,
      "zero": true
    }
  ],
  "axes": [
    {
      "title": "",
      "scale": "x",
      "orient": "bottom",
      "labelFlush": true,
      "labelOverlap": true,
      "tickCount": {"signal": "ceil(width/40)"},
      "zindex": 1,
      "encode": {
        "labels": {
          "update": {
            "text": {"signal": "timeFormat(datum.value, '%Y')"}
          }
        }
      }
    },
    {
      "scale": "x",
      "orient": "bottom",
      "domain": false,
      "grid": true,
      "labels": false,
      "maxExtent": 0,
      "minExtent": 0,
      "tickCount": {"signal": "ceil(width/40)"},
      "ticks": false,
      "zindex": 0,
      "gridScale": "y"
    },
    {
      "title": "Volume (%)",
      "scale": "y",
      "orient": "left",
      "labelOverlap": true,
      "tickCount": {"signal": "ceil(height/40)"},
      "zindex": 1
    },
    {
      "scale": "y",
      "orient": "left",
      "domain": false,
      "grid": true,
      "labels": false,
      "maxExtent": 0,
      "minExtent": 0,
      "tickCount": {"signal": "ceil(height/40)"},
      "ticks": false,
      "zindex": 0,
      "gridScale": "x"
    }
  ],
  "config": {"axisY": {"minExtent": 30}}
};
  	vegaEmbed('#vis', spec).catch(console.warn);
</script>





