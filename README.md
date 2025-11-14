# **EXP4: 4-bit Ripple Carry Adder using Task and 4-bit Ripple Counter using Function with Testbench**

---

## **Aim**
To design, simulate, and verify a **4-bit Ripple Carry Adder** using **Task** and a **4-bit Ripple Counter** using **Function** in Verilog HDL.

---

## **Apparatus Required**
- System with **Vivado Design Suite**
---

## **Theory**
### **Ripple Carry Adder**
A 4-bit Ripple Carry Adder (RCA) adds two 4-bit binary numbers by cascading four full adders. The carry-out of each full adder acts as the carry-in for the next stage. Using a **task** in Verilog, the addition operation can be modularized for each bit.
<img width="692" height="268" alt="image" src="https://github.com/user-attachments/assets/b70c348a-049a-4462-88a4-ad6905446cf4" />


### **Ripple Counter**
A Ripple Counter is a sequential circuit that counts in binary. In a 4-bit ripple counter, the output of one flip-flop serves as the clock input for the next flip-flop. Using a **function** in Verilog allows the counting logic to be encapsulated and reused.

<img width="1024" height="768" alt="image" src="https://github.com/user-attachments/assets/fa8730fc-e48e-477a-b2e3-6697a95355d3" />

---

## **Program**

### **4-bit Ripple Carry Adder using Task**

```verilog
module ripple_carry_adder_4bit(
    input  [3:0] A, B,
    input  cin,
    output [3:0] SUM,
    output cout
);
    wire c0, c1, c2, c3;
    
    
    task full_adder;
        input a, b, cin;
        output sum, cout;
        begin
            sum = a ^ b ^ cin;
            cout = (a & b) | (b & cin) | (a & cin);
        end
    endtask
  
    reg s0, s1, s2, s3;
    reg carry0, carry1, carry2, carry3;
    
    
    always @(*) begin
        full_adder(A[0], B[0], cin,    s0, carry0);
        full_adder(A[1], B[1], carry0, s1, carry1);
        full_adder(A[2], B[2], carry1, s2, carry2);
        full_adder(A[3], B[3], carry2, s3, carry3);
    end
    
   
    assign SUM[0] = s0;
    assign SUM[1] = s1;
    assign SUM[2] = s2;
    assign SUM[3] = s3;
    assign cout = carry3;
    
endmodule
```

### **Test bench 4-bit Ripple Carry Adder using Task**
```
`timescale 1ns / 1ps
module tb_ripple_carry_adder_4bit();
    reg [3:0] A, B;
    reg cin;
    wire [3:0] SUM;
    wire cout;
    
    ripple_carry_adder_4bit uut (A, B, cin, SUM, cout);
    
    initial begin
        $monitor("Time=%0t | A=%b | B=%b | Cin=%b | Sum=%b | Cout=%b", $time, A, B, cin, SUM, cout);
        
        A = 4'b0001; B = 4'b0010; cin = 0; #10;
        A = 4'b1111; B = 4'b0001; cin = 0; #10;
        A = 4'b1010; B = 4'b0101; cin = 0; #10;
        A = 4'b1111; B = 4'b1111; cin = 1; #10;
        
        $finish;
    end
endmodule
```
### 4-bit Ripple Carry Adder Simulation Output 


<img width="1920" height="1200" alt="Screenshot From 2025-11-12 09-25-35" src="https://github.com/user-attachments/assets/0980b654-4c83-496e-a34a-6bc77ab90bf4" />








### **4-bit Ripple Counter using Function**
```
module ripple_counter_4bit(clk, rst, q);
input clk, rst;
output reg [3:0] q;

function [3:0] i;
input [3:0] data;
begin
i = data + 1;
end
endfunction

always @(posedge clk or posedge rst) begin
if (rst)
q <= 4'b0000;
else
q <= i(q);
end
endmodule






```
### **Testbench for 4-bit Ripple Counter using Function**
```
`timescale 1ns / 1ps
module tb_ripple_counter_4bit;
reg clk, rst;
wire [3:0] q;
ripple_counter_4bit uut(clk, rst, q);
always #5 clk = ~clk;
initial begin
$monitor("T=%0t | rst=%b | q=%b (%0d)", $time, rst, q, q);
clk = 0;
rst = 1;  
#10 rst = 0; 
#100;
$finish;
end
endmodule
```
### 4-bit Ripple Counter Simulation output 
 <img width="1920" height="1200" alt="Screenshot From 2025-11-12 09-34-55" src="https://github.com/user-attachments/assets/82d96559-35f9-48dc-a822-1e60262fe780" />








### Result

The simulation of the 4-bit Ripple Carry Adder using Task and 4-bit Ripple Counter using Function was successfully carried out in Vivado Design Suite.
Both designs produced correct functional outputs as verified by waveform and console output.
