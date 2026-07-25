# Processing Java API Reference

All categories from https://processing.org/reference. Processing 4.5.2 Java Mode.

---

## Structure

### setup() / draw() lifecycle

```java
void setup() {
  size(800, 600);            // must be first line
  // fullScreen() or fullScreen(P3D)
  frameRate(60);
}

void draw() {
  background(0);
  // animation loop
}
```

Control: `noLoop()` stops draw looping, `loop()` resumes, `redraw()` runs once when looping stopped.

### size() / fullScreen()

```java
size(800, 600);              // default 2D
size(800, 600, P2D);         // OpenGL 2D
size(800, 600, P3D);         // OpenGL 3D
size(800, 600, FX2D);        // JavaFX (needs library)
fullScreen();                // fill screen
fullScreen(P3D);
```

`width` and `height` set automatically.

### settings() (for variable size)

```java
void settings() {
  size(displayWidth / 2, displayHeight / 2);
}
void setup() {
  // size() not here
}
```

---

## Environment

`cursor(type)` — ARROW, CROSS, HAND, MOVE, TEXT, WAIT
`noCursor()` — hide cursor
`delay(millis)` — pause
`displayWidth`, `displayHeight` — screen dimensions
`frameCount` — frames since start
`frameRate(fps)` — set target framerate
`focused` — window has focus
`pixelWidth`, `pixelHeight` — Retina-aware resolution
`windowMove(x, y)` — move window

---

## Shape

### 2D Primitives

```java
point(x, y);
line(x1, y1, x2, y2);
triangle(x1, y1, x2, y2, x3, y3);
rect(x, y, w, h);
rect(x, y, w, h, tl, tr, br, bl);  // rounded corners
square(x, y, extent);
circle(x, y, extent);
ellipse(x, y, w, h);
arc(x, y, w, h, start, stop);
arc(x, y, w, h, start, stop, mode); // OPEN, CHORD, PIE
quad(x1, y1, x2, y2, x3, y3, x4, y4);
```

### Custom Shapes (beginShape / endShape)

```java
beginShape();
vertex(30, 20);
vertex(85, 20);
vertex(85, 75);
vertex(30, 75);
endShape(CLOSE);

// Contours (holes):
beginShape();
vertex(0, 0);   vertex(100, 0);
vertex(100, 100); vertex(0, 100);
beginContour();
vertex(25, 25); vertex(75, 25);
vertex(75, 75); vertex(25, 75);
endContour();
endShape(CLOSE);

// Bezier vertices:
beginShape();
vertex(30, 70);
bezierVertex(25, 25, 100, 10, 50, 50);
bezierVertex(20, 60, 30, 100, 60, 90);
endShape();

// Quadratic vertices:
beginShape();
vertex(30, 70);
quadraticVertex(25, 25, 50, 50);
quadraticVertex(75, 25, 90, 70);
endShape();
```

Vertex modes: `POINTS`, `LINES`, `TRIANGLES`, `TRIANGLE_FAN`, `TRIANGLE_STRIP`, `QUADS`, `QUAD_STRIP`.

### Curves

```java
bezier(x1, y1, cx1, cy1, cx2, cy2, x2, y2);
bezierDetail(resolution);
bezierPoint(a, b, c, d, t);
bezierTangent(a, b, c, d, t);
curve(cpx1, cpy1, x1, y1, x2, y2, cpx2, cpy2);
curveDetail(resolution);
curveTightness(t);
curvePoint(a, b, c, d, t);
curveTangent(a, b, c, d, t);
```

### Shape Attributes

```java
ellipseMode(CENTER);              // CENTER, RADIUS, CORNER, CORNERS
rectMode(CORNER);                 // CORNER, CORNERS, CENTER, RADIUS
strokeWeight(pixels);
strokeCap(ROUND);                 // ROUND, SQUARE, PROJECT
strokeJoin(MITER);                // MITER, BEVEL, ROUND
```

### 3D Primitives

```java
box(size);
box(w, h, d);
sphere(radius);
sphereDetail(res);                // sphere mesh resolution
sphereDetail(uRes, vRes);
```

### PShape (Advanced Shapes / SVG)

```java
PShape s;
void setup() {
  size(400, 400);
  s = loadShape("bot.svg");       // SVG from data/ folder
}
void draw() {
  shape(s, 40, 40, 320, 320);     // x, y, w, h
}
```

Create shapes programmatically:

```java
PShape square = createShape(RECT, 0, 0, 80, 80);

PShape group = createShape(GROUP);
group.addChild(createShape(RECT, 0, 0, 50, 50));
group.addChild(createShape(ELLIPSE, 60, 60, 30, 30));
shape(group, 0, 0);

// Custom vertex shape:
PShape s = createShape();
s.beginShape();
s.vertex(0, 0);  s.vertex(50, 0);
s.vertex(50, 50); s.vertex(0, 50);
s.endShape(CLOSE);
```

