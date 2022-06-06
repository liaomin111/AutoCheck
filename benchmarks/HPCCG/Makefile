export WORKLOAD=main
TRACER_HOME=/workspace/LLVM-Tracer
#TRACER_HOME=/home/azguolu/Projects/resiliencePatternHunting/LLVM_Tracer_Work_Station/llvm-tracer

SRCS = main.cpp generate_matrix.cpp read_HPC_row.cpp  \
          compute_residual.cpp mytimer.cpp dump_matlab_matrix.cpp \
          HPC_sparsemv.cpp HPCCG.cpp waxpby.cpp ddot.cpp \
          exchange_externals.cpp \
          
EXEC = hpccg
CC = mpicxx
MPI_INC = -I /usr/local/openmpi/include
MPI_LIB = -I /usr/local/openmpi/lib -lmpi
#CPPFLAGS = $(MPI_INC) $(MPI_LIB)
CPPFLAGS =
LOGGER = $(TRACER_HOME)/lib/trace_logger.llvm
TRACER = $(TRACER_HOME)/lib/full_trace.so

LLVM_OBJS = $(patsubst %.cpp, %-opt.llvm, $(SRCS))

$(EXEC)-instrumented: full.s
	$(CC) -O0 -fno-inline -o $@ $< -lm -lz

full.s: full.llvm
	llc -O0 -disable-fp-elim -filetype=asm -o $@ $<

full.llvm: $(LLVM_OBJS)
	llvm-link -o $@ $^ $(LOGGER)

%-opt.llvm: %.cpp
	clang -g -O0 -S -static -emit-llvm  $(CPPFLAGS)  -o $*.llvm $<
	opt -S -load=$(TRACER) -fulltrace -trace-all-callees $*.llvm -o $@

clean:
	rm -f *.ll* *.s *.core *-instrumented *.gz
