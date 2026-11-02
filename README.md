#include "circuit.h"

int main() {
    wire w_a0;
    wire w_a1;

    decoder2to4 dec;
    dec.connect(&w_a0, &w_a1);
    dec.evaluate();

    dec.print_truth_table();

    return 0;
}
