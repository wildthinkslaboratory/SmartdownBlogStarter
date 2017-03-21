---
title: Welcome to My Blog
smartdown: true
---


# card1
---

## Welcome To My Example Blog

This is the first post in an example blog. This blog is intended to be copied (forked) by a user to create their own blog, which will be hosted by GitHub Pages.

### Features

- [Smartdown](http://smartdown.site/?url=README.md)
- [Smartdown](http://smartdown.site/?url=README.md)

[Next](:@card2)


# card2
---

## Welcome To Card 2

I haven't blogged for a few years, and have been mostly building Smartdown as a blogging platform.

[Blog Features and Tech](:@raw/BlogFeaturesAndTech/)

---

[Previous](:@card1)
[Next](:@card3)


# card3
---


#### P5JS Mobius Example

This is a Mobius Strip constructed using 3D cylinders that are gradually rotated and translated to create a mobius strip skeleton.

```p5js
var PI = Math.PI;
var HALF_PI = PI / 2.0;
var SEGMENTS = env.SEGMENTS || 30;  // number of segments
var SEG_WIDTH = env.SEG_WIDTH || 8;
var SEG_LENGTH = env.SEG_LENGTH || 50;
var DIAMETER = SEG_LENGTH * 2;
var speed = 0.05;
var ax = .01;
var ay = ax;
var az = ay;
var dx, dy, dz;

p5.windowResized = function() {
  p5.resizeCanvas(p5.windowWidth - 70, p5.windowHeight - 100);
};

p5.setup = function() {
  dx = p5.random(-speed, speed);
  dy = p5.random(-speed, speed);
  dz = p5.random(-speed, speed);

  p5.createCanvas(300, 300, 'webgl');
  p5.normalMaterial();

  p5.windowResized();
};

p5.draw = function() {
  p5.camera(0, SEG_LENGTH, SEG_LENGTH, 0, 0, 0, 0, 1, 0);
  p5.rotateX(ax += dx);
  p5.rotateY(ay += dy);
  p5.rotateZ(az += dz);

  for (var i = 0; i < SEGMENTS; i++) {
    var frac = i * 2 / SEGMENTS;
    p5.push();
    p5.rotateX(frac * HALF_PI);
    p5.rotateY(HALF_PI);
    p5.translate(
        0,
        DIAMETER * p5.cos(frac * HALF_PI),
        DIAMETER * p5.sin(frac * PI));
    p5.cylinder(SEG_WIDTH, SEG_LENGTH);
    p5.pop();
  }
};
```


[Back to Start](:@card1)
