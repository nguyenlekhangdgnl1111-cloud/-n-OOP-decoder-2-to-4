#include <iostream>
#include "headerfile.h"

using namespace std ;

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