PShape methods: `translate()`, `rotate()`, `rotateX/Y/Z()`, `scale()`, `resetMatrix()`, `setFill()`, `setStroke()`, `setVisible()`, `disableStyle()`, `enableStyle()`, `getChild()`, `getChildCount()`, `addChild()`, `getVertexCount()`, `getVertex()`, `setVertex()`, `beginContour()`, `endContour()`.

`shapeMode(CENTER)` — CORNER, CORNERS, CENTER.

---

## Color

### Setting

```java
background(r, g, b);
background(gray);
background(r, g, b, alpha);
background(hex);                // background(#FF00CC)
background(image);
clear();                        // transparent
fill(r, g, b);
noFill();
stroke(r, g, b);
noStroke();
colorMode(RGB, 255);            // default
colorMode(HSB, 360, 100, 100, 100);
```

### Creating & Reading

```java
color c = color(255, 0, 0);
float r = red(c);  float g = green(c);
float b = blue(c); float a = alpha(c);
float h = hue(c);  float s = saturation(c);
float br = brightness(c);
color blended = lerpColor(c1, c2, 0.5);
```

---

## Image

### PImage

```java
PImage img = loadImage("photo.jpg");  // .gif, .jpg, .tga, .png
image(img, x, y);
image(img, x, y, w, h);
PImage img = createImage(w, h, RGB);  // RGB or ARGB
tint(r, g, b, alpha);
noTint();
imageMode(CENTER);                    // CORNER, CORNERS, CENTER
```

### Pixel Manipulation

```java
img.loadPixels();
color c = img.pixels[loc];
float r = red(c);
img.pixels[loc] = color(255, 0, 0);
img.updatePixels();
img.filter(GRAY);              // GRAY, INVERT, POSTERIZE, BLUR, ERODE, DILATE
img.filter(BLUR, 3);
img.resize(w, h);
img.get(x, y, w, h);           // returns PImage subset
img.set(x, y, color);
img.mask(maskImage);
img.blend(srcImg, sx, sy, sw, sh, dx, dy, dw, dh, mode);
img.copy(srcImg, sx, sy, sw, sh, dx, dy, dw, dh);
img.save("output.png");        // TIFF, TARGA, PNG, JPEG
```

### Textures (3D)

```java
PImage tex = loadImage("texture.jpg");
beginShape();
texture(tex);
vertex(x, y, z, u, v);
endShape();
textureMode(NORMAL);           // IMAGE or NORMAL
textureWrap(REPEAT);           // CLAMP or REPEAT
```

---

## Typography

```java
PFont font = createFont("Arial", 32);
textFont(font);
textSize(32);
text("Hello", x, y);
text("Word", x, y, w, h);       // bounding box
textAlign(LEFT, TOP);           // LEFT/CENTER/RIGHT, TOP/CENTER/BOTTOM/BASELINE
textLeading(spacing);
float tw = textWidth("hello");
float ta = textAscent();        // font metrics
float td = textDescent();
textMode(MODEL);                // MODEL or SHAPE
```

Load font file:

```java
PFont font = loadFont("MyFont-48.vlw");
textFont(font, 48);
```

---

## Math

### Calculation

```java
abs(n);  ceil(n);  floor(n);  round(n);
constrain(val, min, max);
dist(x1, y1, x2, y2);  dist(x1, y1, z1, x2, y2, z2);
exp(n);  log(n);  pow(base, exp);  sq(n);  sqrt(n);
lerp(start, stop, amt);
map(value, low1, high1, low2, high2);
norm(value, low, high);
mag(a, b);  mag(a, b, c);
max(a, b, c);  min(a, b, c);
```

### Trigonometry

```java
sin(a);  cos(a);  tan(a);
asin(v);  acos(v);  atan(v);  atan2(y, x);
degrees(rad);  radians(deg);
```

Constants: `PI`, `HALF_PI`, `QUARTER_PI`, `TWO_PI`, `TAU`

### Random

```java
random(high);                    // 0 to high (exclusive)
random(low, high);               // low to high (exclusive)
randomSeed(seed);
randomGaussian();
noise(x);                        // Perlin noise
noise(x, y);  noise(x, y, z);
noiseDetail(lod, falloff);
noiseSeed(seed);
```

---

## PVector

