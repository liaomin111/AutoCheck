Docker setup
====
We provide a docker image with AutoCheck pre-installed.

1. Docker Pull Command
```
docker pull abclhm/auto-check:latest
```
2. Create container
```
docker run -it --name autocheck  autocheck /bin/bash
```
Run:
------
We provide two scripts for testing (auto.sh and run.sh). Using the auto.sh script 
all experiments performed in our paper are automatically executed and the results are stored in result directory. 
Using run.sh, you can follow the script prompts to enter the trace file location and the information about the location of the 
main computation loop to perform a single benchmark test, and the results will be stored in the results directory.

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
./auto.sh
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
jacobi
```
5. Enter the location where the function for which the main calculation loop exists is called
116
```
6. Enter the line number at the beginning of the main calculation loop
186
```
7. Enter the line number of the end of the main calculation loop
217
```