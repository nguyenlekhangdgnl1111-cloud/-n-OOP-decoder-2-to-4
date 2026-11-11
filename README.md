#include <iostream>
#include <string>

using namespace std;

// ==========================================
// 1. LOGIC GATE CLASSES (từ logic_gate.h & logic_gate.cpp)
// ==========================================

class logicgate {
private:
    string ten;
public:
    logicgate(string t) {
        ten = t;
    }
    virtual ~logicgate() {}

    virtual bool get_output() = 0; // Lấy output
    virtual void set_input(bool value, int pin) = 0; // Nhận input
    
    string get_name() {
        return ten;
    }
};

// Mô phỏng cổng 2 chân
class gate_2_input : public logicgate {
protected:
    bool input[2]; // Mảng có 2 giá trị, tương đương Input1 và Input2
public:
    gate_2_input(string t) : logicgate(t) {
        input[0] = false;
        input[1] = false;
    }
    
    void set_input(bool value, int pin) override {
        if(pin == 0 || pin == 1) {
            input[pin] = value;
        }
    }
};

// Tạo class cổng NOT
class not_gate : public logicgate {
private:
    bool input;
public:
    not_gate(string t) : logicgate(t) {
        input = false;
    }

    void set_input(bool value, int pin) override {
        input = value;
    }

    bool get_output() override {
        return !input;
    }
};

// Tạo class cổng AND
class and_gate : public gate_2_input {
public:
    and_gate(string t) : gate_2_input(t) {}

    bool get_output() override {
        return input[0] && input[1];
    }
};

// ==========================================
// 2. DECODER 2-TO-4 CLASS (từ decoder_2to4_high_active.h)
// ==========================================

class decoder2to4 {
private:
    not_gate notgate_a{"notgate_A"};
    not_gate notgate_b{"notgate_B"};
    and_gate andgate_y3{"andgate_Y3"};
    and_gate andgate_y2{"andgate_Y2"};
    and_gate andgate_y1{"andgate_Y1"};
    and_gate andgate_y0{"andgate_Y0"};

    bool A, B, not_A, not_B, Y3, Y2, Y1, Y0;

public:
    decoder2to4() {
        A = false;
        B = false;
        not_A = false;
        not_B = false;
        Y3 = false;
        Y2 = false;
        Y1 = false;
        Y0 = false;
    }

    void connect_logic(bool A_t, bool B_t) {
        A = A_t;
        B = B_t;

        // Thiết lập đầu vào cho cổng NOT
        notgate_a.set_input(A, 0);
        notgate_b.set_input(B, 0);

        not_A = notgate_a.get_output();
        not_B = notgate_b.get_output();

        // Kết nối cho Y3 = A AND B
        andgate_y3.set_input(A, 0);
        andgate_y3.set_input(B, 1);
        Y3 = andgate_y3.get_output();

        // Kết nối cho Y2 = A AND NOT_B
        andgate_y2.set_input(A, 0);
        andgate_y2.set_input(not_B, 1);
        Y2 = andgate_y2.get_output();

        // Kết nối cho Y1 = NOT_A AND B
        andgate_y1.set_input(not_A, 0);
        andgate_y1.set_input(B, 1);
        Y1 = andgate_y1.get_output();

        // Kết nối cho Y0 = NOT_A AND NOT_B
        andgate_y0.set_input(not_A, 0);
        andgate_y0.set_input(not_B, 1);
        Y0 = andgate_y0.get_output();
    }

    void setting(bool A_t, bool B_t) {
        connect_logic(A_t, B_t);
    }

