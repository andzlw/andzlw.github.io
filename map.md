# Register renaming

https://en.wikipedia.org/wiki/Register_renaming   
Read-after-write (RAW)
a read from a register or memory location must return the value placed there by the last write in program order, not some other write. This is referred to as a true dependency or flow dependency, and requires the instructions to execute in program order.

Write-after-write (WAW)
successive writes to a particular register or memory location must leave that location containing the result of the second write. This can be resolved by squashing (also known as cancelling, annulling, or mooting) the first write if necessary. WAW dependencies are also known as output dependencies.

Write-after-read (WAR)
a read from a register or memory location must return the last prior value written to that location, and not one written programmatically after the read. This is a sort of false dependency that can be resolved by renaming. WAR dependencies are also known as anti-dependencies.

## Architectural versus physical registers
Machine language programs specify reads and writes to a limited set of registers specified by the instruction set architecture (ISA). For instance, the Alpha ISA specifies 32 integer registers, each 64 bits wide, and 32 floating-point registers, each 64 bits wide. These are the architectural registers. Programs written for processors running the Alpha instruction set will specify operations reading and writing those 64 registers. If a programmer stops the program in a debugger, they can observe the contents of these 64 registers (and a few status registers) to determine the progress of the machine.

One particular processor which implements this ISA, the Alpha 21264, has 80 integer and 72 floating-point physical registers. There are, on an Alpha 21264 chip, 80 physically separate locations which can store the results of integer operations, and 72 locations which can store the results of floating point operations (In fact, there are even more locations than that, but those extra locations are not germane to the register renaming operation.)

The following text describes two styles of register renaming, which are distinguished by the circuit which holds the data ready for an execution unit.

In all renaming schemes, the machine converts the architectural registers referenced in the instruction stream into tags. Where the architectural registers might be specified by 3 to 5 bits, the tags are usually a 6 to 8 bit number. The rename file must have a read port for every input of every instruction renamed every cycle, and a write port for every output of every instruction renamed every cycle. Because the size of a register file generally grows as the square of the number of ports, the rename file is usually physically large and consumes significant power.

In the tag-indexed register file style, there is one large register file for data values, containing one register for every tag. For example, if the machine has 80 physical registers, then it would use 7 bit tags. 48 of the possible tag values in this case are unused.

In this style, when an instruction is issued to an execution unit, the tags of the source registers are sent to the physical register file, where the values corresponding to those tags are read and sent to the execution unit.

In the reservation station style, there are many small associative register files, usually one at the inputs to each execution unit. Each operand of each instruction in an issue queue has a place for a value in one of these register files.

In this style, when an instruction is issued to an execution unit, the register file entries corresponding to the issue queue entry are read and forwarded to the execution unit.
