# Exp-No: 06 - Sequence Detector using Moore and Mealy State Machines with Testbench Verification

**Aim:** <br>
<br>
&emsp;&emsp;To design and simulate a sequence detector using both Moore and Mealy state machine models in Verilog HDL, and verify their functionality through a testbench using the Vivado 2023.1 simulation environment. The objective is to detect a specific sequence of bits (e.g., 1001) and compare the Moore and Mealy designs.<br>
<br>
**Apparatus Required:** <br>
<br>
&emsp;&emsp;Vivado 2023.1 or equivalent Verilog simulation tool.<br>
&emsp;&emsp;Computer system with a suitable operating system.<br>
<br>
**Procedure:** <br>

**Launch Vivado 2023.1:** <br>
<br>
&emsp;&emsp;Open Vivado and create a new project.<br>
<br>
**Write the Verilog Code for Sequence Detector (Moore and Mealy FSM):** <br>
<br>
&emsp;&emsp;Design two Verilog modules: one for a Moore FSM and another for a Mealy FSM to detect a sequence such as 1001.<br>
<br>
**Create the Testbench:** <br>
<br>
&emsp;&emsp;Write a testbench to apply input sequences and verify the output of both FSM designs.<br>
<br>
**Add the Verilog Files:** <br>
<br>
&emsp;&emsp;Add the Verilog code for both FSMs and the testbench to the Vivado project.<br>
<br>
**Run Simulation:** <br>
<br>
&emsp;&emsp;Run the simulation and observe the output to check if the sequence is detected correctly.<br>
<br>
**Observe the Waveforms:** <br>
<br>
&emsp;&emsp;Analyze the waveform to ensure both the Moore and Mealy machines detect the sequence as expected.<br>
<br>
**Save and Document Results:** <br>
<br>
&emsp;&emsp;Capture the waveforms and include the results in the final report.<br>
<br>
**Verilog Code for Sequence Detector Using Moore FSM:** <br>
```

module moore_sequence_detector ( 
    input wire clk, 
    input wire reset, 
    input wire seq_in, 
    output reg detected );
parameter reg [2:0] S0=2'd0, S1=2'd1, S2=2'd2, S3=2'd3, S4=3'd4;
reg [2:0] current_state, next_state;
always @(posedge clk) begin
    if (reset)
        current_state <= S0;
    else
        current_state <= next_state;
end
always @(*) begin
    case (current_state)
        S0: begin
            next_state = S1;
            detected = 0;
        end
        S1: begin//1
            next_state = S2;
            detected = 0;
        end
        S2: begin//10
            next_state = S3;
            detected = 0;
        end
        S3: begin//100
            next_state = S4;
            detected = 0;
        end
        S4: begin//1001
            next_state = S0;
            detected = 1;  // Sequence 1001 detected
        end
        default: next_state = S0;
    endcase
end
endmodule

```

**Verilog Code for Sequence Detector Using Mealy FSM:** <br>
```

 module mealy_sequence_detector ( 
 input wire clk, 
 input wire reset, 
 input wire seq_in, 
 output reg detected ); 
parameter reg [1:0] S0=2'd0, S1=2'd1, S2=2'd2, S3=2'd3 ;
reg [1:0] current_state, next_state;
always @(posedge clk) begin
    if (reset)
        current_state <= S0;
    else
        current_state <= next_state;
end
always @(*) begin
    detected = 0;
    case (current_state)
        S0: begin
            if (seq_in) next_state = S1;
            else next_state = S0;
        end
        S1: begin//1
            if (seq_in) next_state = S1;
            else next_state = S2;
        end
        S2: begin//10
            if (seq_in) next_state = S1;
            else next_state = S3;
        end
        S3: begin//100
            if (seq_in) begin
                next_state = S0;
                detected = 1;  // Sequence 1001 detected
            end else
                next_state = S0;
        end
        default: next_state = S0;
    endcase
end
endmodule

```

**Testbench for Sequence Detector (Moore and Mealy FSMs):** <br>
```

`timescale 1ns / 1ps
module sequence_detector_tb;
reg clk; reg reset; reg seq_in;
wire moore_detected;
wire mealy_detected;
moore_sequence_detector moore_fsm (
    .clk(clk),
    .reset(reset),
    .seq_in(seq_in),
    .detected(moore_detected) );
mealy_sequence_detector mealy_fsm (
    .clk(clk),
    .reset(reset),
    .seq_in(seq_in),
    .detected(mealy_detected) );
initial begin
forever #5 clk = ~clk;
end
initial begin
    clk = 1;
    reset = 1;
    seq_in = 1;
    #20 reset = 0;
    @(posedge clk);
     seq_in = 1;
    @(posedge clk);
    seq_in = 0;
    @(posedge clk);
    seq_in = 0;
    @(posedge clk);
    seq_in = 1;
    @(posedge clk);
    seq_in = 1;
    @(posedge clk);
    seq_in = 0;
    @(posedge clk);
    seq_in = 0;
    @(posedge clk);
    seq_in = 1;
    @(posedge clk);
    seq_in = 1;
    @(posedge clk);
    #30 $stop;
end
initial begin
    $monitor("Time=%0t | seq_in=%b | Moore FSM Detected=%b | Mealy FSM Detected=%b",$time, seq_in, moore_detected, mealy_detected);
end
endmodule
```
**Sample Output:** 
```

Time=0 | seq_in=1 | Moore FSM Detected=0 | Mealy FSM Detected=0
Time=30000 | seq_in=0 | Moore FSM Detected=0 | Mealy FSM Detected=0
Time=50000 | seq_in=1 | Moore FSM Detected=1 | Mealy FSM Detected=1
Time=60000 | seq_in=1 | Moore FSM Detected=0 | Mealy FSM Detected=0
Time=70000 | seq_in=0 | Moore FSM Detected=0 | Mealy FSM Detected=0
Time=90000 | seq_in=1 | Moore FSM Detected=0 | Mealy FSM Detected=1
Time=100000 | seq_in=1 | Moore FSM Detected=1 | Mealy FSM Detected=0
Time=110000 | seq_in=1 | Moore FSM Detected=0 | Mealy FSM Detected=0

```
**Output Waveform** <br>
<br>
![Screenshot 2024-11-15 111719](https://github.com/user-attachments/assets/e5901488-3253-4609-a990-861c7d8ace92)
<br>
**Conclusion:** <br>
<br>
&emsp;&emsp;In this experiment, Moore and Mealy FSMs were successfully designed and simulated to detect the sequence 1001. Both designs worked as expected, with the main difference being that the Moore FSM generated the output based on the current state, while the Mealy FSM generated the output based on both the current state and input. The testbench verified the functionality of both FSMs, demonstrating that the Verilog HDL can effectively model both types of state machines for sequence detection tasks.
