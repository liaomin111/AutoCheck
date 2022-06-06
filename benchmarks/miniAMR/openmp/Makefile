export WORKLOAD=main
TRACER_HOME=/workspace/LLVM-Tracer

SRCS = block.c check_sum.c comm_block.c comm.c comm_parent.c comm_refine.c \
       comm_util.c driver.c init.c main.c move.c pack.c plot.c profile.c \
       rcb.c refine.c stencil.c util.c
EXEC = bmt
CPPFLAGS = -I /usr/local/openmpi/include
LOGGER = $(TRACER_HOME)/lib/trace_logger.llvm
TRACER = $(TRACER_HOME)/lib/full_trace.so
CC = mpicc
LLVM_OBJS = $(patsubst %.c, %-opt.llvm, $(SRCS))

$(EXEC)-instrumented: full.s
	$(CC) -O0 -fno-inline -o $@ $< -lm -lz

full.s: full.llvm
	llc -O0 -disable-fp-elim -filetype=asm -o $@ $<

full.llvm: $(LLVM_OBJS)
	llvm-link -o $@ $^ $(LOGGER)

%-opt.llvm: %.c
	clang -g -O0 -S -static -emit-llvm $(CPPFLAGS) -o $*.llvm $<
	opt -S -load=$(TRACER) -fulltrace -trace-all-callees $*.llvm -o $@

clean:
	rm -f *.ll* *.s *.core *-instrumented *.gz
