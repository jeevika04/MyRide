// Example code from Chapter 16.5 from textbook (chapter.16.7.cpp)
// with additional annotations.
// This code constructs a GUI that interacts with the user through
// buttons, boxes, and menus to draw an open polyline defined by
// points given by the user. 

#include <iostream>    // for i/o
#include <sstream>     // for string streams
#include "Graph.h"     // next 3 are for graphics library
#include "GUI.h"
#include "Window.h"

using namespace Graph_lib;
using namespace std;

//----------------------------------------------------------
// define a struct that is a window in which lines can be
// entered via a GUI

struct Lines_window : Graph_lib::Window {       // inherits from Window

  // constructor
  Lines_window(Point xy,             // top lefthand corner
	       int w,                // width
	       int h,                // height
	       const string& title); // label

private:
  // data members
  Open_polyline lines;               // shape to hold the lines themselves
  
  // widgets:
  Button next_button;                // button indicating next point is ready
  Button quit_button;                // end program
  In_box next_x;                     // box for entering x coord of next point
  In_box next_y;                     // box for entering y coord of next point
  Out_box xy_out;                    // box for displaying last point entered
  Menu color_menu;                   // menu of color choices for the lines
  Button menu_button;                // button to display the color menu

  // function members

  void change(Color c) {             // change the color of the lines
    lines.set_color(c);
  }

  void hide_menu() {     
    // hides the color menu and shows the button to display the color menu
    color_menu.hide(); 
    menu_button.show(); 
  }

  // actions invoked by callbacks:

  void red_pressed() {
    change(Color::red);
    hide_menu();        // once a color is chosen from the menu, hide the menu
  }

  void blue_pressed() {
    change(Color::blue);
    hide_menu();
  }

  void black_pressed() {
    change(Color::black);
    hide_menu();
  }

  void menu_pressed() {
    // when menu button is pressed, hide the menu button and show the 
    // actual menu of colors
    menu_button.hide();    
    color_menu.show();
  }

  void next();   // defined below

  void quit();   // defined below

  // callback functions; declared here and defined below.

  static void cb_red(Address, Address);
  static void cb_blue(Address, Address);
  static void cb_black(Address, Address);
  static void cb_menu(Address, Address);
  static void cb_next(Address, Address);
  static void cb_quit(Address, Address);
};

// ----------------------------------------------------------
// now define the parts of Lines_window that were only declared
// inside the class

// constructor:

Lines_window::Lines_window(Point xy, int w, int h, const string& title) : 

  // initialization - start by calling constructor of base class 
  Window(xy,w,h,title),    

  // initialize "Next point" button
  next_button(
	      Point(x_max()-150,0),   // location of button
	      70, 20,                 // dimensions of button
	      "Next point",           // label of button
	      cb_next),               // callback function for button
  // initialize quit button
  quit_button(
	      Point(x_max()-70,0),    // location of button
	      70, 20,                 // dimensions of button 
	      "Quit",                 // label of button
	      cb_quit),               // callback function for button
  // initialize the next_x inbox
  next_x(
	 Point(x_max()-310,0),       // location of box
	 50, 20,                     // dimensions of box
	 "next x:"),                 // label of box 
  // initialize the next_y inbox
  next_y(
	 Point(x_max()-210,0),       // location of box
	 50, 20,                     // dimensions of box
	 "next y:"),                 // label of box
  // initialize the outbox
  xy_out(
	 Point(100,0),               // location of box
	 100, 20,                    // dimensions of box
	 "current (x,y):"),          // label of box
  // initialize the color menu
  color_menu(                        
	     Point(x_max()-70,30),   // location of menu
	     70, 20,                 // dimensions of menu
	     Menu::vertical,         // list menu items vertically
	     "color"),               // label of menu 
  // initialize the menu button
  menu_button(
	      Point(x_max()-80,30),  // location of menu button
	      80, 20,                // dimensions of button 
	      "color menu",          // label of button
	      cb_menu)               // callback for button

  // body of constructor follows
{
  // attach buttons and boxes to window
  attach(next_button);
  attach(quit_button);
  attach(next_x);
  attach(next_y);
  attach(xy_out);
  xy_out.put("no point");        // output to out box

  // First make 3 buttons for color menu, one for each color, and 
  // attach them to the menu: the attach function of the Menu struct
  // adjusts the size and location of the buttons; note callback functions).
  // Then attach menu to window but hide it (initially, the menu button
  // is displayed, not the actual menu of color choices).

  color_menu.attach(new Button(Point(0,0),0,0,"red",cb_red)); 
  color_menu.attach(new Button(Point(0,0),0,0,"blue",cb_blue));
  color_menu.attach(new Button(Point(0,0),0,0,"black",cb_black));
  attach(color_menu);
  color_menu.hide(); 

  // attach menu button
  attach(menu_button);

  // attach shape that holds the lines to be displayed
  attach(lines);
}