    void print_truth_table() {
        cout << "-----------------------------------------\n";
        cout << " A | B || Y3 | Y2 | Y1 | Y0 \n";
        cout << "-----------------------------------------\n";
        for (int i = 0; i < 4; i++) {
            bool a_val = (i >> 1) & 1;
            bool b_val = i & 1;
            setting(a_val, b_val);
            cout << " " << a_val << " | " << b_val << " ||  "  << Y3 << "  |  " << Y2 << "  |  " << Y1 << "  |  " << Y0 << "  \n";
        }
        cout << "-----------------------------------------\n";
    }
void enter_input_signal(int A_s, int B_s){
    cout << "||--------------------------------------------------------||\n";
    cout << "||   A   ||   B   ||   Y3   ||   Y2   ||   Y1   ||   Y0   ||\n";
    cout << "||--------------------------------------------------------||\n";
    setting(static_cast<bool>(A_s),static_cast<bool>(B_s));
    cout << "||   " << A_s << "   ||   " << B_s << "   ||   " << Y3 << "    ||   " << Y2 << "    ||   " << Y1 << "    ||   " << Y0 << "    ||\n";
    cout << "||--------------------------------------------------------||\n";

}

int  voltage_to_logic(double v) {
    if (v >= 3 && v <= 5) {
        return 1;
    } 
    else if (v >= -2 && v <= 1) {
        return 0;
    } 
   else{
   return -1;
   } 
}

void enter_input_voltage(double A_v, double B_v){
int A = voltage_to_logic(A_v);
int B = voltage_to_logic(B_v);
if(A== -1 || B == -1){
enter_input_unknown();	
return ;}
cout << "\n||--------------------------------------------------------||\n";
cout << "||   A   ||   B   ||   Y3   ||   Y2   ||   Y1   ||   Y0   ||\n";
cout << "||--------------------------------------------------------||\n";	
setting(static_cast<bool>(A),static_cast<bool>(B));
cout << "||   " << A   << "   ||   " << B   << "   ||   " << Y3 << "    ||   " << Y2 << "    ||   " << Y1 << "    ||   " << Y0 << "    ||\n";
cout << "||--------------------------------------------------------||\n";
}

void enter_input_unknown(){
cout << "\n||--------------------------------------------------------||\n";
cout << "||   A   ||   B   ||   Y3   ||   Y2   ||   Y1   ||   Y0   ||\n";
cout << "||--------------------------------------------------------||\n";	
cout << "||   X   ||   X   ||   un   ||   un   ||   un   ||   un   ||\n";
cout << "||--------------------------------------------------------||\n";
}

void game_play(){

int select ;
cout << "CHON CHE DO DAU VAO: \n[0] TINH HIEU\n[1] DIEN AP";
cout << "\nCHON : ";
cin >> select ;

if(select == 0 ){
int a_s,b_s;
cout << "\nHAY NHAP GIA TRI A : ";
cin >> a_s ;
cout << "\nHAY NHAP GIA TRI B : ";
cin >> b_s;// ngoai le
if((a_s == 0 || a_s == 1) && (b_s ==0 || b_s == 1)){
enter_input_signal(a_s,b_s);

}else{
enter_input_unknown();
}
}


if (select == 1) {
double a_v, b_v;
double V_IL, V_IH;
int n = 1;     
while (n == 1) { 
cout << "\nHAY CHON GIA TRI V_IL va V_IH : \n";
cout << "|| -2(V) < V_IL < 1(V) || 3(V) < V_IH < 5(V) ||\n";        
cout << "V_IL: ";cin >> V_IL;        
cout << "V_IH: "; cin >> V_IH;        

if (V_IL >= -2 && V_IL <= 1 && V_IH >= 3 && V_IH <= 5) {
n = 0; 
cout << "Da nhap gia tri hop le!\n";
cout << "\nHAY NHAP GIA TRI A : ";
cin >> a_v ;
cout << "\nHAY NHAP GIA TRI B : ";
cin >> b_v;
 if( ((a_v <= V_IL && a_v >= -2) || (a_v >= V_IH && a_v <= 5)) && ((b_v <= V_IL && b_v >= -2) || (b_v >= V_IH && b_v <= 5))){
enter_input_voltage(a_v,b_v);  
}
else{
enter_input_unknown();    
} }
else{
cout << "Nguong khong hop le!\n";
n = 0;	
}
}   
}	
}
};
int main(void) {
decoder2to4 dec;
dec.print_truth_table();
dec.game_play();

return 0;
}
