# FPGA_Power_Test
For FPGA power consumption test.


module Power_Test(
input               Clk,
input               Rst_n,
output  [31:0]      Data1,
output  [31:0]      Data2,
output  [31:0]      Data3

);

parameter REG_LENGTH = 2000;
parameter MEM_LENGTH = 1000;
parameter MUL_LENGTH = 10;

reg [31:0]  Dly_reg [REG_LENGTH - 1 : 0];
reg [31:0]  Dly_mem [MEM_LENGTH - 1 : 0];
reg [31:0]  Mul_reg [MUL_LENGTH - 1 : 0];
reg [31:0]  Num_Gen;


assign Data1 = Dly_mem[MEM_LENGTH - 1];
////assign Data2 = Mul_reg[MUL_LENGTH - 1];




genvar i;
generate
	for(i = 0; i <= REG_LENGTH - 2; i = i + 1) begin:reg_Dly

always @(posedge Clk or negedge Rst_n)
    if(!Rst_n)
        Dly_reg[i+1][31:0] <= ~32'hAAAA_AAAA;
    else
        Dly_reg[i+1][31:0] <= ~Dly_reg[i][31:0];
        

end
endgenerate

always @(posedge Clk or negedge Rst_n)
    if(!Rst_n)
        Dly_reg[0] <= 32'hAAAA_AAAA;
    else
        Dly_reg[0] <= Num_Gen;



genvar j;
generate
	for(j = 0; j <= MEM_LENGTH - 2; j = j + 1) begin:mem_Dly

always @(posedge Clk or negedge Rst_n)
    if(!Rst_n)
        Dly_mem[j+1][31:0] <= 32'hAAAA_AAAA;
    else
        Dly_mem[j+1][31:0] <= Dly_mem[j][31:0];
        

end
endgenerate


always @(posedge Clk or negedge Rst_n)
    if(!Rst_n)
        Dly_mem[0] <= 32'hAAAA_AAAA;
    else
        Dly_mem[0] <= Dly_reg[REG_LENGTH - 1];


// genvar k;
// generate
// 	for(k = 0; k <= MUL_LENGTH - 2; k = k + 1) begin:reg_Mul

// always @(posedge Clk or negedge Rst_n)
//     if(!Rst_n)
//         Mul_reg[k+1][31:0] <= 32'hAAAA_AAAA;
//     else
//         Mul_reg[k+1][31:0] <= Mul_reg[k][31:0] * Mul_reg[k+1][31:0];
        

// end
// endgenerate

// always @(posedge Clk or negedge Rst_n)
//     if(!Rst_n)
//         Mul_reg[0] <= 32'hAAAA_AAAA;
//     else
//         Mul_reg[0] <= Dly_mem[MEM_LENGTH - 1];





always @(posedge Clk or negedge Rst_n)
    if(!Rst_n)
        Num_Gen <= 32'hAAAA_AAAA;
    else
        Num_Gen <= ~Num_Gen;





endmodule



//test bench

`timescale 1ns/1ps
module Power_Test_tb;

reg         Clk     ;
reg         Rst_n   ;
wire[31:0]  Data1   ;
wire[31:0]  Data2   ;
wire[31:0]  Data3   ;

Power_Test Power_Test(
/*  input               */.Clk      (   Clk     ),
/*  input               */.Rst_n    (   Rst_n   ),
/*  output  [31:0]      */.Data1    (   Data1   ),
/*  output  [31:0]      */.Data2    (   Data2   ),
/*  output  [31:0]      */.Data3    (   Data3   )
);

initial begin
Clk     =   1;
Rst_n   =   0;
#20
Rst_n   =   1;

end

always #5 Clk = ~Clk;

endmodule

