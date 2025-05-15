# PIPELINE-PROCESSOR-DESIGN1

*COMPANY*: CODTECH IT SOLUTIONS 

*NAME*: KARANAM UDAYKUMAR 

*INTERN ID*: CT12WX115

*DOMAIN*: VLSI

*DURATION*: 3 MONTHS 

*MENTOR*: NEELA SANTOSH 


### DESCRIPTION FOR 4 STAGE PIPELINED PROCESSOR DESIGN  


## Four-Stage Pipeline Processor Design
The four-stage pipeline processor is designed to execute basic instructions like ADD, SUB, and LOAD. The pipeline stages are:
![2](https://github.com/user-attachments/assets/5f9be9e1-ec2f-4735-a14a-15f01b954c95)

1. Instruction Fetch (IF): Fetches instructions from memory.
2. Instruction Decode (ID): Decodes the instruction and retrieves operands.
3. Execution (EX): Executes the instruction (ADD, SUB, etc.).
4. Memory Access (MA): Accesses memory for LOAD instructions.

## Design Description
The processor will have the following components:

- Instruction Memory: Stores the program instructions.
- Register File: Stores the processor registers.
- ALU: Performs arithmetic and logical operations.
- Data Memory: Stores data for LOAD instructions.

## Instruction Set Architecture (ISA)
The ISA will include the following instructions:

- ADD Rd, Rs, Rt: Rd = Rs + Rt
- SUB Rd, Rs, Rt: Rd = Rs - Rt
- LOAD Rd, address: Rd = Mem[address]

## Pipeline Stage Operations
### Instruction Fetch (IF)
- Fetches the instruction from instruction memory.
- Increments the program counter (PC) to point to the next instruction.
![1](https://github.com/user-attachments/assets/bba18590-afda-4335-8e13-410ca84ccac6)

### Instruction Decode (ID)
- Decodes the instruction and retrieves the operands from the register file.
- Determines the operation to be performed (ADD, SUB, LOAD, etc.).

### Execution (EX)
- Performs the arithmetic or logical operation (ADD, SUB, etc.).
- Calculates the memory address for LOAD instructions.

### Memory Access (MA)
- Accesses data memory for LOAD instructions.
- Retrieves the data from memory and writes it to the register file.


### CODE

module pipelined_processor(
    input clk,
    input reset
);

reg [31:0] pc;
reg [31:0] instruction;
reg [31:0] reg_file [0:31];

wire [5:0] opcode;
wire [4:0] rs, rt, rd;
wire [15:0] immediate;

assign opcode = instruction[31:26];
assign rs = instruction[25:21];
assign rt = instruction[20:16];
assign rd = instruction[15:11];
assign immediate = instruction[15:0];

reg [31:0] if_id_instruction;
reg [31:0] id_ex_rs, id_ex_rt;
reg [4:0] id_ex_rd;
reg [15:0] id_ex_immediate;
reg [31:0] ex_wb_result;
reg [4:0] ex_wb_rd;

always @(posedge clk or posedge reset) begin
    if (reset) begin
        pc <= 0;
        // Initialize register file
        for (int i = 0; i < 32; i++) begin
            reg_file[i] <= 0;
        end
    end else begin
       
        
        // Fetch stage
        instruction <= memory[pc];
        pc <= pc + 4;

        // Decode stage
        if_id_instruction <= instruction;
        id_ex_rs <= reg_file[rs];
        id_ex_rt <= reg_file[rt];
        id_ex_rd <= rd;
        id_ex_immediate <= immediate;

        // Execute stage
        case (opcode)
            6'b100000: // ADD
                ex_wb_result <= id_ex_rs + id_ex_rt;
            6'b10001
            
            0: // SUB
                ex_wb_result <= id_ex_rs - id_ex_rt;
            default:
                ex_wb_result <= 0;
        endcase
        ex_wb_rd <= id_ex_rd;

        // Write Back stage
        reg_file[ex_wb_rd] <= ex_wb_result;
    end
end

endmodule

//TESTBENCH

module pipelined_processor_tb;

reg clk;
reg reset;

pipelined_processor u_processor(
    .clk(clk),
    .reset(reset)
);

initial begin
    clk = 0;
    reset = 1;
    #100 reset = 0;
end

always #5 clk = ~clk;

initial begin
    #1000 $finish;
end

endmodule




## Simulation
The simulation will demonstrate the execution of a sample program that includes ADD, SUB, and LOAD instructions. The simulation will show the pipeline stages operating in sequence, with each stage performing its designated function.


## Example Use Case
Suppose we want to execute the following program:

1. LOAD R1, 0x1000
2. LOAD R2, 0x1004
3. ADD R3, R1, R2
4. SUB R4, R3, R1

The simulation will show the pipeline stages operating in sequence, with each stage performing its designated function. The final result will be stored in register R4.

By designing and simulating a four-stage pipeline processor, we can demonstrate the basic principles of pipelining and instruction-level parallelism. This design can be extended to more complex processors with additional pipeline stages and instructions.



## OUTPUT

![Image](https://github.com/user-attachments/assets/e377a4fc-6185-4edf-b07d-656d06effc88)

