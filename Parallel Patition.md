# Parallel Partition



## Differential MITM Attack
> from Section 2.2 of Paper [22-CC-Differential Meet-In-The-Middle Cryptanalysis](https://link.springer.com/chapter/10.1007/978-3-031-38548-3_9)

### 原理与假设

$n$-bit 分组密码 $E$，有如下图的区分器与扩展：

<img src="https://github.com/user-attachments/assets/93471796-555c-41da-9b9e-9fad9f48de36" width = "300" height = "500" div align=center />

### 方法与步骤

随机选一个明文 $P$，加密得到相应的密文 $C=E_K(P)$ ；

依上图规则，根据 $k_{in}$ 将 $P$ 部分加密，然后 $\oplus\Delta_x$ ，最后解密到 $\widetilde{P}$，即

$$
\begin{align}
& Upper:\qquad & P\stackrel{k_{in}}{\longrightarrow} \oplus\Delta_x \stackrel{k_{in}}{\longrightarrow} \widetilde{P} \qquad & (E_{k_{in}}(P)\oplus E_{k_{in}}(\widetilde{P})=\Delta_x)\\
& Lower:\qquad & C\stackrel{k_{out}}{\longrightarrow} \oplus\Delta_y \stackrel{k_{out}}{\longrightarrow} \widetilde{C}\qquad & (E^{-1}_{k_{out}}(C)\oplus E^{-1}_{k_{out}}(\widetilde{C})=\Delta_y)
\end{align}
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

如上图，在攻击最后添加一轮，$X$ 和 $Y$ 分别是 Key Addition 前后的状态（其中有 $n-m$ bits 未被 Key Addition 影响）

$\blacktriangleright$ 由于需要 $2^p$ 个明文来保证至少找到一个正确 pair，假设现有数据量即为 $2^p$，则在上图 $S_{r-1}$ 处即有 $2^p$ 个数据。将 $2^n$（实际是 $2^p$ 种情况） 在 $X$ 和 $Y$ 处进行划分，可化为 $2^m \times 2^{p-m}$ 两组，其中 $2^m$ 对应受密钥影响的部分；$2^{p-m}$ 对应不受密钥影响的部分，这部分同样需要多次取值（<u>但不受密钥影响，每次不同取值对应 $2^m$ 次 basic D-MITM 的操作</u>）来达到数据要求。

* 对每个不同取值的 $p-m$ 部分的 $X$ 和 $Y$ （复杂度 $2^{p-m}$），执行以下操作：

  * 对 $X$，由于有 $2^m$ 个 $P$，所以 $(P,\widetilde{P},k_{in})$ 的数量要 $\times 2^m$，即为 $2^{m+k_{in}}$ 个 Pairs.
  * 对 $Y$，由于其是密文，可将其<u>解密至明文</u>，然后执行明文部分的部分加密操作。同上，有 $2^{m+k_{out}}$  个Pairs.

* 匹配 $X$ 和 $Y$，匹配后的数量为 $2^{|k_{in}|+|k_{out}|+2m}$ ，比之前多了 $2^{2m}$（没考虑密钥桥，有密钥桥时会更少）.

  * 每 $2^m$ 个 $(X,X')$ 和 $(Y,Y')$ 中只有 1 个有效配对，所以对 $(X,X')$ 和 $(Y,Y')$ 都有 $2^{-m}$ 的过滤效果.
  * 因此，匹配后的数量为 $2^{|k_{in}|+|k_{out}|+2m-m-m}=2^{|k_{in}|+|k_{out}|}$ 没变.

  
