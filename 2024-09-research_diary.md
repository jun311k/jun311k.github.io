---
layout: post
title:  "2024-09 research diary"
---
20240906
오늘 할 거
1. RISCV 실태 조사
2. gpu 구조 keyword 복습
3. arm 공부 TLB
4. chisel 공부
5. llama 논문 읽기
/*RISC-V 실태 조사 및 Chipyard 조사*/
-risc v란 커스텀 프로세서를 개발하기 위한 오픈 소스 isa이다. 주로 임베디드 디자인이나 수퍼컴에 많이 쓰인다.
오픈 소스이기 때문에 개발하기 편하고 호환하기 좋다.
막 하드웨어 구조를 다 오픈한다는 것이 아니라 쓰이는 RISC-V ISA를 open하는 것이기 때문에 ISA를 공통으로 Customize할 수 있다.
-주요 사용처는? 웨어러블, 산업, IoT, 가정기구(배터리 파워 측면)/스마트폰, 복잡한 계산을 커스터마이즈 ISA로 이용해 HPC에 적용
-2027년까지 35퍼센트 매년 성장률로 기대
-Customize->IP제공자/IP이용자(SoC 팀)
-Customize한 것이 이용되기 위해 유연하게 제작되어야 한다.
*출처: https://www.synopsys.com/glossary/what-is-risc-v.html

-RISC V가 arm보다 더 나아서 사용될 수 있느냐? 또한 open source인데 수입은 어디서?
특정 작업 최적화된 cpu만들기 위해 라이선스 측면에서 risc v 기반으로 프로세서를 만드는 것이 이득.
또한 오픈 소스는 개발 제품이 무료라는 것이 아니라 커스터마이즈한 프로세서가 본인이 개발한 또는 최적화한 스프트웨어와 호환될 수 있다는 점에서 완성된 제품을 이용할 수 있고 이를 수익화할 수 있다.
*출처:https://www.reddit.com/r/RISCV/comments/11mzbo7/arm_versus_riscv/

/*gpu 강의 1강*/
-Out of Order Processor
*gpt한테 물어봄
instruction window: 실행되기 전 명령어는 이 윈도우에서 대기. 여기서 의존성 없거나 미리 실행할 수 있는 명령어 찾아서 동시에 병렬적으로 실행한다.
데이터 의존성 없을 때 여러 명령어 동시에 병렬처리 이를 ILP(Instruction Level Parallelism)이라 한다. 의존성 관리를 위한 방법으로는 Register Renaming(두 명령어가 같은 reg 사용하는 것처럼 보일 때 가사의 reg를 사용해 의존성 해결(False dependency 해결)). Reorder Buffer를 이용해 순차적으로 명령어가 완료된 것처럼 보이도록 순서 조정하여 결과 출력
-주파수를 증가하면 소비 전력이 증가하는 이유: 동적 전력 P=f*CV^2 왜냐하면 주파수가 높아질수록 cpu가 더 많은 작업 처리하기 위해 더 자주 전류를 켜고 끄기 때문이다.(트랜지스터 전환 빈도가 높아진다.)

/*가상 메모리와 TLB*/
-가상 메모리: 물리 메모리보다 더 큰 메모리 공간을 사용할 수 있게 한다.
-TLB: 가상 주소를 물리 주소로 변환할 때 성능 저하를 줄이기 위한 캐시 메모리

20240909
1. Virtual Memory & TLB
2. Chisel and chipyard 조사 및 노션에 작성

/*Virtual memory...*/


/*Chipyard*/
-Firesim 이라는 게 있다.
-simulator로 gem5, spike라는 게 있다
-firtil이라는 것도 있다
- chipyard learning curve: configure a custom SoC

*출처:https://fires.im/asplos-2023-tutorial/

-Chisel, FIRRTL, Chipyard, Firesim 개요 모아 놓은 블로그
*출처: https://cafe.naver.com/plduser/17374?art=ZXh0ZXJuYWwtc2VydmljZS1uYXZlci1zZWFyY2gtY2FmZS1wcg.eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJjYWZlVHlwZSI6IkNBRkVfVVJMIiwiY2FmZVVybCI6InBsZHVzZXIiLCJhcnRpY2xlSWQiOjE3Mzc0LCJpc3N1ZWRBdCI6MTcyNTg2MjU2MzMyOH0.3cnQA0PkGS5DVzHOxFB3p8YR6HGYuG55M9UJ0o-4NII
rocc

-종상이의 조언: 1.12.3을 깔아서 saturn을 해라. 여기에는 vector extension

20240910
/*Chipyard*/
-chipyard 1.12.3 재설치 중
-saturn이 뭔지 조사해봐야 할듯
우선 RTL Generator에 속해 있는 녀석.
vector extension에 대해서 공부 해야 할듯
-verilator vs firesim
둘다 rtl simulation 할 수 있는 애들인데 전자는 sw simulator로써 좀 느리다. 후자는 Amazon EC2 F1 FPGA를 씀으로써 빠르다.

