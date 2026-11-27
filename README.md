module counter_4bit (
input wire clk,

input wire [3:0] reset,
input wire [3:0] preset,
output reg [3:0] q,
output reg [3:0] not_q
);

reg [3:0] j;
reg [3:0] k;

FF_JK jA(.clk(clk),.j(j[3]),.k(k[3]),.reset(reset[3]),.preset(preset[3]),.q(q[3]),.not_q(not_q[3]));
FF_JK jB(.clk(clk),.j(j[2]),.k(k[2]),.reset(reset[2]),.preset(preset[2]),.q(q[2]),.not_q(not_q[2]));
FF_JK jC(.clk(clk),.j(j[1]),.k(k[1]),.reset(reset[1]),.preset(preset[1]),.q(q[1]),.not_q(not_q[1]));
FF_JK jD(.clk(clk),.j(j[0]),.k(k[0]),.reset(reset[0]),.preset(preset[0]),.q(q[0]),.not_q(not_q[0]));

// vùng thiết kế
assign    j[3] = not_q[0] & not_q[2] & not_q[1];
assign    k[3] = not_q[0] & not_q[2] & not_q[1];
assign    j[2] = not_q[1] & not_q[0] ; 
assign    k[2] = not_q[1] & not_q[0] ;
assign    j[1] = not_q[0] ;
assign    k[1] = not_q[0] ;
assign    j[0] = 1'b1 ;
assign    k[0] = 1'b1 ;
//
endmodule



module FF_JK (
input wire clk,
input wire j,
input wire k,
input wire reset,
input wire preset,
output reg q,
output reg not_q
);

always @(posedge clk) begin
if(!reset) begin
q <= 1'b0 ;
not_q <= 1'b1 ;     
end 
else if(!preset) begin 
q <= 1'b1;
not_q <= 1'b0;
end    

else begin
case ({j,k})
2'b00 : begin
q <= q ;
not_q <= ~q;
end 
2'b01 : begin
q <= 1'b0 ;
not_q <= 1'b1;
end 
2'b10 : begin
q <= 1'b1 ;
not_q <= 1'b0;
end 
2'b11 : begin
q <= ~q ;
not_q <= q;
end     
endcase
end    
end
endmodule
