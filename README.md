#ifndef CIRCUIT_H
#define CIRCUIT_H

#include <string>

using namespace std;

enum logicstate {
    low = 0,
    high = 1,
    unknown
};

class wire {
private:
    string name;
    logicstate state;
public:
    void set_wire(string t_name, logicstate t_state);
    logicstate get_state();
    string get_name();
};

class build_logicgate {
protected:
    wire *input1;
    wire *input2;
    wire *output;
    string gate_name;
public:
    build_logicgate(string name1);
    void setname(string name);
    void connect_input1(wire* t_input1);
    void connect_input2(wire* t_input2);
    void connect_output(wire* t_output);
};

class and_gate : public build_logicgate {
public:
    and_gate(string name1);
    void evaluate();
};

class not_gate : public build_logicgate {
public:
    not_gate(string name2);
    void evaluate();
};

class decoder2to4 {
private:
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

public:
    decoder2to4();
    void connect(wire* a0, wire* a1);
    void evaluate();
    void print_truth_table();
};

#endif // CIRCUIT_H
