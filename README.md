#include <iostream>
using namespace std;
double divide(int num, int denom);
int main(void){
int numerator;
int denominator;
double result;
while(1){
cout << "please enter a numerator and denominator: ";
cin >> numerator;
cin >> denominator;
try{
result = divide(numerator, denominator);
cout << "the result is: " << result << endl;
}
catch(int exception_int){
cout << exception_int << endl;
}
}
cout << "end of program";
}
double divide(int num, int denom){
if(denom == 0)
throw 1;
return (static_cast<double>(num) / denom);
}
