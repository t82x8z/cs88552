java c
ESE 123
Laboratory 9
Stopwatch and Timer
Parts list:
Quantity
Reference
Value
Description
¼
D1
Red
Display, Four Digit LED, Red
¼
D1
Blue
Display, Four Digit LED, Blue
¼
D1
White
Display, Four Digit LED, White
¼
D1
Yellow
Display, Four Digit LED, Yellow
4
Q1-Q4

TRANS PNP 40V 0.2A TO92
3
SW1-SW3

SWITCH TACTILE SPST-NO 0.05A 12V
Objec/ves:
In this experiment we will:
•    Install the 7 segment numeric display and associated drive circuitry
•    Install the tacAle switches
•    Learn some more coding
Displaying decimal numbers using integer division and modulo:If  we  want  to   display   a  binary  number  in  decimal  we  need  to   make  a  series  of conversions. We ﬁrst need to convert the binary number to a decimal number, and then we  need to  look  up which segments should  be turned  on to  represent the  number’s glyph (the LED segments are turned on to represent the number). The laMer conversion is handled by the display_digit funcAon.If we have a four digit decimal number, say 6837, we can ﬁnd the ﬁrst decimal digit by integer dividing by 1000 to get 6 and displaying the 6 in the thousands digit (digit 1 on our  four  digit  display).  Assuming  we   had  saved   6837   in  an   integer  variable  called disp_int, this would look like:
display_digit (disp_int/1000, 1);
We then use the modulo funcAon to ﬁnd the remainder − the hundreds, tens, and ones and save it in the original variable. ATer this operaAon disp_int would contain 837:
disp_int = disp_int % 1000;
Now we can repeat the process for the next digit. We integer divide by 100 to get the number of hundreds (8) and display this in digit 2:
display_digit (disp_int/100, 2);
And compute the remainder (37) − the hundreds are gone, the tens and ones remain:
disp_int = disp_int % 100;
The tens (3) are displayed next:
display_digit (disp_int/10, 3);
And then the ones (7):
display_digit (disp_int%10, 4);
Note that disp_int starts at 6837, but ends as 37. This display procedure destroys the original value. If we wanted to retain the original value, we would need to make a copy.
Procedure: circuit assembly
In today’s lab you add the four digit LED display, the remaining digit select transistors, and the pushbuMons. The LED display is available in red, blue, white, or yellow.
1. Collect the parts for today’s lab from your TAs.
2. Carefully  clip  oﬀ  pracAce  resistors  R1  through  R5  with  wire  cuMers.  These  are  the resistors you ﬁrst learned to solder with. The LED display will be installed directly over this  area  and  it  will  not  lie  ﬂat  against  the  board  if  there  are  any  large  (>1  mm) protrusions in this area.
3. Install the four digit  LED assembly.  Note that there  are six  pins on one side of the device and seven pins on the other; the LED will ﬁt only one way around. The wire leads on this device are thin and ﬂexible, and this will make it diﬃcult to get all the leads into the circuit board at the same Ame. Force will not help the situaAon. What will help is:
3.1.Making certain that the pins are straight before you start.
3.2.Using a thin tool (tweezers or a Any screwdriver) to help guide the pins into the holes one at a Ame.
4. Ensure that the LED display is lying ﬂush against the board and solder one pin. Check to see that the LED is sAll ﬂush and then solder the remaining pins. The LED assembly has a plasAc ﬁlm protecAng it from damage. It is probably a good idea to leave this ﬁlm on the LED unAl we complete the case.
5. Install four 2N3906  pnp transistors  at  Q1 through Q4  (we’ve already  installed  Q5). Align the parts to match the outline printed on the board.
6. Install tacAle switches SW1 (UP), SW2 (OK), and SW3 (DOWN). These switches snap into the board and will hold themselves in while you solder them.
7. Solder everything down and trim the excess leads. This concludes today’s hardware assembly.


Procedure: ini/al display coding
8. Create a new MPlab project and connect your board to the programmer as in your previous lab. Create a new avr-main.c source ﬁle and type the following into it:

9.   Create  a  new  xc8_header.h  ﬁle  named funcAons.h  and  open  it for  ediAng.  Delete whatever is there and paste in the source code from the funcAons.h ﬁle provided with the lab.
10.Build the source code and install in on your project. You should see it display 6837.
Procedure: coding a counter
Displaying 6837 is a bit of a feat, but were not done yet. Let’s make a counter out of it.
11.Modify your source code to iniAalize number to 0 instead of 6837;
12.Add a line of code aTer the last (ones) digit is displayed. This code will increment number by one. The code will look like this:
number++;


