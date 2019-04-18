---
title: "A tale of ticking clocks"
date: <span class="timestamp-wrapper"><span class="timestamp">&lt;2019-04-19 Fri&gt;</span></span>
layout: post
categories:
tags:
---

# Table of Contents


Here&rsquo;s a javascript abstraction to generate ticking clocks. Such clocks can be
useful for generating interactive and non-interactive visualizations at several
speeds.

The clock listens to the passage of time and triggers a callback function.

The clock scheduler takes in two arguments and either executes a callback
function, or returns a function that gets executed in context. The two arguments
to the clock scheduler are:

1.  rate at which the clock wrt to the 1 seconds /number of times that the function is called in 1 second,
2.  callback function

*The standard clock function as 1 second*
*Lower number clocks are called before the higher number clocks*

The following is a code snippet for executing 5 clocks at different time rates.


    // Tale of ticking clocks:

    function js_clock ( r, cb ){

        var then = Date.now();
        var rate = r;
        var interval = r*1000;
        var lookahead = 20; //as low 1 frame
        var counter = 0;
        return function(now){
            var delta = now - then;
            if( delta > interval - lookahead &&  delta < interval){ ///just in time scheduling
                then = then + interval;
                counter++;
                cb(rate, counter);
            }
        };
    }


    function drawLoop(){
        var now = Date.now()
        clock1(now);
        clock2(now);
        clock3(now);
        clock4(now);
        clock5(now);
    }


    function printcount(n ,counter){console.log("hello world " +n + " " + counter)};

    var clock5 = js_clock(2, printcount);
    var clock4 = js_clock(1, printcount);
    var clock3 = js_clock(0.5, printcount);
    var clock2 = js_clock(0.125, printcount);
    var clock1 = js_clock(1/16, printcount);

    var rafId = setInterval(drawLoop,1000/60);
