#include <iostream>
#include <cstdlib>
#include <string>
using namespace std;

enum  logicstate{// header
high = 1,
low = 0 ,
unknown ,
high_z  	
};


class wire {//header
private:
string name ;
logicstate state  ;  
public: 
void set_wire(string t_name, logicstate t_state){
name = t_name;
state = t_state;
	
}	

logicstate get_state(){
return state ;	
}

string get_name(){
return name ;	
}	
};

class build_logicgate{//header
protected : 
wire *input1;
wire *input2;
wire *output;
string gate_name;
public :
build_logicgate(string name1){
input1 = 0 ;
input2 =  0;
output = 0 ;
gate_name = name1;	
}	

void connect_input1(wire* t_input){
input1 = t_input	;
}

void connect_input2(wire* t_input){
input2 = t_input	;
}

void connect_output(wire* t_output){
output = t_output;	
}  
		
};

class and_gate : public  build_logicgate{ // header
public :
and_gate(string name1) : build_logicgate(name1) {}	
void evaluate(){
logicstate s1 = input1->get_state();
logicstate s2 = input2->get_state();
logicstate out_state = (s1 == high && s2 == high)? high : low;
output->set_wire(output-> get_name(), out_state);

//cout<< "Cong and ["<< gate_name<< "] "<< s1 <<" AND "<< s2 << " ket qua la : "<<out_state<<endl;
}
};

class not_gate : public build_logicgate{//header
public :
not_gate(string name2) : build_logicgate(name2){}		
void evaluate(){
logicstate s1 = input1->get_state();
logicstate out_state = (s1 == high)? low : high;
output->set_wire(output-> get_name(), out_state);

//cout<< "Cong not ["<< gate_name<< "] "<< s1 <<" NOT "<<"\n"<< " ket qua la : "<< out_state<<endl;
}
};

class decoder2to4 {//header
private :
wire w_A0;
wire w_notA0;
wire w_A1;
wire w_notA1;
wire w_y0;
wire w_y1;
wire w_y2;
wire w_y3;

not_gate nota0;	
not_gate nota1;
and_gate andy0;
and_gate andy1;
and_gate andy2;
and_gate andy3;

wire* input_A0; 
wire* input_A1; 

public :
decoder2to4(string name):
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

void connect_inputs(wire* a0,wire* a1){
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
	
void evaluate(){
nota0.evaluate();	
nota1.evaluate();
andy3.evaluate();
andy2.evaluate();
andy1.evaluate();
andy0.evaluate();
}


void get_outputs(logicstate &y3, logicstate &y2, logicstate &y1, logicstate &y0) {

int state1, state2;
wire w_a0;
wire w_a1;
cout<<" nhap vao gia tri  A0 va A1 \n";

cout<<" nhap A0:";
cin >> state1;



cout<<"nhap A1:  \n";
cin >> state2;  
	

if(input_A1) 
input_A1->set_wire("A1",(logicstate)state2);
if(input_A0)
input_A0->set_wire("A0",(logicstate)state1);

evaluate();
y3 = w_y3.get_state();
y2 = w_y2.get_state();
y1 = w_y1.get_state();
y0 = w_y0.get_state();
cout<< "||==================================================||";
cout<< "\n||       INPUT      ||            OUTPUT            ||\n";
cout<< "||--------------------------------------------------||\n";
cout<< "||   A1   ||   A0   ||  Y3  ||  Y2  ||  Y1  ||  Y0  ||\n"; 
cout<< "||   "<<state2<<"    ||   "<<state1<<"    ||   "<<y3<<"  ||   "<<y2<<"  ||   "<<y1<<"  ||   "<<y0<<"  ||\n";
cout<< "||==================================================||";
}

}

		
};



int main(){

wire w_a0;
wire w_a1;
  
decoder2to4 dec(" ");
dec.connect_inputs(&w_a0,&w_a1);
dec.evaluate();

logicstate y3,y2,y1,y0;
dec.get_outputs(y3,y2,y1,y0);


return 0 ;

}


