---
title: "São João de Campina Grande não é o responsável pela seca do Açude."
date: 2017-11-15T15:30:59-03:00
draft: false
---

<div id="text" width=300></div>
O Maior São João do Mundo não prejudica significativamente o volume das águas do açude, não tão quanto a má administração do mesmo e a própria evaporação, decorrente do clima da caatinga.
Veja no gráfico que, desde o ano de 1990, em nenhum período alto do São João (junho) o açude atingiu o mínimo do ano.

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
    "text": "Volume Mínimo do Açude de Boqueirão no Mês do São João"
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
          "type": "filter",
          "expr": "inrange(time(datetime(0, month(datum[\"DataInformacao\"]), 1, 0, 0, 0, 0)), [time(datetime(0, 5, 1, 0, 0, 0, 0)), time(datetime(0, 5, 1, 0, 0, 0, 0))])"
        },
        {
          "type": "formula",
          "as": "yearmonth_DataInformacao",
          "expr": "datetime(year(datum[\"DataInformacao\"]), month(datum[\"DataInformacao\"]), 1, 0, 0, 0, 0)"
        },
        {
          "type": "aggregate",
          "groupby": ["yearmonth_DataInformacao"],
          "ops": ["min"],
          "fields": ["VolumePercentual"],
          "as": ["min_VolumePercentual"]
        },
        {
          "type": "filter",
          "expr": "datum[\"yearmonth_DataInformacao\"] !== null && !isNaN(datum[\"yearmonth_DataInformacao\"])"
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
      "type": "rect",
      "style": ["bar"],
      "from": {"data": "data_0"},
      "encode": {
        "update": {
          "interpolate": {"value": "monotone"},
          "xc": {"scale": "x","field": "yearmonth_DataInformacao"},
          "width": {"value": 5},
          "y": {"scale": "y","field": "min_VolumePercentual"},
          "y2": {"scale": "y","value": 0},
          "fill": {"value": "#BE9063"}
        }
      }
    },
    {
      "name": "layer_1_marks",
      "type": "line",
      "style": ["line"],
      "sort": {
        "field": "datum[\"year_DataInformacao\"]",
        "order": "descending"
      },
      "from": {"data": "data_1"},
      "encode": {
        "update": {
          "interpolate": {"value": "monotone"},
          "x": {"scale": "x","field": "year_DataInformacao"},
          "y": {"scale": "y","field": "min_VolumePercentual"},
          "stroke": {"value": "#040C0E"}
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
          {
            "data": "data_0",
            "field": "yearmonth_DataInformacao"
          },
          {"data": "data_1","field": "year_DataInformacao"}
        ],
        "sort": true
      },
      "range": [0,{"signal": "width"}],
      "padding": 5
    },
    {
      "name": "y",
      "type": "linear",
      "domain": {
        "fields": [
          {"data": "data_0","field": "min_VolumePercentual"},
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
      "values": [
        {"signal": "datetime(1990, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1991, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1992, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1993, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1994, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1995, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1996, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1997, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1998, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1999, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2000, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2001, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2002, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2003, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2004, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2005, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2006, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2007, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2008, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2009, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2010, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2011, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2012, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2013, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2014, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2015, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2016, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2017, 5, 1, 0, 0, 0, 0)"}
      ],
      "scale": "x",
      "orient": "bottom",
      "labelFlush": true,
      "labelOverlap": true,
      "tickCount": {"signal": "ceil(width/40)"},
      "zindex": 1,
      "encode": {
        "labels": {
          "update": {
            "text": {"signal": "timeFormat(datum.value, '%b %Y')"}
          }
        }
      }
    },
    {
      "values": [
        {"signal": "datetime(1990, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1991, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1992, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1993, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1994, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1995, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1996, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1997, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1998, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(1999, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2000, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2001, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2002, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2003, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2004, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2005, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2006, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2007, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2008, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2009, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2010, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2011, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2012, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2013, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2014, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2015, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2016, 5, 1, 0, 0, 0, 0)"},
        {"signal": "datetime(2017, 5, 1, 0, 0, 0, 0)"}
      ],
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