20240911
/*GPU paper #1*/
How GPUs Work
-Affine space: Vector space에서 포인트 도입하면 이때 부터 affine space라 부름. 즉 벡터에 포인트를 더하면 이때부터 affine space라 한다.
-이때 linear transform마냥 affine transform(y=Ax+b) 꼴을 y=Ax 꼴 처럼 linear transform으로 바꿀 수 없을까?->이를 homogeneous coordinate을 도입하여 바꿀 수 있다.
-gpu에서 파이프라이닝 과정

GPUs and the future of Parallel computing
-off die memory: 메모리가 칩 내에 있는 것(가령 캐시)이 아닌 외부에 존재하는 DRAM 같은 메인 메모리를 가리킨다. 칩에서 멀리 떨어져 있어 데이터 전송에 많은 시간, 에너지가 소모3

20240912
1. 논문 마저 읽기
2. arm tlb 마저 공부 다하기
3. Chipyard

/*

20240919

1. 오늘 배운 내용 복습 - 정보보호, gpu구조
2. risc v vector extension에 대해 공부
3. chipyard에 대해 공부
4. tlb 마무리

/*정보보호입문 복습*/
-가장 키포인트
8 design principles for computer security
Simplicity(KISS 원칙)->간단할수록 오류나 vulnerability 줄일 수 있다
Safe default: nudge이론에서 나왔듯이 default로 접근 불허, 필요시만 접근 허용
Open design: 비밀스러운게 아닌 공개된 설계 속에서 안전함 추구
Complete mediation: 시스템 모든 접근은 인증 및 권한 부여가 필
Least privilege: 최소한의 권한만 부여. 즉 필요때만 권한 부여하고 다 하면 권한 다시 내리기
Seperation of duties: 여러 요소에 과도한 권한이 아닌 역할별로 필요 권한만
Least common mechanism: 매커니즘 공유x
Ease-to-use(Psychological acceptability): user 사용 쉽도록해야 우회같은 거 안함

/*gpu 구조*/
-static pipeline은 diverged latency를 가지고 있다.
idverged latency는 주로 분기 명령 처리할 때 발생하는 지연, 또는 lw, sw에서도 cache miss에 대해서 발생. 분기가 발생시 gpu는 다른 경로의 스레드들을 순차적으로 처리해야 해서 실행 속도가 느려진다.
-RAW. WAR, WAW hazard: WAW hazard는 reg file의 개수 한계로 동일 variable을 쓰게 되는 것. data는 다르다
-dynamically controlled pipelining: decoding에서 FP와 integer reg file이 따로 있음, execution에서 integer alu, AGU(Addr generation unit), FP alu가 있음
-일단 static pipeline과 dynamically pipeline 차이가 뭘까? 정적 파이프라인은 마지막 입력 데이터가 파이프라인을 떠나고 난 후에야 연산 가능. 곱셈 다 한 후에 덧셈연산으로 전환. 하지만 후자는 queue라는 것을 단계별로 사용함으로써 밀리지 않게? 즉 product와 consume에 있어 시차가 있다. 전자는 고정되어 있기 때문에 divergenced latency가 발생하지만 후자는 queue에 저장해놓기 때문에 pipeline이 멈출 일이 없다.
*출처:https://cs.stackexchange.com/questions/71172/whats-the-difference-between-dynamic-and-static-pipelines
-tomasulo algorithm에서 WAR, WAW 해저드에 대해서 queue에 tag를 부여해 operands를 구별한다.

/*risc v vector extension*/
-terminology
XRF, VRF: integer reg file, vector reg file
SIMD: Single Instruction Multiple Data
SEW: Selected Element Width (8-64)
XLEN: Scalar register length in bits(64)
VLEN: Vector register length in bits (128-512)
LMUL: Register grouping multiple(1/8-8)
VLMAX: Vector length Max   vl: Vector length
configurable, extensible, standard extension
*출처: https://www.youtube.com/watch?v=oTaOd8qr53U

20240920

1. vector extension
2. chipyard
3. tlb

/*vector extension*/
-Vtype(SEW, LMUL), vl(vector 요소 얼마큼 사용할지)
-LMUL에 대해
-tail elements

/*chipyard*/

20240923

1. trust zone
2. vector extension
3. chipyard


/*trust zone*/
다 끝내고 미드텀 이그잼

/*Parallel Processing Unit*/

20240924
1. gpu구조, ai 경량화 복습
2. csd midexam
3. pps

