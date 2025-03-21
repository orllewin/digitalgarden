#### Note. this requires an unpublished version of Modular Play, this message will be removed after a supported version is launched.

Playdate have added a convenient serial input API so my previous (pleasingly complex) methods are redundant:

```
playdate.serialMessageReceived(message)

Called when a `msg <text>` command is received on the serial port. The text following the command is passed to the function as the string _message_.

Running `!msg <message>` in the simulator Lua console sends the command to the device if one is connected, otherwise it sends it to the game running in the simulator.
```

Instead of compiling Lua bytecode with a custom compiler and calling an undocumented `eval(bytecode)` serial interface method you can just call `msg somePayload`. I've left the previous methods in place so any older implementation will still work. 

For ModularPlay I'm using `c` and `m` and `b` prefixes followed by a number:
* `c` is for clock and should be followed by a number, 1 to 16.
* `m` is for midi-note and should be followed by a valid midi note number. 
* `b` is for bang, and should be followed by a number

Examples:

`c1` - send the first beat of a bar.
`m112` - send midi note number 112.
`b3` - send a bang event to a bang module with id 3.

## Updated Processing Project

My latest Playdate setup consists of two Playdates connected to a Pi Zero with a 4x USB-A hat. The Pi Zero runs Processing, but there's very little processing power for high frame-rate animations. The HDMI output is connected to a small LED screen with a touchscreen (720x576 pixels).

```
import processing.serial.*;

int bpm = 110;
int minBpm = 40;
int maxBpm = 220;

boolean running = true;
int delayMs = (int)(60000/bpm)/4;
int beat = 1;



ArrayList<Serial> playdates = new ArrayList<Serial>();

int screenWidth = 720;
int screenHeight = 576;
int gridWidth = screenWidth/2 - 10;
int gridHeight = screenHeight - 90;
int gridColumns = 3;
int gridRows = 4;
int cellWidth = gridWidth/gridColumns;
int cellHeight = gridHeight/gridRows;
int gridStartX = screenWidth/2;
int gridStartY = 10;

float controlX = map(bpm, minBpm, maxBpm, 0, screenWidth);

void setup(){
  size(720, 576);

  surface.setTitle("Playdate Clock");
  
  printArray(Serial.list());
  
  String[] serialDevices = Serial.list();
  int deviceCount = serialDevices.length;
  for(int i = 0; i < deviceCount ; i++){
    String device = serialDevices[i];
    println("Serial device: " + device);
    if(device.contains("cu.usbmodemPD")){
      println("Found a Playdate at " + device);
      playdates.add(new Serial(this, device, 115200));
    }
  }
  
  thread("clockThread");
}

void draw() {
  background(0);
  strokeWeight(2);
  fill(255);
  
  //----------------------------------------------
  //Play/Pause button:
  if(running){
    rect(20, 230, 200, 200);
  }else{
    triangle(20, 230, 220, 330, 20, 430);
  }
  
  //----------------------------------------------
  //BPM control footer:
  noStroke();
  fill(255);
  rect(0, height - 70, width, height);
  
  stroke(0);
  line(15, height - 35, width - 15, height - 35);

  fill(0);
  noStroke();
  rect(controlX, height - 60, 20, 50);
  
  //----------------------------------------------
  //Labels:
  textSize(80);
  fill(255);
  text(bpm + "BPM", 10, 90);
  
  if(running){
    text("Playing", 10, 180);
  }else{
    text("Paused", 10, 180);
  }
  
  //----------------------------------------------
  //Bang grid:
  textSize(60);
  int cellNumber = 1;
  for(int r = 0; r < gridRows ; r++){
    for(int c = 0 ; c < gridColumns ; c++){
      
      fill(255);
      strokeWeight(8);
      stroke(0);
      rect(gridStartX + (c * cellWidth), gridStartY + (r * cellHeight), cellWidth, cellHeight, 20);
      
      fill(0);
      text("" + cellNumber, gridStartX + (c * cellWidth) + (10), gridStartY + (r * cellHeight) + (cellHeight/2));
      
      cellNumber++;
    }
  }
  
  noLoop();
}

void mousePressed(){
  if(mouseX > gridStartX && mouseY < height - 70){
    //grid matrix
    int clickedRow = (mouseY - gridStartY) / cellHeight;
    int clickedColumn = (mouseX - gridStartX ) / cellWidth;
    int cellIndex = (clickedRow*gridColumns+clickedColumn) + 1;//+1 for Lua, Lua is not zero-indexed
    sendMessage("b" + cellIndex);
  }else if(mouseX > 20 && mouseY > 230 && mouseX < 220 && mouseY < 430){
    //running toggle
    
    running = !running;
    
    //Restarted after a pause - reset to beat 1 
    if(running) beat = 1;
  }else if(mouseY > height - 70){
    bpm = (int)map(mouseX, 0, width, minBpm, maxBpm);
    if(bpm % 2 != 0) bpm += 1;
    delayMs = (int)(60000/bpm)/4;
    controlX = map(bpm, minBpm, maxBpm, 20, width - 30);
  }
  
  //Redraw
  loop();
}

void sendMessage(String msg){
for(int i=0 ; i < playdates.size() ; i++){
      if(running){
        Serial playdate = playdates.get(i);
        playdate.write("msg " + msg);
      }
    }
}

void clockThread() {
  for (;; delay(delayMs)){
    if(beat == 16) beat = 0;
    sendMessage("c" + beat);
    beat++;
  }
}
```