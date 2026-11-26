module ff_jk (
input wire clk,
input wire j,
input wire k,
input wire reset,
input wire preset,
output reg q,
output reg not_q
);

always @(posedge clk ) begin
  if(!reset) begin 
q <= 1'b0; 
not_q <= 1'b1;   
end
  else if(!preset) begin
q <= 1'b1; 
not_q <= 1'b0; 
end

else begin
case ({j,k})
 2'b00: begin
 q <= q ; 
 not_q <= not_q ;   
 end
  2'b01: begin
 q <= 1'b0; ; 
 not_q <= 1'b1 ;   
 end
  2'b10: begin
 q <= 1'b1; ; 
 not_q <= 1'b0 ;   
 end
  2'b11: begin
 q <= not_q ; 
 not_q <= q ;   
 end 
endcase
end
end
    
endmodule


