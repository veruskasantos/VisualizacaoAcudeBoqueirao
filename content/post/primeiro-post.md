---
title: "Primeiro Post"
date: 2017-11-14T21:14:36-03:00
draft: false
---

<div id="vis" width=300></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/vega/3.0.7/vega.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vega-lite/2.0.1/vega-lite.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vega-embed/3.0.0-rc7/vega-embed.js"></script>

<script>
    const spec = {
     "width": 900,
  "height": 450,
  "$schema": "https://vega.github.io/schema/vega-lite/v2.json",
  "title": "Situação do Açude de Boqueirão",
  "data": {
    "url": "https://api.insa.gov.br/reservatorios/12172/monitoramento",
    "format": {
      "type": "json",
      "property": "volumes",
      "parse": {
        "DataInformacao": "utc:'%d/%m/%Y'"
      }
    }
  },
  "mark": {
    "type": "area",
    "interpolate": "monotone"
  },
  "encoding": {
    "x": {
      "timeUnit": "yearmonthdate",
      "field": "DataInformacao",
      "type": "temporal",
      "axis": {
        "title": ""
      }
    },
    "y": {
      "field": "VolumePercentual",
      "type": "quantitative",
      "axis": {
        "title": "Volume (%)"
      }
    },

    "color": {"field": "DataInformacao", "type": "temporal"}
  },
  "transform": [
    {
      "filter": {
        "timeUnit": "year",
        "field": "DataInformacao",
        "range": [
          2017,
          2017
        ]
      }
    },
    {
      "filter": {
        "timeUnit": "month",
        "field": "DataInformacao",
        "range": [
          1,
          11
        ]
      }
    }
  ]
     };
  	vegaEmbed('#vis', spec).catch(console.warn);
</script>