// ---------------------------- 
// callback function for quit button - boilerplate: 
// When the button is pressed, the system invokes the
// specified callback function.  First argument is address of the
// button (which we won't use, so we don't bother to name it).  Second
// argument, named pw, is address of the window containing the pressed
// button, i.e., address of our Lines_window object.  reference_to
// converts the address pw into a reference to a Lines_window object,
// so we can call the quit() function.  Objective is to call function
// quit() which does the real work specific to this button.

void Lines_window::cb_quit(Address, Address pw) {
  reference_to<Lines_window>(pw).quit();   // quit is defined next
}

//------------------------------------
// what to do when quit button is pressed

void Lines_window::quit() {
  hide();                   // FLTK idiom for delete window
}

// ----------------------------
// callback function for next button - boilerplate:

void Lines_window::cb_next(Address, Address pw) {
  reference_to<Lines_window>(pw).next();  // next is defined next
}

// -------------------------------
// what to do when "next point" button is pressed

void Lines_window::next() {
  // get input data from the inboxes - x and y coordinates
  // of next point
  int x = next_x.get_int();
  int y = next_y.get_int();

  // add the new point to the lines object
  lines.add(Point(x,y));

  // update current position readout - make a string with the
  // coordinate info and use the out box
  stringstream ss;
  ss << '(' << x << ',' << y << ')';
  xy_out.put(ss.str());

  redraw();  // function inherited from Window to redraw the window
}

// -------------------------------
// callback for when red button (part of color menu) is pressed - boilerplate

void Lines_window::cb_red(Address, Address pw) {
  reference_to<Lines_window>(pw).red_pressed();  
  // red_pressed defined in Lines_window class as part of declaration
}

// -------------------------------
// callback for when blue button (part of color menu) is pressed - boilerplate

void Lines_window::cb_blue(Address, Address pw) {
  reference_to<Lines_window>(pw).blue_pressed();  
  // blue_pressed defined in Lines_window class as part of declaration
}

// -------------------------------
// callback for when black button (part of color menu) is pressed - boilerplate

void Lines_window::cb_black(Address, Address pw) {
  reference_to<Lines_window>(pw).black_pressed();  
  // black_pressed defined in Lines_window class as part of declaration
}

// -------------------------------
// callback for when menu button is pressed - boilerplate

void Lines_window::cb_menu(Address, Address pw)
{  
    reference_to<Lines_window>(pw).menu_pressed();
    // menu_pressed defined in Lines_window class as part of declaration
} 

// ---------------------------------------------------
// main - just creates window and invokes gui_main

int main() 
  try {
    // construct the GUI window
    Lines_window win(Point(100,100),600,400,"lines");
    return gui_main();  // inherited from Window; calls FLTK's run
  }
  catch(exception& e) {
    cerr << "exception: " << e.what() << '\n';
    return 1;
  }
  catch(...) {
    cerr << "some exception\n";
    return 2;
  }
