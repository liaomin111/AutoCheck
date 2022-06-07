AutoCheck
====
AutoCheck is a dependency analysis tool, which aims to autmatically identify objects for checkpointing for large-scale machine learning models and HPC applications.

Docker setup
====
We *highly* recommend that the reviewers use the docker container for NO prerequisite 
software installation, which could take a few hours to setup.

1. Docker Pull Command
```
docker pull abclhm/auto-check:latest
```
2. Create container
```
docker run -it abclhm/auto-check:latest /bin/bash
```

Requirements:
-------------------
We *highly* recommend that users use the provided Docker image, available for
download [here](https://hub.docker.com/repository/docker/abclhm/auto-check).
This will solve basically all environment issues, and, the it holds all the examples we 
used for testing (including the generated trace). If you cannot use Docker,
then read on. Also, we cover Docker installation and using AutoCheck in detail below.
  1. LLVM-Tracer 1.2 (available for download [here](https://github.com/harvard-acc/LLVM-Tracer/tree/llvm-3.4), and follow
the instructions on the page to install).
  2. CMake 3.5.1 or newer.
  3. graphviz 2.38.0 or newer (available for download [here](https://graphviz.org)).

Build:
-------------------
  1. Clone AutoCheck
```
  git clone https://github.com/liaomin111/AutoCheck.git
  cd AutoCheck/
```
  2. Install
```
  mkdir build/
  cd build
  cmake ..
  make
```

Run:
------
We provide two scripts for testing (auto.sh and run.sh). With the auto.sh script 
all experiments performed in our paper are automatically executed and the results are  stored in the result directory (/workspace/AutoCheck/build/result). 
With the run.sh script, you can follow the script prompts to enter the trace file location and the information about the location of the 
main computation loop to perform a single benchmark test, and the results will be  stored in the result directory.

Test with auto.sh:
------
1. Go to Autocheck 
```
cd /workspace/AutoCheck/build
```
2. Execute auto.sh
```
./auto.sh
```
Test with run.sh:
------
Example benchmark :Himeno
1. Go to Autocheck 
```
cd /workspace/AutoCheck/build
```
2. Execute run.sh
```
./run.sh
```
3. Enter the name of the trace file to be tested
```
himeno
```
4. Enable or disable OpenMP acceleration (on or off)
```
off
```
5. Enter the name of the function where the main computation loop is located
```
jacobi
```
6. Enter the location where the function for which the main calculation loop exists is called
```
116
```
7. Enter the line number at the beginning of the main calculation loop
```
186
```
8. Enter the line number of the end of the main calculation loop
```
217
```

Benchmark's main loop location
------------

Benchmark | function | call location | start line | end line
-----| -----| -----| -----| -----
Himeno | jacobi | 116 | 186 | 217
HPCCG | HPCCG | 138 | 118 | 146
CG | main | 0 | 296 | 330
MG | main | 0 | 266 | 276
FT | appft | 74 | 109 | 119
SP | main | 0 | 184 | 190
EP | main | 0 | 168 | 213
IS | main | 0 | 732 | 736
BT | main | 0 | 180 | 186
LU | ssor | 205 | 115 | 267
CoMD | main | 0 | 180 | 186
miniAMR | ssor | 205 | 115 | 267

Validation:
------------
We provide a homemade library with C/R functionality, and we add C/R code to all 10 benchmarks. 
When executing our modified program, only need to add one parameter (0 to start the program 
normally, 1 to start the program from the checkpoint file) to invoke the C/R functionality.

To verify the correctness of the variables provided by AutoCheck for checkpointing, we run the same 
benchmark three times (twice with the C/R code added and once with the original benchmark).
First, we execute the program with the C/R code which will be interrupted by the fail-stop failure we simulated, 
then we restart the program with the data of the critical variables we stored in memory. 
Finally, we execute the original program and verify whether the program restarted successfully by comparing the 
execution result of the original program with the result of the restarted program.

Example benchmark :Himeno
------------------------------------
1. Go to Himeno's (with C/R enable)  working directory and compile the executable file
```
cd /workspace/Benchmarks/checkpoint/himeno/build

cmake .. && make
```
2. Execute himeno
```
./himeno 0
```
3. Restart himeno
```
./himeno 1
```
4. Go to the working directory of the original Himeno program and compile it to get the executable file
```
cd /workspace/Benchmarks/varify/himeno && make
```
5. Execute himeno
```
./himeno
```



