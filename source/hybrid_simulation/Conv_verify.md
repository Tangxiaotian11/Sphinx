# 验证卷积公式
$$
\begin{aligned}
    conv(f+df,g+dg) \approx (1+df/f+dg/g)*conv(f,g) + conv(df,fg)\\
    \approx conv(f,g) + conv(df,dg) + conv(f,dg) + conv(df,g)
\end{aligned}
$$

流程(ps: )：
 1. 解算`f,g`和`conv(f,g)`;
 2. 解算`df,dg`，`conv(df,dg)`和`conv(f+df,g+dg)`;
 3. 解算`conv(f,dg) + conv(df,g)`.