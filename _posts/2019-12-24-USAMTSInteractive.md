---
title: USAMTS Competition Problem Explorable
smartdown: true
---

The USA Mathematical Talent Search creates fabulous problem sets.

**4/1/30**. Right triangle $\triangle ABC$ has $\angle C = 90^{\circ}$. A fly is trapped inside $\triangle ABC$. It starts at point $D$, the foot of the altitude from $C$ to $\overline{AB}$, and then makes a (finite) sequence of moves. In each move, it flies in a direction parallel to either $\overline{AC}$ or $\overline{BC}$; upon reaching a leg of the triangle, it then flies to a point on $\overline{AB}$ in a direction parallel to $\overline{CD}$. For example, on its first move, the fly can move to either of the points $Y_1$ or $Y_2$, as shown.
Let $P$ and $Q$ be distinct points on $\overline{AB}$. Show that the fly can reach some point on $\overline{PQ}$.

[P](:-P/0/1/0.01)
[Q](:-Q/0/1/0.01)
[:arrow_left:](:=left=true) [:arrow_down:](:=down=true) [reset](:=reset=true)
```p5js /playable/autoplay

// this code is html code to make the app size and background
const myDiv = this.div;
myDiv.style.width = '100%';
myDiv.style.height = '100%';
myDiv.style.margin = 'auto';
myDiv.style.background = '#DDDDDD';
myDiv.style.borderRadius = '8px';


// this is a bunch of variables for the size of the app window
// it needs to change when the user resizes the web page
const widthScale = 0.5;
const heightScale = 0.5;
let canvasWidth = p5.windowWidth * widthScale;  // set the size of the playable
let canvasHeight = p5.windowWidth * heightScale;
let xMargin = canvasWidth * 0.1;   // margins for the main triangle
let yMargin = xMargin;
let unit = canvasWidth - 2 * xMargin;  // how wide should the triangle be


// we keep track of the number of clicks so we can 
// 1. set the green target interval
// 2. set the position of first point
let numClicks = 0;  
let pink = [240,0,200];


// our X and Y points are between 0 and 1.  We need to convert them to 'pixels' to scale
// them to the size of the app window.
let getX = function(x) {
  return x * unit + xMargin;
};

let getY = function(y) {
  return canvasHeight - yMargin - y * unit;
};


// these are constants that define the triangle
let a = 1; // x intercept. this should always be 1.  it's the width of the triangle
let c = 1; // y intercept.  it's the height of triangle.  must be between 0 and 1.
let m =  - c / a; // slope of the hypotenuse
let orthoM = - 1 / m;  // slope that is 90 degrees to hypotenuse
let f = function(x) { return m * x + c; }; // returns a point on the hypotenuse


// plug in a point and it will return a point that is on the hypotenuse and also on
// an altitude through the given point.
let altPoint = function(x,y) { 
  const b = y + x / m;
  const newX = m * (b - c) / (m*m + 1);
  const newY = f(newX);
  return [newX , newY];
};

// this is a starting point on the hypotenuse
let [startX, startY] = altPoint(0,0);
let X = startX;
let Y = startY;

// find the X coordinate (relative to triangle) from a mouse position (relative to app window)
let getXfromMousePosition = function(p) {
  if (p < xMargin) return 0;
  if (p > canvasWidth - xMargin) return a;
  return (p - xMargin) / unit;
};

let drawTriangle = function() {
  p5.clear();
  p5.push();

  p5.fill('white');
  p5.stroke('black');
  p5.strokeWeight(2);
  // draw the main triangle
  p5.triangle(getX(0), getY(0), getX(a), getY(0), getX(0), getY(c)); 
  
  // draw the triangle altitude
  const [aX,aY] = altPoint(0,0);
  p5.stroke(0,0,200);
  p5.line(getX(0), getY(0), getX(aX), getY(aY));

  // // draw the interval PQ
  p5.stroke(100,255,100);
  p5.line(getX(env.P),getY(f(env.P)), getX(env.Q), getY(f(env.Q)));

  p5.fill('black');
  p5.stroke('black');
  p5.strokeWeight(1);
  p5.text('P', getX(env.P), getY(f(env.P)) - 10);
  p5.text('Q', getX(env.Q), getY(f(env.Q)) - 10);
  p5.ellipse(getX(env.P), getY(f(env.P)), 5);
  p5.ellipse(getX(env.Q), getY(f(env.Q)), 5);

  p5.text('A', getX(0), getY(c) - 10);
  p5.text('B', getX(a), getY(0) + 15);
  p5.text('C', getX(0), getY(0) + 15);
  
  p5.circle(getX(0), getY(c), 5);
  p5.circle(getX(a), getY(0), 5);
  p5.circle(getX(0), getY(0), 5);
            
  p5.pop();
};

p5.setup = function() {                          // this function is called when you start the

  p5.createCanvas(canvasWidth,canvasHeight);     // create the canvas we will draw on
  p5.windowResized();  
  drawTriangle(); 
  p5.noLoop();                                  
};


p5.windowResized = function() {                  // this function is called when the user changes
  canvasWidth = p5.windowWidth * widthScale;  // the size of the window.  It will rescale all the
  canvasHeight = p5.windowWidth * heightScale;    // components to fit into the new window size.

  p5.resizeCanvas(canvasWidth, canvasHeight);
}


p5.draw = function() {                           // this function gets called repeatedly in a loop.

}



let makeMove = function( moveX, moveY ) 
{
  p5.push()
  p5.fill(pink);
  p5.noStroke();
  p5.ellipse(getX(X), getY(Y), 5);  // draw the point
  p5.stroke(pink)
  p5.strokeWeight(1);

  p5.line(getX(X),getY(Y),getX(moveX),getY(moveY));  // draw the line to axis
  const [xP, yP] = altPoint(moveX,moveY);
  p5.line(getX(moveX),getY(moveY), getX(xP), getY(yP));  // draw the next altitude line
  X = xP;                                                // udate new value of x and y
  Y = yP;
  p5.pop();
}


smartdown.setVariable('P', 0.1);
smartdown.setVariable('Q', 0.5);
smartdown.setVariable('left', false);
smartdown.setVariable('down', false);
smartdown.setVariable('reset', false);

let prevP = 0.1;
let prevQ = 0.5;
this.dependOn = ['P', 'Q', 'left', 'down', 'reset'];
this.depend = function() {
  if (env.left == true) {
    smartdown.setVariable('left', false);
    makeMove(0, Y);
  }
  if (env.down == true) {
      smartdown.setVariable('down', false);
      makeMove(X, 0);
  }
  if (prevP != env.P || prevQ != env.Q || env.reset == true) {
      smartdown.setVariable('reset', false);
      drawTriangle();
      prevP = env.P;
      prevQ = env.Q;
      const [rx, ry] = altPoint(0,0);
      X = rx;
      Y = ry;
  }
};

```