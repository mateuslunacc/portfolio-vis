---
title: "Lab 2"
date: 2017-11-28T08:59:01-03:00
draft: false
---

<script src="https://d3js.org/d3.v4.min.js"></script>
<h3>
	1. Pontos com a posição horizontal sendo o 90-percentil e a vertical 10-percentil, e a cor do ponto dizendo se é mês de período chuvoso ou não. 
</h3>
</div>
<div class="row mychart" id="chart">
</div>
</div>

<style>
.mychart rect {
  fill: steelblue;
}

.mychart rect:hover {
  fill: goldenrod;
}

.mychart text {
  font: 12px sans-serif;
  text-anchor: left;
}
</style>

<script type="text/javascript">

function desenhaPontos(dados) {
  var alturaSVG = 400, larguraSVG = 900;
  var margin = {top: 10, right: 20, bottom:30, left: 45}, 
      larguraVis = larguraSVG - margin.left - margin.right,
      alturaVis = alturaSVG - margin.top - margin.bottom;

  var grafico = d3.select('#chart')
    .append('svg')
      .attr('width', larguraVis + margin.left + margin.right)
      .attr('height', alturaVis + margin.top + margin.bottom)
    .append('g') 
      .attr('transform', 'translate(' +  margin.left + ',' + margin.top + ')');

  var x = d3.scaleLinear()
            .domain([d3.min(dados, (d) => d.noventa_percentil) - 1, d3.max(dados, (d) => d.noventa_percentil) + 1])
            .rangeRound([0, larguraVis]);

  var y = d3.scaleLinear()
            .domain([d3.min(dados, (d) => d.dez_percentil) - 1, d3.max(dados, (d) => d.dez_percentil) + 1])
            .rangeRound([alturaVis, 0]);

  grafico.selectAll('g')
          .data(dados)
          .enter()
            .append('circle')
              .attr('cx', d => x(d.noventa_percentil))
              .attr('r', 10)
              .attr('cy', d => y(d.dez_percentil))
              .style("fill", function(d) { // Periodo de chuva de acordo com https://pt.wikipedia.org/wiki/Campina_Grande#Clima
                if(d.mes > 3 && d.mes < 8) {
                  return "blue";
                } else {
                  return "yellow";
                }
              });;

  grafico.append("g")
          .attr("class", "x axis")
          .attr("transform", "translate(0," + alturaVis + ")")
          .call(d3.axisBottom(x));

  grafico.append('g')
          .attr('transform', 'translate(0,0)')
          .call(d3.axisLeft(y))

  grafico.append("text")
    .attr("transform", "translate(-35," + (alturaVis + margin.top)/2 + ") rotate(-90)")
    .text("10 percentil");

}

d3.json('../boqueirao-por-mes.json', function(dados) {
  desenhaPontos(dados);
});

</script>
