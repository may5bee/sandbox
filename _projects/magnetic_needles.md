---
layout: full
title: "Magnetic Needles"
description: ""
date: 2014-7-14 09:37:43 +0800
technologies:
category: physical
tags:
permalink: "/field"
description: Here we simulating a magnetic field, the cursor is the magnet. Click to shuffle.
---

<canvas class="w-100 h-100" resize="true" id="myCanvas" style="background:rgb(241,145, 146)"></canvas>

<script type="text/javascript" src="{{site.baseurl}}/js/paper-full.js"></script>
<script type="text/paperscript" canvas="myCanvas">

var activeConfig = 1;
var totalConfigs = 100;

var nav = 0;
var numHor;
var numVert;
var from = new Point(0,0);
var to;
var count;
var thickness = 40;
var pink = '#EB005A'
var blue = '#009dec'

var yellow = '#FFFF00'
generate();

function generate(){
    if (activeConfig==1){
        thickness = 4;
        numHor = 20;
        numVert = 10;
        to = [20, 20];
        color = blue;

    }
    else if (activeConfig>1){
        thickness = Math.floor((Math.random() * 26))
        numHor = Math.floor((Math.random() * 20) + 10);
        numVert = Math.floor((Math.random() * 15) + 5);

        var bracket = Math.floor(Math.random*3)

        if (bracket > 2){
        var x = Math.floor((Math.random() * 800));
        var y = Math.floor((Math.random() * 1000));
        } else if (bracket > 1){
        var x = Math.floor((Math.random() * 400));
        var y = Math.floor((Math.random() * 300));
        } else {
        var x = Math.floor((Math.random() * 100)+2);
        var y = Math.floor((Math.random() * 100)+2);
        }

        to = (x, y);
        var redRandom = Math.random()
        var blueRandom = Math.random()
        var greenRandom = Math.random()
        color = new Color(redRandom, greenRandom, blueRandom);
    }
    else if (activeConfig==3){
        thickness = 1;
        numHor = 10;
        numVert = 10;
        to = (0, 3000);
        color = blue;
    }
    else if (activeConfig==3){
        thickness = 35;
        numHor = 20;
        numVert = 10;
        to = (75, 75);
        color = blue;
    }
    else if (activeConfig==5){
        thickness = 1;
        numHor = 20;
        numVert = 10;
        to = (20, 600);
        color = pink;
    }
    else if (activeConfig==6){
        thickness = Math.floor((Math.random() * 30) + 1)
        numHor = Math.floor((Math.random() * 10) + 10);
        numVert = Math.floor((Math.random() * 12) + 5);
        var x = Math.floor((Math.random() * 80) + 20);
        var y = Math.floor((Math.random() * 40) + 20);
        to = (x, y);
        color = blue;
    }


    count = numHor * numVert;
    var line = new Path.Line(from, to);

    line.style = {strokeColor : color, strokeWidth : thickness, strokeCap : 'round'};
    var symbol = new Symbol(line);
    for (var i = 0; i< numHor; i++){
        for (var j = 0; j< numVert; j++){
           var instance = symbol.place();
             instance.position.x = (view.size.width-nav)/numHor * i + view.size.width/40 +nav;
             instance.position.y = view.size.height/numVert * j + view.size.height/20;
        }
    }
}

function onMouseMove(event){
    for (var i = 0; i < count; i++) {
        var item = project.activeLayer.children[i];
        var vector = event.point - item.position;
        var prevVector = event.lastPoint - item.position;
        item.rotate(vector.angle-prevVector.angle, item.position);
    }
}

//kinda cool but not what I want
// function onResize(){
//     for (var i = 0; i < count; i++) {
//         var item = project.activeLayer.children[i];
//         item.position.y = view.size.width/numVert * i/numHor + 30
//     }
// }

function onMouseDown(event) {
    activeConfig++;
    if (activeConfig>totalConfigs){
        activeConfig = 1;
    }
    project.activeLayer.removeChildren();
    generate();
}

function onResize(event){
   project.activeLayer.removeChildren();
   generate();
}


</script>
