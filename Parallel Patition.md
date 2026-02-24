# Parallel Partition



## Differential MITM Attack

### 原理与假设

$n$-bit 分组密码 $E$，有如下图的区分器与扩展：

<img src="https://github.com/user-attachments/assets/93471796-555c-41da-9b9e-9fad9f48de36" width = "500" height = "300" div align=center />



## Basic Parallel Partition in D-MITM

> from Section 2.2 of Paper [22-CC-Differential Meet-In-The-Middle Cryptanalysis](https://link.springer.com/chapter/10.1007/978-3-031-38548-3_9)



### 原理与假设

假设有概率为 $2^{-p}$ 的差分区分器，其中 $p$ 为区分器概率的负对数（$p>0$）

$\bigstar$ 当分组密码的密钥加不是加在全状态上的时候（**非全状态密钥加（Non-Full Key Addition）**，典型算法有$\texttt{SKINNY, GIFT, SIMON}$，parallel partition可以用来在增加复杂度的情况下增加攻击轮数，**ONE** round。

如下图所示，在区分器末尾添加一轮，密钥只加在 $m$ bits 状态上，若 $p>m$，则以上加一轮条件可以被满足。

<img src="https://github.com/user-attachments/assets/41a6a985-4011-481a-a627-f37cd4677d02" width = "500" height = "300" div align=center />

### 方法与步骤

