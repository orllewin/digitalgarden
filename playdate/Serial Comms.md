## Note. deprecated

Playdate now has [serialMessageReceived(msg)](https://sdk.play.date/2.5.0/Inside%20Playdate.html#c-serialMessageReceived) which makes this kind of thing much simpler.

## About

Playdate has an undocumented USB API with a serial interface. 

Here's some notes on creating a mechanism to control your Playdate creations from another device. To call code within your project you need to use the `eval` method, this requires sending Lua bytecode, but Playdate uses a custom Lua environment.

## Make the Lua Fork

Playdate runs a custom Lua environment, somebody has created a fork of Lua including those changes. In order to generate the Lua bytecode to send to your app/game you need to compile some code using this custom compiler.

* Clone the repo: [github.com/orllewin/lua54](https://github.com/orllewin/lua54) which is a fork of [github.com/scratchminer/lua54](https://github.com/scratchminer/lua54) - they're identical, use either.
* Open a terminal and cd to the repo, type `make`
* If successful the directory will now contain newly compiled `lua` and `luac` binaries.

## Create your own API

You app/game needs some kind of hook, create a function in your Playdate project to call over USB. For my own project I want to control a drum machine, I want to send which column and row indexes the user clicked to the Playdate, so in my project I've added a `serialTap(c, r)` function to `main.lua` (column and row indexes in a grid). Once that's ready install on a Playdate - **make sure to turn the simulator off after install** - you won't be able to connect to the Playdate from whatever you implement a serial client in if the simulator is running.

## Compile some Lua bytecode

With your freshly minted custom Lua compiler generate some bytecode, I created a `main.lua` file in the same directory as the new `lua` and `luac` binaries with nothing more than `serialTap(1,1)` in:

* `echo "serialTap(1,1)" > main.lua`
* `./luac -o serial1x1.luac main.lua`

I repeated this process 16 times for each of my 4x4 cells - though from inspecting the bytecode in a hex editor it should be easy enough to just edit a single byte array.

## Use eval to send the bytecode

From whatever environment you're coding in import the bytecode to a byte array ready to send over the serial interface, pseudo code:

```
serialPort = Serial("/dev/tty.usbmodemPDXX_XXXXXXXXXX", 115200);
bytes = loadBytes("serial1x1.luac")
serialPort.write("eval " + bytes.length + "\n")
serialPort.write(bytes)
```

You'll need the address of your Playdate, it'll look something like `/dev/tty.usbmodemPDXX_X1234567`

## Processing Example

Proof-of-concept client written in [Processing](https://processing.org):

```java
import processing.serial.*;

ArrayList<byte[]> byteCodes = new ArrayList<byte[]>();

int w = 480;
int h = 480;
int columns = 4;
int rows = 4;
int columnWidth = w/columns;
int rowHeight = h/rows;

Serial serialPort = null;

boolean serialActive = false;
ArrayList<Pad> pads = new ArrayList<Pad>();

void setup() {
  size(480, 480);
  printArray(Serial.list());

  byteCodes.add(loadBytes("serial1x1.luac"));//1
  byteCodes.add(loadBytes("serial2x1.luac"));//2
  byteCodes.add(loadBytes("serial3x1.luac"));//3
  byteCodes.add(loadBytes("serial4x1.luac"));//4
  
  byteCodes.add(loadBytes("serial1x2.luac"));//q
  byteCodes.add(loadBytes("serial2x2.luac"));//w
  byteCodes.add(loadBytes("serial3x2.luac"));//e
  byteCodes.add(loadBytes("serial4x2.luac"));//r
  
  byteCodes.add(loadBytes("serial1x3.luac"));//a
  byteCodes.add(loadBytes("serial2x3.luac"));//s
  byteCodes.add(loadBytes("serial3x3.luac"));//d
  byteCodes.add(loadBytes("serial4x3.luac"));//f
  
  byteCodes.add(loadBytes("serial1x4.luac"));//z
  byteCodes.add(loadBytes("serial2x4.luac"));//x
  byteCodes.add(loadBytes("serial3x4.luac"));//c
  byteCodes.add(loadBytes("serial4x4.luac"));//v

  for(int r = 0; r < rows; r++){
    for(int c = 0; c < columns; c++){
      pads.add(new Pad(c, r, c * columnWidth, r * rowHeight, columnWidth, rowHeight));
    }
  }
  
  noStroke();
  fill(255);
  stroke(0);
}

void draw() {
  background(0);
  
  strokeWeight(2);
  
  for(int i = 0 ; i < pads.size() ; i++){
    Pad pad = pads.get(i);
    pad.update().draw();
  }
  
  if(serialActive){
    while (serialPort.available() > 0) {
      String inBuffer = serialPort.readString();   
      if (inBuffer != null) {
        println(">>>>> " + inBuffer);
      }
    }
  }
}

void mousePressed() {
  Pad pad = find(mouseX, mouseY);
  if(pad != null){
    println("Clicked pad: " + pad.id());
    pad.click();
  }else{
    //Clicked some other area of the window
  }
}

void sendByteCode(int index){
  if(serialActive){
    print("sendByteCode(): " + index);
    byte[] byteCode = byteCodes.get(index);
    serialPort.write("eval " + byteCode.length + "\n");
    serialPort.write(byteCode);
  }
}

void keyPressed() {
  if (key == '1') {
    pads.get(0).click();
    sendByteCode(0);
  }else if(key == '2'){
    pads.get(1).click();
    sendByteCode(1);
  }else if(key == '3'){
    pads.get(2).click();
    sendByteCode(2);
  }else if(key == '4'){
    pads.get(3).click();
    sendByteCode(3);
  }else if(key == 'q'){
    pads.get(4).click();
    sendByteCode(4);
  }else if(key == 'w'){
    pads.get(5).click();
    sendByteCode(5);
  }else if(key == 'e'){
    pads.get(6).click();
    sendByteCode(6);
  }else if(key == 'r'){
    pads.get(7).click();
    sendByteCode(7);
  }else if(key == 'a'){
    pads.get(8).click();
    sendByteCode(8);
  }else if(key == 's'){
    pads.get(9).click();
    sendByteCode(9);
  }else if(key == 'd'){
    pads.get(10).click();
    sendByteCode(10);
  }else if(key == 'f'){
    pads.get(11).click();
    sendByteCode(11);
  }else if(key == 'z'){
    pads.get(12).click();
    sendByteCode(12);
  }else if(key == 'x'){
    pads.get(13).click();
    sendByteCode(13);
  }else if(key == 'c'){
    pads.get(14).click();
    sendByteCode(14);
  }else if(key == 'v'){
    pads.get(15).click();
    sendByteCode(15);
  }else if(key == 'o'){
    println("Starting serial connection...");
    serialPort = new Serial(this, "/dev/tty.usbmodemPDXX_XXXXX", 115200);
    serialActive = true;
  }else if(key == 'p'){
    println("Closing serial connection.");
    serialPort.clear();
    serialPort.stop();
    serialPort = null;
    serialActive = false;
  }
}

Pad find(int x, int y){
  
  for(int i = 0 ; i < pads.size() ; i++){
    Pad pad = pads.get(i);
    if(pad.hit(x, y)){
      return pad;
    }
  }
  
  return null;
}

class Pad{
  int column, row, pX, pY, pWidth, pHeight;
  
  int g = 255;
  
  public Pad(int column, int row, int pX, int pY, int pWidth, int pHeight){
    this.column = column;
    this.row = row;
    this.pX = pX;
    this.pY = pY;
    this.pWidth = pWidth;
    this.pHeight = pHeight;
  }
  
  String id(){
    return "" + column + "x" + row;
  }
  
  boolean hit(int x, int y){
    return x > pX && x < pX + pWidth && y > pY && y < pY + pHeight;
  }
  
  void click(){
    g = 0;
  }
  
  Pad update(){
    if(g < 255) {
      g += 15;
    }
    return this;
  }
  
  void draw(){
    noStroke();
    fill(g);
    rect(pX, pY, pWidth, pHeight, 12);
  }
}
```