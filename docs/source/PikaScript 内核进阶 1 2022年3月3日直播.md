# PikaPython kernel advanced

[Video link](https://www.bilibili.com/video/BV12Z4y167SP)

## outline
1. Overview of kernel development

Second, the construction of the kernel development environment

3. Test Driven Development

4. Kernel distribution and upstream and downstream

## Kernel development overview
Kernel development environment: linux

Kernel deployment environment: mcu ( ARM, Risc-V, Others )
​

### Reasons to choose linux
#### Kernel requirements: cross-platform capability, stability
Only cross-platform, cross-platform
Only fully tested can it be stable
​

#### Development requirements: mainstream platform, convenient debugging, complete testing tools
Use mainstream platforms, mainstream technologies, and build only one wheel at a time


#### Team requirements: Avoid relying on hardware, unified development environment
Reduce the difficulty of joining new members and solve the obstacle of physical distance (the express fee is very expensive)
Reduce the cost of trial and error (what should I do if the hardware test board is burned)
Simplify the construction of the development environment (why can't your software be used on my computer)


#### Project requirements: easy to deploy automation facilities, CI-CD, easy software distribution
Automate all steps that can be automated to reduce maintenance costs

### Kernel development steps

![](assets/yuque_diagram.jpg)

95% of the workload before the real machine test
​

## Kernel environment construction


## Test Driven Development
Implement functionality -> write unit tests
## Kernel distribution and upstream and downstream