```java
PVector v1 = new PVector(40, 20);
PVector v2 = new PVector(25, 50, 0);

v1.x;  v1.y;  v1.z;
v1.set(x, y);
float m = v1.mag();
float ms = v1.magSq();           // squared (faster)
v1.normalize();
v1.limit(max);
v1.setMag(len);
float h = v1.heading();          // 2D angle
v1.setHeading(angle);
v1.rotate(angle);                // 2D only

v1.add(v2);  v1.sub(v2);
v1.mult(scalar);  v1.div(scalar);

// Static (return new PVector):
PVector.add(v1, v2);   PVector.sub(v1, v2);
PVector.mult(v1, 3);    PVector.div(v1, 3);

v1.dot(v2);
PVector.cross(v1, v2);            // static
v1.dist(v2);
v1.lerp(v2, 0.5);
PVector.angleBetween(v1, v2);     // static

PVector.random2D();
PVector.random3D();
PVector.fromAngle(angle);
PVector v3 = v1.copy();
float[] arr = v1.array();         // [x, y, z]
```

---

## Data

### Conversion

```java
int(x);  float(x);  boolean(x);  byte(x);  char(x);
str(x);
binary(n);  unbinary(s);
hex(n);  unhex(s);
```

### Arrays

```java
append(array, value);             // returns new array
arrayCopy(src, dst);
arrayCopy(src, srcPos, dst, dstPos, len);
concat(a, b);                     // returns new array
expand(array, newSize);           // returns new array
reverse(array);
shorten(array);                   // returns new array
sort(array);  sort(array, count);
splice(array, value, index);      // returns new array
subset(array, offset, len);       // returns new array
```

### Strings

```java
join(array, separator);
split(str, delimiter);
splitTokens(str, tokens);
trim(str);
match(str, regex);                // returns String[]
matchAll(str, regex);             // returns String[][]
```

### Number Formatting

```java
nf(num, digits);                  // "001"
nf(num, left, right);             // "00.00"
nfc(num);                         // with commas
nfc(num, right);
nfp(num);                         // "+01", "-01"
nfp(num, digits);
nfs(num, digits);                 // " 1"
nfs(num, left, right);
```

### Collections

```java
// ArrayList
ArrayList<PVector> pts = new ArrayList<PVector>();
pts.add(new PVector(10, 20));
pts.get(i);  pts.size();  pts.remove(i);

// HashMap
HashMap<String, Integer> map = new HashMap<String, Integer>();
map.put("key", 42);  map.get("key");

// Typed lists
IntList nums = new IntList();
nums.append(10);  nums.get(0);
FloatList, StringList — same API.

// Dicts
IntDict dict = new IntDict();
dict.set("health", 100);  dict.get("health");
FloatDict, StringDict — same API.
```

### Table (CSV/TSV)

```java
Table table = loadTable("data.csv", "header");
for (TableRow row : table.rows()) {
  String name = row.getString("name");
  int age = row.getInt("age");
  float score = row.getFloat("score");
}
// Access by index:
table.getRowCount();  table.getColumnCount();
TableRow r = table.getRow(0);
String val = r.getString(0);
```

### JSON

```java
JSONObject json = loadJSONObject("data.json");
String name = json.getString("name");
int age = json.getInt("age");
JSONArray items = json.getJSONArray("items");
for (int i = 0; i < items.size(); i++) {
  JSONObject item = items.getJSONObject(i);
}
// Parse from string:
JSONObject obj = parseJSONObject("{\"key\":42}");
JSONArray arr = parseJSONArray("[1,2,3]");
// Save:
saveJSONObject(json, "output.json");
saveJSONArray(arr, "output.json");
```

### XML

```java
XML xml = loadXML("data.xml");
XML child = xml.getChild("element");
String val = xml.getString("attribute");
XML[] children = xml.getChildren("item");
for (XML kid : children) {
  String id = kid.getString("id");
}
// Parse from string:
XML xml = parseXML("<root><child/></root>");
// Save:
saveXML(xml, "output.xml");
```

---

## Input

### Mouse

Variables: `mouseX`, `mouseY`, `pmouseX`, `pmouseY`, `mousePressed`, `mouseButton` (LEFT, RIGHT, CENTER)

```java
void mousePressed()  { }   // once per press
void mouseReleased() { }   // once per release
void mouseClicked()  { }   // press + release
void mouseDragged()  { }   // move while button pressed
void mouseMoved()    { }   // move without button pressed
void mouseWheel(MouseEvent e) {
  float delta = e.getCount();
}
```

### Keyboard

Variables: `key`, `keyCode` (UP, DOWN, LEFT, RIGHT, ALT, CONTROL, SHIFT, ENTER, TAB, etc.), `keyPressed`

```java
void keyPressed()  { }    // once per press
void keyReleased() { }    // once per release
void keyTyped()    { }    // characters only (no modifiers)
```

### Time & Date

