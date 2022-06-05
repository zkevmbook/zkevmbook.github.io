# 证明系统 / Proof Systems

## 1. 证明系统的特性
+ 完整性 completeness: 真的假不了。
+ 可靠性 soundness: 假的真不了。
+ 高效性 efficiency: 验证高效。
+ 简洁性 Succinctness: proof size 短，证明高效。
    * 这个不一定所有协议都能做到。

### 1.1 zk-SNAKRs

__TODO:__ zero knowledge Succinct Non-interactive Arguments of Knowledge

__TODO:__ Fiat-Shamir 变换

### 1.2 zk-STAKRs

## 2. 证明系统的分类

__TODO:__

+ IP
+ MIP
+ PCP
    * IOP
+ Linear PCP

## 3. 知名 & 常用的协议

### 3.1 Groth16

### 3.2 PLONK

## 4. Misc
### vector commitment
vector commitment 是 polynomial commitments 的一种。

polynomial commitments 不用提供每个 sibling 就能 open&verify，所以达到了 减小 witness 的效果
