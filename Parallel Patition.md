# Parallel Partition



## Differential MITM Attack
> from Section 2.2 of Paper [22-CC-Differential Meet-In-The-Middle Cryptanalysis](https://link.springer.com/chapter/10.1007/978-3-031-38548-3_9)

### 原理与假设

$n$-bit 分组密码 $E$，有如下图的区分器与扩展：

<img src="https://github.com/user-attachments/assets/93471796-555c-41da-9b9e-9fad9f48de36" width = "300" height = "500" div align=center />

### 方法与步骤

随机选一个明文 $P$，加密得到相应的密文 $C=E_K(P)$ ；

依上图规则，根据 $k_{in}$ 将 $P$ 部分加密，然后$\oplus\Delta_x$ ，最后解密到 $\widetilde{P}$，即

$$
Upper:\qquad P\stackrel{k_{in}}{\longrightarrow} \oplus\Delta_x \stackrel{k_{in}}{\longrightarrow} \widetilde{P} \qquad (E_{k_{in}}(P)\oplus E_{k_{in}}(\widetilde{P})=\Delta_x)\\

Lower:\qquad C\stackrel{k_{out}}{\longrightarrow} \oplus\Delta_y \stackrel{k_{out}}{\longrightarrow} \widetilde{C}\qquad (E^{-1}_{k_{out}}(C)\oplus E^{-1}_{k_{out}}(\widetilde{C})=\Delta_y)
$$

所以组成的 Pairs 数量为 $2^{|k_{in}|}$ 个 $(P,\widetilde{P},k_{in})$， $2^{|k_{out}|}$ 个 $(C,\widetilde{C},k_{out})$

得到的 $\widetilde{P}$ 之后可以加密得到相应的密文 $\widehat{C}$, 将 $(P,(\widetilde{P},\widehat{C}),k_{in})$ 存入 Hash 表（索引为 $\widehat{C}$），在 Lower 部分同时生成 $\widetilde{C}$ 之后，寻找 $(\widehat{C},\widetilde{C})$ 相等的碰撞。找到，即为正确 Pairs。

### 复杂度分析





## Basic Parallel Partition in D-MITM

> from Section 2.2 of Paper [22-CC-Differential Meet-In-The-Middle Cryptanalysis](https://link.springer.com/chapter/10.1007/978-3-031-38548-3_9)



### 原理与假设

假设有概率为 $2^{-p}$ 的差分区分器，其中 $p$ 为区分器概率的负对数（$p>0$）

$\bigstar$ 当分组密码的密钥加不是加在全状态上的时候（**非全状态密钥加（Non-Full Key Addition）**，典型算法有$\texttt{SKINNY, GIFT, SIMON}$，parallel partition可以用来在增加复杂度的情况下增加攻击轮数，**ONE** round。

如下图所示，在区分器末尾添加一轮，密钥只加在 $m$ bits 状态上，若 $p>m$，则以上加一轮条件可以被满足。

<img src="https://github.com/user-attachments/assets/41a6a985-4011-481a-a627-f37cd4677d02" width = "500" height = "300" div align=center />

### 方法与步骤

