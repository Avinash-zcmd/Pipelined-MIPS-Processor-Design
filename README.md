
# Pipelined-MIPS-Processor-with-MULADD-and-Performance-Monitor

# Description:
A 5-stage (Fetch, Decode, Execute, Memory, Writeback) pipelined MIPS processor in SystemVerilog with data forwarding, branch flushing and stalling, extended with a custom MULADD multiply accumulate instruction and a hardware performance monitor (perfmon) that reads the cycle and instruction counters.

# Problem Statement:
1. Adding Pipeline registers can raises the clock frequency but creates hazards
2. Dependent instruction can read a register before the previous one has written it back, and a branch is resolved after the next instructions have already been fetched.
This project addresses the need for a pipeline design that gives correct results without stalling every dependency, and shows how its performance can be measured in hardware.

# My Work:
1. Designed the 5-stage pipelined MIPS in SystemVerilog (controller, datapath, instruction and data memory) supporting add, sub, and, or, slt, addi, lw, sw, beq and j instructions.
2. Implemented data forwarding into the Execute stage from the Memory and Writeback stages (ForwardAE/BE), a one cycle stall for load use hazards, and a 3 instruction flush for taken BEQ branches (flushD/flushE).
3. Added the MULADD instruction ($rd = $rs*$rt + $rd, funct 0x1C) with a third register file read port and its own forwarding path (ForwardCE) for the accumulator.
4. Added the perfmon $rt, flag instruction (opcode 0x1D) with a performance monitor module
   ** flag 0 reads the cycle counter
   **flag 1 reads the instruction counter, with stall bubbles and flushed instructions excluded from the count.
5. Verified the design with SystemVerilog testbench that compares every memory write (address and data) against expected values, using 7 directed tests (main, forwarding, BEQ flush, load use, MULADD, perfmon, combined) and reporting CPI and IPC 0.743 on the main 26 instruction program, 0.889 with forwarding, 0.723 with branch flushing.
6. Synthesized the design with flip flop memories and with SRAM macros SRAM_32x64_1rw in Synopsys Design Compiler using the OSU FreePDK45 library, and compared it with the non pipelined design.

# Tools Used:
SystemVerilog, Synopsys DesignCompiler, Synopsys VCS, Verdi
