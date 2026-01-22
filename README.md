# fifo-uvm-verification`timescale 1ns/1ps

module fifo_tb;

    logic clk;
    logic rst_n;
    logic wr_en;
    logic rd_en;
    logic [7:0] wr_data;
    logic [7:0] rd_data;
    logic full;
    logic empty;

    // DUT instantiation
    fifo dut (
        .clk(clk),
        .rst_n(rst_n),
        .wr_en(wr_en),
        .rd_en(rd_en),
        .wr_data(wr_data),
        .rd_data(rd_data),
        .full(full),
        .empty(empty)
    );

    // Clock generation
    always #5 clk = ~clk;

    initial begin
        // Initialize
        clk = 0;
        rst_n = 0;
        wr_en = 0;
        rd_en = 0;
        wr_data = 0;

        // Apply reset
        #10 rst_n = 1;

        // Write data into FIFO
        repeat (4) begin
            @(posedge clk);
            wr_en = 1;
            wr_data = $random;
        end
        wr_en = 0;

        // Read data from FIFO
        repeat (4) begin
            @(posedge clk);
            rd_en = 1;
        end
        rd_en = 0;

        #20 $finish;
    end

endmodule