13.Build and install your code. It should count up rapidly. ATer it reaches 9999 strange stuﬀ will happen. We’ll ﬁx that later.
Procedure: coding a stopwatchThis counter increments about 125 Ames a second. We can slow it down to about 100 Ames per second by adding a decimal point aTer the second digit. The addiAonal Ame required to display the decimal point will slow the code down. With the decimal point it will be slowed to counAng around 100 Ames代 写ESE 123 Laboratory 9 Stopwatch and TimerR
代做程序编程语言 per second and we can consider it to be a (very approximate) stopwatch. We’ll deal with the accuracy next week.
14.Add a line of code in the while loop to display a decimal point in the second digit. The code will look like this:
display_digit(‘ . ’,2);
15.Build  and  install  your  code.  You  should  see  your  project  counAng  more-or-less  in seconds.
Procedure: adding some bu16.Add a new char type variable called run iniAalized to zero. This will be a mode for our stopwatch if it is zero the watch will be stopped, if it is 1 the watch will run. Put it in the code near where the other variables are declared:
char run = 0;
17.Change your code so that number is only incremented if the run variable is 1.
if (run == 1)
{
number++;
}
18.Add a test for the UP buMon (bit 0 of PORTE) to start the clock if it is pressed:
if ((PORTE_IN  0b00000001) == 0)
{
run = 1;         // enter run mode
}
19.Compile and load your code. Your project should display 00.00 unAl you press the top buMon. Then it should begin counAng.20.The  stopwatch  sAll  behaves  badly  when  it  reaches  99.99  seconds.  Add  an   IF
statement to set run = 0 if number is equal to 9999. Where should this code go? 21.Add a test for the OK buMon to stop the watch if you press it.
22.Add a test for the DOWN buMon to reset number to zero if you press it. 23.number isn’t such a great name for a variable. Let’s change it.
23.1.Double click on any instance of the number variable.
23.2.Right click and select Refactor. Select Rename. A new dialog box will open. 23.3.Type in a beMer name, maybe ‘hundredths’ or something like that.
23.4.Click on the Refactor buMon. All instances will be changed.
24.Test your code. When you are happy with it, ask your TA to verify that it works. This will be part of your grade for this lab.
25.Check your stopwatch against another stopwatch (e.g. your phone). What is the real elapsed Ame when your stopwatch hits 99.99 seconds?
26.Take a screenshot of your code.  It should be neatly formaMed and commented for clarity.
27.Save this code for next week.
Procedure: Countdown TimerWe now  have a  program that counts from 0 to 99.99 seconds and then stops.    With some modiﬁcaAons, we can turn our stopwatch into a Amer.    Let’s make a Amer that counts down from 10, 20, or 30 seconds using the stopwatch code.  The UP buMon will start  a  10-second  Amer,  the  OK  buMon  will  start  a  20-second  Amer,  and  the  DOWN buMon will start a 30-second Amer.
28.Create a  new  MPLAB  project.  Create  a  new  header  and  source  ﬁle,  and  copy and paste the header ﬁle and the code for your stopwatch into these ﬁles.
29.We have seen how to increment a variable’s value by 1 using two plus signs. Similarly, we can decrement by one using two minus signs::
hundredths—-;
Modify your code to count down instead of up.
30.Modify the  IF  statements  that  check  the  buMons.  The  UP  buMon  should  load  the value for  10 seconds  into the hundredths variable and start the Amer. The OK and DOWN buMons should also start the Amer, but load hundredths with values for 20 and 30 seconds, respecAvely.  When you press the buMon, the display should update to  the   corresponding  seconds.  When  you   release  the   buMon,  the  Amer   should immediately begin counAng down.
31.Similarly  to  the  stopwatch,  your  Amer  should  stop  counAng  when  it   reaches  0. Modify your code so that it stops counAng when hundredths = 0.
32.We now have a Amer that can count down from 10, 20, or 30 seconds.   Let’s add an auditory cue that the Amer has expired.   We can employ the buzzer from lab 7 to accomplish this.  Add a funcAon to create a sound from the buzzer for a few seconds. We can use a WHILE loop with a < operator to bend the buzzer a deﬁned number of Ames, as in this example:
Place this  code  in  a  new  funcAon  (not  your  main  func;on)  and  give  the  funcAon  a suitable  name.  When  the   buzzer  sounds,  the   LED  display  will   not   be  on.  This  is expected behavior.
33.Add a call to your buzzer funcAon. The buzzer should go oﬀ once when the Amer reaches 0, but it should not go oﬀ conAnuously. Try to ﬁgure out how to make the buzzer go oﬀ when the Amer reaches 0. The buzzer should sound for a few seconds and  then  stop  buzzing.  A  nested  IF  statement  might  be  useful  here  since  two condiAons need to be true before the buzzer sounds.
34.You  can  modify  the  _delay_ms   argument   (change   1  to   something  else)  to   get diﬀerent “notes” from your buzzer.   You can also change the WHILE condiAon (say, from 2000 to 3000) to adjust the buzzer duraAon.  Make it around 3 seconds.
35.When you are happy with your code, please ask a TA to verify its operaAon. This will be part of your grade.
36.Screenshot your code. Your code should be neat and commented for clarity.
37.Answer the quiz quesAons. You will be asked to upload images of your stopwatch and Amer code.


         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
