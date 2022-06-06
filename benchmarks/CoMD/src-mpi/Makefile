export WORKLOAD=main
TRACER_HOME=/workspace/LLVM-Tracer

# double precision (ON/OFF)
DOUBLE_PRECISION = OFF
# MPI for parallel (ON/OFF)
DO_MPI = OFF

CC = gcc
CFLAGS = -std=c99
OPTFLAGS = -g -O0
INCLUDES =
C_LIB = -lm

CoMD_VARIANT = CoMD-serial

ifeq ($(DO_MPI), ON)
CoMD_VARIANT = CoMD-mpi
INCLUDES += ${MPI_INCLUDE}
CFLAGS += -DDO_MPI
LDFLAGS += ${MPI_LIB}
endif

SRCS = $(wildcard *.c)
EXEC = ${CoMD_VARIANT}
CPPFLAGS = # Put include directories here
LOGGER = $(TRACER_HOME)/lib/trace_logger.llvm
TRACER = $(TRACER_HOME)/lib/full_trace.so

LLVM_OBJS = $(patsubst %.c, %-opt.llvm, $(SRCS))

$(EXEC)-instrumented: full.s
	$(CC) -O0 -fno-inline -o $@ $< -lm -lz

full.s: full.llvm
	llc -O0 -disable-fp-elim -filetype=asm -o $@ $<

full.llvm: $(LLVM_OBJS)
	llvm-link -o $@ $^ $(LOGGER)

%-opt.llvm: %.c
	clang -g -O0 -S -static -emit-llvm $(CFLAGS) ${LDFLAGS} -o $*.llvm $<
	opt -S -load=$(TRACER) -fulltrace -trace-all-callees $*.llvm -o $@

CoMD_info.h: Makefile
	./generate_info_header ${CoMD_VARIANT} "$(CC)" "$(CFLAGS)" "$(LDFLAGS)"

clean:
	rm -f *.ll* *.s *.core *-instrumented *.gz
