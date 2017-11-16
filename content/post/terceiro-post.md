---
title: "Volume do Açude de Boqueirão vem crescendo."
date: 2017-11-15T15:31:06-03:00
draft: false
---
<div id="text" width=300></div>
O açude de Boqueirão, no Agreste da Paraíba, atingiu o seu volume morto em 18 de junho de 2016 ao atingir a marca de 8,22%, segundo o G1.
Após décadas de espera, a transposição das águas do Rio São Franscisco chegou finalmente ao açude em abril de 2017, trazendo esperança aos que dele dependem.
Lentamente o volume do açude está subindo, porém em alguns períodos, como final de agosto, a vazão de saída é maior que a vazão de entrada da água.
Hoje, o açude atinge a marca de 9,3% da sua capacidade, o que corresponde a 38.150.000 m³ de água, segundo os dados do INSA.
Acompanhe a situação do volume no gráfico abaixo, desde janeiro deste ano.
Faça sua parte, não desperdice água!

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
  "title": {"text": "Situação do Açude de Boqueirão em 2017"},
  "style": "cell",
  "data": [
    {
      "name": "source_0",
      "url": "https://api.insa.gov.br/reservatorios/12172/monitoramento",
      "format": {
        "type": "json",
        "property": "volumes",
        "parse": {
          "DataInformacao": "utc:'%d/%m/%Y'",
          "VolumePercentual": "number"
        }
      },
      "transform": [
        {
          "type": "filter",
          "expr": "inrange(time(datetime(year(datum[\"DataInformacao\"]), 0, 1, 0, 0, 0, 0)), [time(datetime(2017, 0, 1, 0, 0, 0, 0)), time(datetime(2017, 0, 1, 0, 0, 0, 0))])"
        },
        {
          "type": "filter",
          "expr": "inrange(time(datetime(0, month(datum[\"DataInformacao\"]), 1, 0, 0, 0, 0)), [time(datetime(0, 0, 1, 0, 0, 0, 0)), time(datetime(0, 10, 1, 0, 0, 0, 0))])"
        },
        {
          "type": "formula",
          "as": "yearmonthdate_DataInformacao",
          "expr": "datetime(year(datum[\"DataInformacao\"]), month(datum[\"DataInformacao\"]), date(datum[\"DataInformacao\"]), 0, 0, 0, 0)"
        },
        {
          "type": "impute",
          "field": "VolumePercentual",
          "groupby": ["DataInformacao"],
          "key": "yearmonthdate_DataInformacao",
          "method": "value",
          "value": 0
        },
        {
          "type": "stack",
          "groupby": ["yearmonthdate_DataInformacao"],
          "field": "VolumePercentual",
          "sort": {
            "field": ["DataInformacao"],
            "order": ["descending"]
          },
          "as": ["VolumePercentual_start","VolumePercentual_end"],
          "offset": "zero"
        },
        {
          "type": "filter",
          "expr": "datum[\"DataInformacao\"] !== null && !isNaN(datum[\"DataInformacao\"]) && datum[\"VolumePercentual\"] !== null && !isNaN(datum[\"VolumePercentual\"])"
        }
      ]
    }
  ],
  "marks": [
    {
      "name": "pathgroup",
      "type": "group",
      "from": {
        "facet": {
          "name": "faceted_path_main",
          "data": "source_0",
          "groupby": ["DataInformacao"]
        }
      },
      "encode": {
        "update": {
          "width": {"field": {"group": "width"}},
          "height": {"field": {"group": "height"}}
        }
      },
      "marks": [
        {
          "name": "marks",
          "type": "area",
          "style": ["area"],
          "sort": {
            "field": "datum[\"yearmonthdate_DataInformacao\"]",
            "order": "descending"
          },
          "from": {"data": "faceted_path_main"},
          "encode": {
            "update": {
              "interpolate": {"value": "monotone"},
              "orient": {"value": "vertical"},
              "x": {
                "scale": "x",
                "field": "yearmonthdate_DataInformacao"
              },
              "y": {"scale": "y","field": "VolumePercentual_end"},
              "y2": {
                "scale": "y",
                "field": "VolumePercentual_start"
              },
              "fill": {"scale": "color","field": "DataInformacao"}
            }
          }
        }
      ]
    }
  ],
  "scales": [
    {
      "name": "x",
      "type": "time",
      "domain": {
        "data": "source_0",
        "field": "yearmonthdate_DataInformacao"
      },
      "range": [0,{"signal": "width"}]
    },
    {
      "name": "y",
      "type": "linear",
      "domain": {
        "data": "source_0",
        "fields": ["VolumePercentual_start","VolumePercentual_end"],
        "sort": true
      },
      "range": [{"signal": "height"},0],
      "nice": true,
      "zero": true
    },
    {
      "name": "color",
      "type": "sequential",
      "domain": {"data": "source_0","field": "DataInformacao"},
      "range": "ramp",
      "nice": false,
      "zero": false
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
            "text": {
              "signal": "timeFormat(datum.value, '%b %d, %Y')"
            }
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
  "legends": [
    {
      "fill": "color",
      "title": "DataInformacao",
      "encode": {
        "labels": {
          "update": {
            "text": {
              "signal": "timeFormat(datum.value, '%b %d, %Y')"
            }
          }
        }
      }
    }
  ],
  "config": {"axisY": {"minExtent": 30}}
};
  	vegaEmbed('#vis', spec).catch(console.warn);
</script>






