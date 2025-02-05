### CODE

// Code your design here
module pipeline_processor (
    input clk,
    input reset,
    input [31:0] instruction, // Instruction input
    output reg [31:0] result  // Final result after Write-back stage
);

// Pipeline Registers for storing intermediate results at each stage
reg [31:0] IF_ID_instruction;
reg [31:0] ID_EX_instruction;
reg [31:0] EX_MEM_instruction;
reg [31:0] MEM_WB_instruction;

reg [31:0] IF_ID_result;
reg [31:0] ID_EX_result;
reg [31:0] EX_MEM_result;
reg [31:0] MEM_WB_result;

// Stage updates (each stage gets data from the previous)
always @(posedge clk or posedge reset) begin
    if (reset) begin
        IF_ID_instruction <= 32'b0;
        ID_EX_instruction <= 32'b0;
        EX_MEM_instruction <= 32'b0;
        MEM_WB_instruction <= 32'b0;
    end else begin
        IF_ID_instruction <= instruction;
        ID_EX_instruction <= IF_ID_instruction;
        EX_MEM_instruction <= ID_EX_instruction;
        MEM_WB_instruction <= EX_MEM_instruction;
    end
end

// Processing instructions at each stage
always @(posedge clk or posedge reset) begin
    if (reset) begin
        IF_ID_result <= 32'b0;
        ID_EX_result <= 32'b0;
        EX_MEM_result <= 32'b0;
        MEM_WB_result <= 32'b0;
    end else begin
        // Example operation in each stage
        IF_ID_result <= instruction;
        ID_EX_result <= IF_ID_result + 1;  // Example ADD
        EX_MEM_result <= ID_EX_result - 1;  // Example SUB
        MEM_WB_result <= EX_MEM_result & 32'hFFFF;  // Example AND
    end
end

// Output the result from the MEM_WB stage
always @(posedge clk or posedge reset) begin
    if (reset) begin
        result <= 32'b0;
    end else begin
        result <= MEM_WB_result;
    end
end

endmodule


### TESTBENCH


// Code your testbench here
// or browse Examples
module testbench;

// Declare inputs and outputs
reg clk;
reg reset;
reg [31:0] instruction;
wire [31:0] result;

// Instantiate the pipeline processor
pipeline_processor uut (
    .clk(clk),
    .reset(reset),
    .instruction(instruction),
    .result(result)
);

// Generate clock signal
always begin
    #5 clk = ~clk; // 10ns clock period
end

// Initialize inputs and apply stimulus
initial begin
    // Initialize signals
    clk = 0;
    reset = 1;
    instruction = 32'h00000000;

    // Apply reset for a few cycles
    #10 reset = 0;
    
    // Apply a few instructions to the pipeline
    instruction = 32'h00000001; // First instruction
    #10 instruction = 32'h00000002; // Second instruction
    #10 instruction = 32'h00000003; // Third instruction
    #10 instruction = 32'h00000004; // Fourth instruction
    
    // Finish simulation after some cycles
    #100 $finish;
end

// Monitor results
initial begin
    $monitor("Time=%0t | Instruction=0x%h | Result=0x%h", $time, instruction, result);
end

// VCD generation
initial begin
    $dumpfile("waveform.vcd");  // Create a VCD file named waveform.vcd
    $dumpvars(0, testbench);    // Dump all variables in the testbench to the VCD file
end

endmodule
