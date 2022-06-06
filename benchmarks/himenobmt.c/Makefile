
export WORKLOAD=main
TRACER_HOME=/workspace/LLVM-Tracer
#TRACER_HOME=/home/azguolu/Projects/resiliencePatternHunting/LLVM_Tracer_Work_Station/llvm-tracer

SRCS = himenoBMT.c
EXEC = bmt
CPPFLAGS = # Put include directories here
LOGGER = $(TRACER_HOME)/lib/trace_logger.llvm
TRACER = $(TRACER_HOME)/lib/full_trace.so

LLVM_OBJS = $(patsubst %.c, %-opt.llvm, $(SRCS))
MODEL = SMALL

$(EXEC)-instrumented: full.s
	        $(CC) -O0 -fno-inline -o $@ $< -lm -lz

full.s: full.llvm
	        llc -O0 -disable-fp-elim -filetype=asm -o $@ $<

full.llvm: $(LLVM_OBJS)
	        llvm-link -o $@ $^ $(LOGGER)

%-opt.llvm: %.c
	        clang -g -O0 -S -static \
			        -emit-llvm -D$(MODEL) \
				        $(CPPFLAGS) \
					        -o $*.llvm $<
		        opt -S -load=$(TRACER) -fulltrace -trace-all-callees $*.llvm -o $@

clean:
	        rm -f *.ll* *.s *.core *-instrumented *.gz
