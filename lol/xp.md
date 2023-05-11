---
layout: default
title: "catchup xp"
description: ""
category: 
tags: []
---
{% include JB/setup %}
si sos el jungla y estás 2 levels atrás del level promedio del game, el jueguito te da más experiencia eso se llama catchup xp

bastante más

![](../../../assets/images/catchup.png)
<a href="https://www.leagueoflegends.com/en-pl/news/game-updates/patch-11-10-notes/" target="_blank">PATCH 11.10 NOTES</a>

quedarse atrás apropósito cuando sos jungla es algo que se podría hacer más seguido

es como que te caes en level pero caes en un trampolín y salís volando

además si te quedás atrás estás dejando que un man de tu equipo te robe los campos y se feedee

seguro que hay algún momento del game que yo sospecho que si no es en level 6-7 es en level 8 o 9

donde rinda dejar de farmear y quedarse un toque atrás para que tus carries saquen no sé level 11? su segundo item? más rápido

y después dentro de un rato comebackeás con la catchup xp

es una estrategia que para mi recontra existe pero es difícil de matematiquear en tu cabeza cuándo es worth, es medio un calculo para científicos

pero es worth muchas más veces de las que se hace, yo creo, entonces

le pedí al chat de bing que me haga una calculadora para visualizar cuando aplica la catchup xp

ponés los levels de las 10 personas del game y te dice qué level <a href="https://www.youtube.com/watch?v=8GAV8UUhTkM" target="_blank">(dos por debajo del promedio, redondeado para arriba)</a> que tendrías que ser vos para estar ganando mucha mas experiencia


<h1>Catchup Experience Calculator</h1>
<p>pone los levels de todos</p>
<form>
  <input type="number" id="form1" onchange="calculate()"><br>
  <input type="number" id="form2" onchange="calculate()"><br>
  <input type="number" id="form3" onchange="calculate()"><br>
  <input type="number" id="form4" onchange="calculate()"><br>
  <input type="number" id="form5" onchange="calculate()"><br>
  <input type="number" id="form6" onchange="calculate()"><br>
  <input type="number" id="form7" onchange="calculate()"><br>
  <input type="number" id="form8" onchange="calculate()"><br>
  <input type="number" id="form9" onchange="calculate()"><br>
  <input type="number" id="form10" onchange="calculate()"><br>

  <button style="color: #7253ed; background-color: #f5f6fa; border: 1px solid #7253ed;" onclick="calculate()">Calculate</button>
</form>
<p>level promedio (redondeado arriba) es: <span id="subresult">0</span></p>
<p>para sacar catchup xp en esta situacion tenes que ser level: <span id="result">0</span></p>





<script>
  function calculate() {

    var values = [];
    for (var i = 1; i <= 9; i++) {
      var value = document.getElementById("form" + i).value;

      value = Number(value);
      if (!isNaN(value)) {
        values.push(value);
      }
    }

    var sum = values.reduce((a, b) => a + b, 0);
    var average = Math.ceil(sum / values.length);

    document.getElementById("subresult").innerHTML = average;
    var result = average - 2;
    document.getElementById("result").innerHTML = result;
  }
</script>