```java
millis();              // ms since sketch started
second();  minute();  hour();
day();  month();  year();
```

### Files

```java
// Read
String[] lines = loadStrings("file.txt");
byte[] data = loadBytes("file.dat");
BufferedReader reader = createReader("file.txt");
String line = reader.readLine();
reader.close();

// Write
PrintWriter out = createWriter("output.txt");
out.println("hello");
out.flush();
out.close();
saveStrings("output.txt", new String[]{"a", "b"});
saveBytes("output.dat", data);
```

### File Dialogs

```java
selectInput("Choose:", "fileSelected");
selectOutput("Save:", "fileSaved");
selectFolder("Choose folder:", "folderSelected");

void fileSelected(File selection) {
  if (selection != null) {
    // selection.getAbsolutePath()
  }
}
```

### External Apps

```java
launch("/path/to/app");
launch("https://processing.org");
```

---

## Output

### Console

```java
print("text");
println("text");
printArray(array);
```

### Save Images/Frames

```java
save("screenshot.png");
saveFrame("frame-####.png");        // #### = frame number
saveFrame("frames/frame-####.tif");
```

### Recording

```java
beginRecord(PDF, "output.pdf");
// ... draw ...
endRecord();

beginRaw(P3D, "output.raw");
// ... draw ...
endRaw();
```

---

## Transform

```java
translate(x, y);
translate(x, y, z);              // 3D
rotate(angle);
rotateX(angle);                  // 3D
rotateY(angle);                  // 3D
rotateZ(angle);                  // 3D
scale(s);  scale(x, y);  scale(x, y, z);
shearX(angle);  shearY(angle);
```

### Matrix stack

```java
pushMatrix();
translate(50, 50);
rotate(PI/4);
rect(0, 0, 100, 100);
popMatrix();                     // restore

pushStyle();                     // style isolation
// ...
popStyle();
```

---

## Lights & Camera (3D)

### Lights

```java
ambientLight(r, g, b, x, y, z);
directionalLight(r, g, b, dx, dy, dz);
pointLight(r, g, b, x, y, z);
spotLight(r, g, b, x, y, z, dx, dy, dz, angle, conc);
lightFalloff(constant, linear, quadratic);
lightSpecular(r, g, b);
noLights();
```

### Camera

```java
camera(eyeX, eyeY, eyeZ, centerX, centerY, centerZ, upX, upY, upZ);
perspective(fov, aspect, near, far);
ortho(left, right, bottom, top, near, far);
frustum(left, right, bottom, top, near, far);
printCamera();
```

---

## Rendering

### Off-screen buffer

```java
PGraphics pg = createGraphics(400, 400, P2D);
pg.beginDraw();
pg.background(0);
pg.fill(255);
pg.rect(0, 0, 100, 100);
pg.endDraw();
image(pg, 0, 0);
```

### Shaders

```java
PShader shader = loadShader("frag.glsl", "vert.glsl");
shader(shader);
shader.set("time", millis() / 1000.0);
resetShader();
```

### Blend Modes

```java
blendMode(BLEND);                // BLEND, ADD, SUBTRACT, DARKEST,
                                 // LIGHTEST, DIFFERENCE, EXCLUSION,
                                 // MULTIPLY, SCREEN, REPLACE, REMOVE
```

### Hints

```java
hint(ENABLE_DEPTH_MASK);
hint(DISABLE_DEPTH_TEST);
hint(DISABLE_STROKE_PERSPECTIVE);
hint(ENABLE_STROKE_PURE);
```

### Clipping

```java
clip(x, y, w, h);
noClip();
```

---

## Control

Standard Java: `if`/`else`, `switch`, `for`, `while`, `do`/`while`.

Operators: `&&`, `||`, `!`, `==`, `!=`, `<`, `>`, `<=`, `>=`.

Arithmetic: `+`, `-`, `*`, `/`, `%`, `+=`, `-=`, `*=`, `/=`, `++`, `--`.

Bitwise: `&`, `|`, `<<`, `>>`.

---

## Common Patterns

### Basic animation sketch

```java
void setup() {
  size(800, 600);
}
void draw() {
  background(0);
  fill(255);
  ellipse(mouseX, mouseY, 50, 50);
}
```

### Static sketch (no animation)

```java
void setup() {
  size(800, 600);
  noLoop();
}
void draw() {
  background(255);
  fill(0);
  rect(100, 100, 200, 200);
}
```

### Event-driven sketch

```java
void setup() {
  size(800, 600);
}
void draw() { }     // empty but needed for events

void mousePressed() {
  fill(random(255), random(255), random(255));
  ellipse(mouseX, mouseY, 50, 50);
}

void keyPressed() {
  if (key == 's') saveFrame("screen-####.png");
}
```
