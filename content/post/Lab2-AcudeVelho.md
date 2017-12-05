---
title: "Lab 2 - Açude Velho"
date: 2017-12-05T08:55:15-03:00
draft: false
---

<script src="https://d3js.org/d3.v4.min.js"></script>
<h4>
1 - Observar os horários que as pessoas não motorizadas mais transitam no açude velho.
2 - Observar os horários que os veículos mais transitam no açude velho.
3 - Identificar o gênero de pessoa que mais utiliza a ciclovia.
4 - Visualizar a proporção de carros e motos.
</h4>
<br>
</div>
<div class="row mychart" id="chart">
</div>
</div>

<style>
chart circle:hover {
  fill: goldenrod;
}

.mychart text {
  font: 12px sans-serif;
  text-anchor: left;
}
</style>

<script type="text/javascript">

function somaPontos(dados, horario) {
	
}

function desenhaPontos(dados) {
  var alturaSVG = 400, larguraSVG = 2500;
  var margin = {top: 20, right: 50, bottom:50, left: 50}, 
      larguraVis = larguraSVG - margin.left - margin.right,
      alturaVis = alturaSVG - margin.top - margin.bottom;

  var grafico = d3.select('#chart')
    .append('svg')
      .attr('width', larguraVis + margin.left + margin.right)
      .attr('height', alturaVis + margin.top + margin.bottom)
    .append('g') 
      .attr('transform', 'translate(' +  margin.left + ',' + margin.top + ')');

  var x = d3.scaleBand()
            .domain(dados.map((dado) => dado.horario_inicial))
	        .rangeRound([0, larguraVis])
	        .padding(1); // Configure essa escala com domain, range e padding

  var y = d3.scaleLinear()
            .domain([(d3.min(dados, (d) => d.total_ciclistas) + d3.min(dados, (d) => d.total_pedestres)), 
            		  d3.max(dados, (d) => d.total_ciclistas) + d3.max(dados, (d) => d.total_pedestres)])
            .rangeRound([alturaVis, 0]);

  grafico.selectAll('g')
          .data(dados)
          .enter()
            .append('rect')
              .attr('x', dado => x(dado.horario_inicial))
              .attr('y', dado => y(dado.total_pedestres + dado.total_ciclistas))

  grafico.selectAll('text')
          .data(dados)
          .enter()
          .append("text")
          .attr("x", dado => x(dado.horario_inicial))
          .attr("y", dado => y(dado.total_pedestres + dado.total_ciclistas))
          .text(dado => dado.total_pedestres + dado.total_ciclistas);

  grafico.append("g")
          .attr("class", "x axis")
          .attr("transform", "translate(0," + alturaVis + ")")
          .call(d3.axisBottom(x));

  grafico.append('g')
          .attr('transform', 'translate(0,0)')
          .call(d3.axisLeft(y))

  grafico.append("text")
    .attr("transform", "translate(-40," + (alturaVis + margin.top)/2 + ") rotate(-90)")
    .text("Não motorizados");

  grafico.append("text")
    .attr("transform", "translate(" + (larguraVis - margin.left)/2 + "," + (alturaVis + margin.bottom - 2) + ") rotate(0)")
    .text("Horários");
}

d3.csv('../acude-velho.csv', function(dados) {
  desenhaPontos(dados);
});

</script>
