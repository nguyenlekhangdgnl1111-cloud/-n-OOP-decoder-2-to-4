#include "decoder2to4.h"


decoder2to4 :: decoder2to4(){
A = false ;
B = false ;
not_A = false ;
not_B = false;
Y3 = false ;
Y2 = false ;
Y1 = false ;
Y0 = false ;
}

void decoder2to4 :: connect_logic(bool A_t, bool B_t){
if(A == 1 || A == 0 ){
A = A_t;
}
if(B == 1 || B == 0){
B = B_t;
}

}

void decoder2to4 :: setting(bool A_t,bool B_t){
A = A_t;
B = B_t;

notgate_a.set_input(A, 0);
notgate_b.set_input(B, 0);

not_A = notgate_a.get_output();
not_B = notgate_b.get_output();


andgate_y3.set_input(A, 0);
andgate_y3.set_input(B, 1);

andgate_y2.set_input(A, 0);
andgate_y2.set_input(not_B, 1);

andgate_y1.set_input(not_A, 0);
andgate_y1.set_input(B, 1);

andgate_y0.set_input(not_A, 0);
andgate_y0.set_input(not_B, 1);

Y3 = andgate_y3.get_output();
Y2 = andgate_y2.get_output();
Y1 = andgate_y1.get_output();
Y0 = andgate_y0.get_output();




}

void decoder2to4 :: print_truth_table(){


	cout<< "||==================================================||\n";
	cout<< "||       INPUT      ||            OUTPUT            ||\n";
	cout<< "||--------------------------------------------------||\n";
	cout<< "||    A   ||    B   ||  Y3  ||  Y2  ||  Y1  ||  Y0  ||\n";
	cout<< "||--------------------------------------------------||\n";
for(int i= 0 ;i<2;i++){
for(int j= 0 ;j<2;j++){
setting(static_cast<bool>(i),static_cast<bool>(j));

	cout<< "||   "<<A<<"    ||   "<<B<<"    ||   "<<Y3<<"  ||   "<<Y2<<"  ||   "<<Y1<<"  ||   "<<Y0<<"  ||\n";
}}

	cout<< "||==================================================||\n";

}



