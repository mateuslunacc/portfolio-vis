---
title: "Lab 1"
date: 2017-11-10T14:42:39-03:00
draft: false
---

<h2>Análise de dados sobre o açude Epitácio Pessoa.</h2>

<h3> Volume do açude ao longo do tempo</h3>
<div id="vis" width=300></div>

<h3>O volume morto</h3>
<div id="vis2" width=300></div>

<h3>O volume dividido pelas estações do ano</h3>
<div id="vis3" width=300></div>

<h3>Quando o açude sangrou</h3>
<div id="vis4" width=300></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/vega/3.0.7/vega.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vega-lite/2.0.1/vega-lite.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vega-embed/3.0.0-rc7/vega-embed.js"></script>
<script>
    const spec = "../volume.json";
    const spec2 = "../volumemorto.json";
    const spec3 = "../estacoes.json";
    const spec4 = "../sangrou.json";

  	vegaEmbed('#vis', spec).catch(console.warn);
  	vegaEmbed('#vis2', spec2).catch(console.warn);
  	vegaEmbed('#vis3', spec3).catch(console.warn);
  	vegaEmbed('#vis4', spec4).catch(console.warn);

</script>