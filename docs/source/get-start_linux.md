# Start with the Docker Development Environment

## Why use docker development environment

PikaPython's kernel and standard libraries are developed in a docker environment, which can be prone to some hard-to-debug problems when developing features that involve the kernel internals, such as

- memory leaks
- memory overruns
- broken kernel functionality

This problem can be avoided by using PikaPython's docker development environment, which has been installed with **unit testing framework** and **memory checking tool**, so that if there is a memory security problem, it can be quickly found and solved to avoid memory hazards.

PikaPython's linux development platform also needs to install go, rust, GoogleTest, GoogleBenchmark, valgrind and other tools, which is rather cumbersome, Docker-based development environment can install these tools in one click, and ensure that all developers' development environment is consistent.


## Build Docker container

Please make sure you have installed Docker on the host:

- Install Docker directly on Linux platform
- Install Docker-Desktop on Windows platform
  - Docker-Desktop requires the installation of wsl2 

(For windows platform, you can use the following command in wsl, not PowerShell)

step1: Clone the repository

``` shell
git clone https://github.com/pikastech/pikascript
cd pikascript/docker 
```

step2: Build the Docker image, then start the container
```
bash build.sh
sh run.sh
```

step3: Initialize the port/linux

``` shell	
cd port/linux
sh pull-core.sh
sh init.sh
```

step4: Run GoogleTest, BenchMark, and valgrind
``` shell
sh gtest.sh
sh ci_benchmark.sh
sh valgrind.sh
```

step5: Run REPL
``` shell
sh run.sh
```

For more development guidelines under Docker, please refer to [ Development Process for Standard Libraries ](contribute_to_stdlib.html).