/*gpu*/
-static pipelining은 fp operation에서 좀 길게 설정되어 있다. 따라서 유동적인 스케쥴링 없이 무조건 긴 latency를 가진다. 이를 diverged latency라 한다.
-ILP랑 TLP 내가 생각하는 차이: ILP는 한 스레드에서 여러 instr들을 OoO하는 거고 TLP는 여러 스레드를 이용해 OoO하는 것.
-RAW, WAW, WAR hazard
-->Tomasulo algorithm
-CDB를 통해 dependancy 있는 차후의 instr의 operands에 전달한다. 그렇게 함으로써 dependancy 원흉의 instr가 굳이 순서에 따라 계산되지 않아도 차후의 isntr가 계산됨.
-register renaming: queue에 tag를 달아서 operand를 구별한다 그래서 WAW, WAR 해결한다.
add r1, r2, r3 // r1은 tag #2 of queue
add r1, r5, r4 // r1은 tag #3 of queue
-궁금한 것: 그러면 que에 저장되어 있으면 dependancy 있는 애들 보다 이후에 들어온 instr를 먼저 다음 stage로 issue한다는 얘기?
-하지만 tomasulo algorithm은 WB시 OoO completion이기 때문에 branch의 경우 false일 때 이미 다 regfile이나 write될 게 다 업데이트가 되어서 문제라는데 맞아?
-->이를 해결하기 위해 ROB(Reorder Buffer) 이용: In Order Completion을 보장해준다. 결과값을 보관하고 있다가 순차적으로 완료될 수 있을 때만(branch가 참일 때)

/*ai 경량화*/
-mem access는 당근 많은 에너지, 낮은 bit width op는 낮은 에너지
-Quantization: 4bit로는 16 level, 2bit로는 16 level을 표현할 수 있다. 학습시에 '수'(weights, biases, activations)를 줄임으로써 computationally intensive 가져오고 over-parameterized, 즉 요새 너무 parameter들이 과하게 많아서 이를 quantization을 통해 메모리 사용 감소이나 계산 속도 향상을 꾀할 수 있다.
-Uniform Quantization: Quantizer, Dequantizer, S
-Integer Arithmetic Only Inference: 8bit int으로 input, output 나타남, 모든 계산은 int

/*csd mid*/
-스택의 가장 위 포인터는 제일 최근 들어온 주소를 가리키는 게 full stack, 다음 데이터가 들어올 빈 주소 가리키는 게 empty stack
-메모리 낮은 주소에는 무조건 낮은 번호 레지스터, 높->높 가령 {r2, r3, r1}이어도 메모리 큰 순서대로 r3, r2, r1 집어 넣음

20240925
1. riscv 논문 읽기
2. arm mid
3. chipyard

/*riscv minimal vector processor*/
-intro이들이 주장하는 것은 

20240926
1. rvv 관련 회의록 작성
/*rvv 관련 회의록 작성*/
RISC-V VPU to reduce the area and power

In the DNN, parallel process of large amounts of data is essential for vector operations, making the VPU indispensable. However, one of the downsides of VPUs is that, while they offer high computational efficiency, they also consume a significant amount of area and power. To solve this problem, it is crucial to optimize VPUs.

We discussed methods to reduce the area and power consumption of VPUs, such as designing smaller VPUs. This can be achieved by reducing the size of the vector register file or replacing it with general registers. To efficiently use the RISC-V Vector Extension, we are considering reducing the vector element width(SEW). This approach enables efficient operations for DLP(Data Level Parallelism) while also reducing both area and power consumption.

20240927
1.회의록 하나 작성
2. 보고서 작성
2-1.논문
3. gpu과제
4. csd과제

/*회의록*/
conv layer에서 zero인 항은 제외하여 새로운 input array와 weight를 구성해 partial sum conv를 진행한다. 이때 stride대로 array를 구성하고 cycle별로 곱하는 weight를 차례로 넣어주면 invalid 값 없이 parallel operation이 가능하게 된다. 이러한 operator에 대해 seminar에서 토의하였고 이를 chipyard에 적용할 수 있다는 가능성을 제시했다.
chipyard 내에서 verilator를 사용해 제안된 가속기의 성능을 시뮬레이션하는 계획이다. 이를 통해 zero skipping 및 compression 알고리즘 성능을 평가하고 얼마나 성능이 향상하는지 simulation을 통한 검증을 할 예정이다.
Designing Accelerator in Chipyard
We discussed that the accelerator that is available for compression. In a convolution layer, zero-valued elements are excluded to form a new input array and weights for conducting partial sum convolution. The array is structured according to the stride, and weights are multiplied without resting PE. In other words, there are no invalid output values.
During the seminar we discussed this type of operator and proposed the possibility of applying it in Chipyard. We plan to use Verilator within Chipyard to simulate the performance of the proposed accelerator. This simulation will evaluate the performance of the zero skipping and compression algorithms and verify how much performance improvement is achied through simulation.

/*논문*/
-얘네가 보통 dot product를 많이 가속하지만 얘네는 VADD, VMUL instr를 포함시켰대
-VMAC엔 8bit, arith는 32bit
-행 중심의 matrix 열중심으로 읽어오도록 strided instr 추가
-area 줄이기 위해 vector register가 아닌 regfile 그대로
-scalar arith instr의 data path 재사용
A. vec instr->vl param 조절하는 control
