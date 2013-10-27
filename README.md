Project-2
=========
import TUIO.*;
TuioProcessing tuioClient;
import java.util.*; 

// these are some helper variables which are used
// to create scalable graphical feedback
float cursor_size = 15;
float object_size = 60;
float table_size = 760;
float scale_factor = 1;
PFont font;

int Count;
int signOff;

void setup()
{
  //size(screen.width,screen.height);
  size(1080, 720);
  noStroke();
  fill(0);

  loop();
  frameRate(30);
  //noLoop();


  font = createFont("Arial", 18);
  scale_factor = height/table_size;

  // we create an instance of the TuioProcessing client
  // since we add "this" class as an argument the TuioProcessing class expects
  // an implementation of the TUIO callback methods (see below)
  tuioClient  = new TuioProcessing(this);
}

// within the draw method we retrieve a Vector (List) of TuioObject and TuioCursor (polling)
// from the TuioProcessing client and then loop over both lists to draw the graphical feedback.
void draw()
{

  background(255);
  textFont(font, 16*scale_factor);
  float obj_size = object_size*scale_factor; 
  float cur_size = cursor_size*scale_factor; 

  Vector tuioObjectList = tuioClient.getTuioObjects();
  if (tuioObjectList.size()==0)
  {
    background(255);
    textAlign(CENTER);
    fill(0);
    text("Hi! I'm Kroma", 540, 320);
text(" How are you feeling today?", 540, 340);
 text("Please pick a card that best describes how you are feeling right now", 540, 360);
 text("Place the card in the slot in front of you", 540, 380);
 text("Thank You", 540, 400);
}

  println(Count);

  for (int i=0;i<tuioObjectList.size();i++) {
    TuioObject tobj = (TuioObject)tuioObjectList.elementAt(i);

    stroke(0);
    fill(0);

    pushMatrix();
    translate(tobj.getScreenX(width), tobj.getScreenY(height));
    rotate(tobj.getAngle());
    rect(-obj_size/2, -obj_size/2, obj_size, obj_size);
    popMatrix();
    fill(255);
    text(""+tobj.getSymbolID(), tobj.getScreenX(width), tobj.getScreenY(height));


    if (tobj.getSymbolID()==0)//happy
    {
      textAlign(CENTER);

      background(255, 125, 18);
      text("wonder", width/2, height/2);
    }
    if (tobj.getSymbolID()==1)//sad
    {
      textAlign(CENTER);

      background(24, 214, 15);
      text("wit", width/2, height/2);
    }
     if (tobj.getSymbolID()==2)//confused
    {
      textAlign(CENTER);

      background(27, 181, 193);
      text("human", width/2, height/2);
    }
       if (tobj.getSymbolID()==3)//anger
    {
      textAlign(CENTER);

      background(240, 15, 15);
      text("breathe", width/2, height/2);
    }
       if (tobj.getSymbolID()==4)//mental block
    {
      textAlign(CENTER);

      background(7, 86, 240);
      text("free", width/2, height/2);
    }
       if (tobj.getSymbolID()==5)//aggressive
    {
      textAlign(CENTER);

      background(255, 139, 217);
      text("bubble", width/2, height/2);
    }
  }

  Vector tuioCursorList = tuioClient.getTuioCursors();
  for (int i=0;i<tuioCursorList.size();i++) {
    TuioCursor tcur = (TuioCursor)tuioCursorList.elementAt(i);
    Vector pointList = tcur.getPath();

    if (pointList.size()>0) {
      stroke(0, 0, 255);
      TuioPoint start_point = (TuioPoint)pointList.firstElement();
      ;
      for (int j=0;j<pointList.size();j++) {
        TuioPoint end_point = (TuioPoint)pointList.elementAt(j);
        line(start_point.getScreenX(width), start_point.getScreenY(height), end_point.getScreenX(width), end_point.getScreenY(height));
        start_point = end_point;
      }

      stroke(192, 192, 192);
      fill(192, 192, 192);
      ellipse( tcur.getScreenX(width), tcur.getScreenY(height), cur_size, cur_size);
      fill(0);
      text(""+ tcur.getCursorID(), tcur.getScreenX(width)-5, tcur.getScreenY(height)+5);
    }
  }
}

// these callback methods are called whenever a TUIO event occurs

// called when an object is added to the scene
void addTuioObject(TuioObject tobj) {
  println("add object "+tobj.getSymbolID()+" ("+tobj.getSessionID()+") "+tobj.getX()+" "+tobj.getY()+" "+tobj.getAngle());
}

// called when an object is removed from the scene
void removeTuioObject(TuioObject tobj) {
  println("remove object "+tobj.getSymbolID()+" ("+tobj.getSessionID()+")");
}

// called when an object is moved
void updateTuioObject (TuioObject tobj) {
  println("update object "+tobj.getSymbolID()+" ("+tobj.getSessionID()+") "+tobj.getX()+" "+tobj.getY()+" "+tobj.getAngle()
    +" "+tobj.getMotionSpeed()+" "+tobj.getRotationSpeed()+" "+tobj.getMotionAccel()+" "+tobj.getRotationAccel());
}

// called when a cursor is added to the scene
void addTuioCursor(TuioCursor tcur) {
  println("add cursor "+tcur.getCursorID()+" ("+tcur.getSessionID()+ ") " +tcur.getX()+" "+tcur.getY());
}

// called when a cursor is moved
void updateTuioCursor (TuioCursor tcur) {
  println("update cursor "+tcur.getCursorID()+" ("+tcur.getSessionID()+ ") " +tcur.getX()+" "+tcur.getY()
    +" "+tcur.getMotionSpeed()+" "+tcur.getMotionAccel());
}

// called when a cursor is removed from the scene
void removeTuioCursor(TuioCursor tcur) {
  println("remove cursor "+tcur.getCursorID()+" ("+tcur.getSessionID()+")");
}

// called after each message bundle
// representing the end of an image frame



void refresh(TuioTime bundleTime) { 
  redraw();
}


