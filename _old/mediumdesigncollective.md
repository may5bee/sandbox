---
layout: full
title: Medium
description: ""
date: 2014-12-01 20:37:43 +0800
technologies:
category: web
tags:
description: Identity work for Medium Collective, an interdisciplinary design group at Cornell University.
---



<script type="text/javascript" src="{{site.baseurl}}/js/paper-full.min.js"></script>
<script src="https://d3js.org/d3.v3.min.js" charset="utf-8"></script>

<script type="text/paperscript" canvas="canvas">

  var count = 1000;
  var fillColors = ['rgba(255,0,0,1)','rgba(255,255,0,1)','rgba(0,0,255,1)']
  var colorScale = d3.scale.linear().domain([0,0.5,1]).range(['red','rgba(255,255,0,0.5)','rgba(255,0,0,0.0)']);
  console.log(colorScale(0));

  var circle = new Path.Circle(new Point(100, 100), 10);
  circle.fillColor = 'rgba(64,224,208,0.0)';

  for (var i = 0; i < count; i++) {
      var clonedCircle = circle.clone();
      var center = Point.random() * view.size;
      clonedCircle.position = center;

      var scale = (i + 1) / count;
      clonedCircle.scale(scale);

      clonedCircle.data.vector = new Point({
          angle: Math.random() * 360,
          length : scale * Math.random()
      });
      clonedCircle.fillColor = fillColors[i%3];
  }

  var vector = new Point({
      angle: 50,
      length: 0
  });

  var mouseVector = vector.clone();

  function onFrame(event) {
      vector = vector + (mouseVector - vector) / 30;
      // Run through the active layer's children list and change
      // the position of the placed symbols:
      for (var i = 0; i < count; i++) {
          var item = project.activeLayer.children[i];
          var size = item.bounds.size;
          var angle = Math.floor(Math.random * 360);
          var length = vector.length /30 * size.width ;
          item.position += vector.normalize(length) + item.data.vector;
          keepInView(item);

      }
  }

  function onMouseMove(event) {
      mousePos = event.point;
      project.activeLayer.selected = false;
  }

  function keepInView(item) {
      var position = item.position;
      var itemBounds = item.bounds;
      var bounds = view.bounds;
      if (itemBounds.left > bounds.width) {
          position.x = -item.bounds.width;
      }

      if (position.x < -itemBounds.width) {
          position.x = bounds.width + itemBounds.width;
      }

      if (itemBounds.top > view.size.height) {
          position.y = -itemBounds.height;
      }

      if (position.y < -itemBounds.height) {
          position.y = bounds.height  + itemBounds.height / 2;
      }
  }
  </script>

  <canvas id="canvas" resize hidpi="off" style="background:white"></canvas>
  <div class="medium">
    <h1>medium</h1>
    
</div>
