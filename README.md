#include "headerfile.h"

void wire :: set_wire(string t_name, logicstate t_state){
name = t_name;
state = t_state;
}

logicstate wire :: getstate(void){
return state ;	
}

string wire :: getstate(void){
return name ;	
}

build_logicgate :: build_logicgate(string name1){
	
input1 = 0 ;
input2 = 0 ;
output = 0 ;
gate_name = name1;	
}

void build_logicgate :: connect_input1(wire *t_input1){
input1 = t_input1;	
}

void build_logicgate :: connect_input2(wire *t_input2){
input2 = t_input2;	
}

void build_logicgate :: connect_output(wire *t_output){
input1 = t_output;	
}

void and_gate :: evaluate(){
logicstate s1 = input1 -> getstate();
logicstate s2 = input2 -> getstate();
logicstate s = (s1 == high && s2 == high) ? high : low;
output->set_wire(output-> get_name(), out_state);
}

void not_gate :: evaluate(){
logicstate s1 = input1->get_state();
logicstate out_state = (s1 == high)? low : high;
output->set_wire(output-> get_name(), out_state);

void decoder2to4 :: decoder2to4(string name):
nota0("notA0_"+name),nota1("notA1_"+name),andy0("andY0"+name),
andy1("andY1"+name),andy2("andY2"+name),andy3("andY3"+name),
input_A0(0),input_A1(0)
{
w_A0.set_wire("A0",high);
w_A1.set_wire("A1",low);
w_notA0.set_wire("not_A0",high);
w_notA1.set_wire("not_A1",high);
w_y0.set_wire("Y0",low);
w_y1.set_wire("Y1",low);
w_y2.set_wire("Y2",low);
w_y3.set_wire("Y3",low);
}

void decoder2to4 ::  connect_inputs(wire* a0,wire* a1){
input_A0= a0,input_A1 = a1;

nota0.connect_input1(input_A0);
nota0.connect_output(&w_notA0);

nota1.connect_input1(input_A1);
nota1.connect_output(&w_notA1);

andy3.connect_input1(input_A0);
andy3.connect_input2(input_A1);
andy3.connect_output(&w_y3);

andy2.connect_input1(&w_notA0);
andy2.connect_input2(input_A1);
andy2.connect_output(&w_y2);

andy1.connect_input1(input_A0);
andy1.connect_input2(&w_notA1);
andy1.connect_output(&w_y1);

andy0.connect_input1(&w_notA0);
andy0.connect_input2(&w_notA1);
andy0.connect_output(&w_y0);
}

void decoder2to4 ::  evaluate(){
nota0.evaluate();
nota1.evaluate();
andy3.evaluate();
andy2.evaluate();
andy1.evaluate();
andy0.evaluate();
}


void decoder2to4 ::  get_outputs(logicstate &y3, logicstate &y2, logicstate &y1, logicstate &y0) {
validateinput tester;
int state1=0, state2=0;
wire w_a0;
wire w_a1;

cout<<" nhap vao gia tri  A0 va A1 \n";

cout<<" nhap A0:";
state1 = tester.check();

cout<<" nhap A1:";
state2 = tester.check();

string A0=(state1 != 3667) ? to_string(state1) : "u" ;
string A1=(state2 != 3667) ? to_string(state2) : "u" ;
string Y3="u", Y2="u", Y1="u", Y0="u";

if (state1 != 3667 && state2 != 3667) {
if(input_A1)
input_A1->set_wire("A1",(logicstate)state2);
if(input_A0)
input_A0->set_wire("A0",(logicstate)state1);

evaluate();
y3=w_y3.get_state();
y2=w_y2.get_state();
y1=w_y1.get_state();
y0=w_y0.get_state();
Y3 = to_string((int)w_y3.get_state());
Y2 = to_string((int)w_y2.get_state());
Y1 = to_string((int)w_y1.get_state());
Y0 = to_string((int)w_y0.get_state());
}
cout<< "||==================================================||";
cout<< "\n||       INPUT      ||            OUTPUT            ||\n";
cout<< "||--------------------------------------------------||\n";
cout<< "||   A1   ||   A0   ||  Y3  ||  Y2  ||  Y1  ||  Y0  ||\n";
cout<< "||   "<<A1<<"    ||   "<<A0<<"    ||   "<<Y3<<"  ||   "<<Y2<<"  ||   "<<Y1<<"  ||   "<<Y0<<"  ||\n";
cout<< "||==================================================||";
}
