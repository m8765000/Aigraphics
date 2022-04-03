# Aigraphics

#P5JS 3D Terrain
#add 1. Cameras 2. Lights 3. Metarials

let sliderGroup = [];
let X;
let Y;
let Z;
let centerX;
let centerY;
let centerZ;
let H = 20;

var cols, rows;
var scl = 20;
var w = 1400;
var h = 1000;

var flying = 0;

var terrain = [];

let stars = [];
let i;




var img;
var gif;


function setup() {
  createCanvas(600, 600, WEBGL);

   for (var k = 0; k < 3; k++) {
    if (k=== 2) {
      sliderGroup[k] = createSlider(10, 400, 200);
    } else {
      sliderGroup[k] = createSlider(-400, 400, 0);
    }
    H = map(k, 0, 6, 5, 85);
    sliderGroup[k].position(10, height + H);
    sliderGroup[k].style('width', '80px');
   }
  img = loadImage('data:image/jpeg;base64,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');
  noStroke();
  cols = w / scl;
  rows = h / scl;

  for (var x = 0; x < cols; x++) {
    terrain[x] = [];
    for (var y = 0; y < rows; y++) {
      terrain[x][y] = 0; //specify a default value for now
    }

}
}
  

function draw() {
  background(0);
  
  let locX = mouseX - height / 2;
  let locY = mouseY - width / 2;
  

  
      let s = new Star(random (-400,width), random (-400,height/2), random (1,5), random (1,1), 5, 250, 250, 200);
  for (i = 0; i < random(3); i+=1) {
  stars.push(s);}
  for (let star of stars) {
  star.moveStar();
  star.createStar();
  }

  flying -= 0.1;
  var yoff = flying;
  for (var y = 0; y < rows; y++) {
    var xoff = 0;
    for (var x = 0; x < cols; x++) {
      terrain[x][y] = map(noise(xoff, yoff), 0, 1, -25, 0.01);
      xoff += 1;
    }
    yoff += 1;
  }

  translate(0, 45);
  rotateX(PI / 2);
  fill(200, 200, 200, 150);
  translate(-w / 2, -h / 2);
  
  
  for (var y = 0; y < rows - 1; y++) {
    beginShape(TRIANGLE_STRIP);
    fill(0, 20, 60);
    for (var x = 0; x < cols; x++) {
      vertex(x * scl, y * scl, terrain[x][y]);
      vertex(x * scl, (y + 1) * scl, terrain[x][y + 1]);
    }
    fill(100, 110, 200);
    for (var x = 0; x < cols; x++) {
      vertex(x * scl, y * scl, terrain[x][y]);
    }
        fill(240, 240, 175);
    for (var x = 10; x < cols; x++) {
      vertex(250, y* scl, terrain[x][y]);
    }

    endShape();
  }


      push();
  texture(img);
      X = sliderGroup[0].value();
  Y = sliderGroup[1].value();
  Z = sliderGroup[2].value();

  
  camera(X, Y, Z,  -50,  -30,  +30, 10, 30,0 );
        rotateY(millis() / 2500);
  rotateX(millis() / 2000);
  rotateZ(millis() / 2500);
    box(50);
  

  
  pop();
  
  
push();
  
    ambientLight(150, 150, 150);
  pointLight(100, 100, 0, locX, locY, 100);
   directionalLight(240, 240, 175, 0.25, 0.25, 0);
  translate(700, 100, 0);
  ambientMaterial(240, 240, 175);

  sphere(100);
  pop();

}

class Star {
  constructor(_x,_y,_r1,_r2,_npoints,_colr,_colg,_colb) {
    this.x = _x;
    this.y = _y;
    this.r1 = _r1;
    this.r2 = _r2;
    this.npoints = _npoints;
    this.colr = _colr;
    this.colg = _colg;
    this.colb = _colb;
    
    this.sx = 0;
    this.sy = 0;
    this.angle = TWO_PI / _npoints;
    this.halfAngle = TWO_PI / _npoints / 2;
    
  }

  createStar() {
    noStroke();
    fill(this.colr,this.colg,this.colb);
    beginShape();
    
    for (let j = 0; j < TWO_PI; j += this.angle) {
      this.sx = this.x + cos(j) * this.r2;
      this.sy = this.y + sin(j) * this.r2;
      vertex(this.sx, this.sy);
      this.sx = this.x + cos(j+this.halfAngle) * this.r1;
      this.sy = this.y + sin(j+this.halfAngle) * this.r1;
      vertex(this.sx, this.sy);
      }
    endShape(CLOSE);
  }
  
  moveStar() {
    this.y += pow(this.r1, 0.1);
    
    if (this.y > height) {
      let index = stars.indexOf(this);
      stars.splice(index, 1);
    }
  } 
} 
