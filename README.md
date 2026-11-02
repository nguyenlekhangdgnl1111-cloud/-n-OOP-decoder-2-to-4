#include "circuit.h"
#include <iostream>

// --- wire methods ---
void wire::set_wire(string t_name, logicstate t_state) {
    name = t_name;
    state = t_state;
}

logicstate wire::get_state() {
    return state;
}

string wire::get_name() {
    return name;
}

// --- build_logicgate methods ---
build_logicgate::build_logicgate(string name1) {
    input1 = 0;
    input2 = 0;
    output = 0;
    gate_name = name1;
}

void build_logicgate::setname(string name) {
    gate_name = name;
}

void build_logicgate::connect_input1(wire* t_input1) {
    input1 = t_input1;
}

void build_logicgate::connect_input2(wire* t_input2) {
    input2 = t_input2;
}

void build_logicgate::connect_output(wire* t_output) {
    output = t_output;
}

// --- and_gate methods ---
and_gate::and_gate(string name1) : build_logicgate(name1) {}

void and_gate::evaluate() {
    logicstate s1 = input1->get_state();
    logicstate s2 = input2->get_state();
    logicstate out_state = (s1 == high && s2 == high) ? high : low;
    output->set_wire(output->get_name(), out_state);
}

// --- not_gate methods ---
not_gate::not_gate(string name2) : build_logicgate(name2) {}

void not_gate::evaluate() {
    logicstate s1 = input1->get_state();
    logicstate out_state = (s1 == high) ? low : high;
    output->set_wire(output->get_name(), out_state);
}

// --- decoder2to4 methods ---
decoder2to4::decoder2to4() : nota0("nota0"), nota1("nota1"), andy0("andy0"), andy1("andy1"), andy2("andy2"), andy3("andy3") {
    input_A0 = 0;
    input_A1 = 0;
}

void decoder2to4::connect(wire* a0, wire* a1) {
    input_A0 = a0;
    input_A1 = a1;

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

void decoder2to4::evaluate() {
    nota0.evaluate();
    nota1.evaluate();
    andy3.evaluate();
    andy2.evaluate();
    andy1.evaluate();
    andy0.evaluate();
}

void decoder2to4::print_truth_table() {
    logicstate y3, y2, y1, y0;
    int state1, state2;
    cout << "||==================================================||\n";
    cout << "||       INPUT      ||            OUTPUT            ||\n";
    cout << "||--------------------------------------------------||\n";
    cout << "||   A1   ||   A0   ||  Y3  ||  Y2  ||  Y1  ||  Y0  ||\n"; 
    cout << "||==================================================||\n";

    for (state2 = 0; state2 < 2; state2++) {
        for (state1 = 0; state1 < 2; state1++) {
            if (input_A1) 
                input_A1->set_wire("A1", (logicstate)state2);
            if (input_A0)
                input_A0->set_wire("A0", (logicstate)state1);
            evaluate();
            y3 = w_y3.get_state();
            y2 = w_y2.get_state();
            y1 = w_y1.get_state();
            y0 = w_y0.get_state();
             
            cout << "||   " << state2 << "    ||   " << state1 << "    ||   " << y3 << "  ||   " << y2 << "  ||   " << y1 << "  ||   " << y0 << "  ||\n";
        }
    }
    cout << "||==================================================||";
}
