<section id="nice" data-tool="markdown编辑器" data-website="https://markdown.com.cn/editor" style="font-size: 16px; color: black; padding: 25px 30px; line-height: 1.6; word-spacing: 0px; letter-spacing: 0px; word-break: break-word; word-wrap: break-word; text-align: justify; font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, 'PingFang SC', Cambria, Cochin, Georgia, Times, 'Times New Roman', serif; margin-top: -10px;"><h1 data-tool="markdown.com.cn编辑器" style="margin-top: 30px; margin-bottom: 15px; font-weight: bold; color: black; font-size: 24px;"><span class="prefix" style="display: none;"></span><span class="content">Finite Frequency Tomography of ALPS Reigon</span><span class="suffix"></span></h1>

<h2 data-tool="markdown.com.cn编辑器" style="margin-top: 30px; margin-bottom: 15px; font-weight: bold; color: black; border-bottom: 2px solid rgb(239, 112, 96); font-size: 1.3em;"><span class="prefix" style="display: none;"></span><span class="content" style="display: inline-block; font-weight: bold; background: rgb(239, 112, 96); color: #ffffff; padding: 3px 10px 1px; border-top-right-radius: 3px; border-top-left-radius: 3px; margin-right: 3px;">文献阅读</span><span class="suffix"></span><span style="display: inline-block; vertical-align: bottom; border-bottom: 36px solid #efebe9; border-right: 20px solid transparent;"> </span></h2>

 1. ***Foulger et al., 2013, Terra Nova: Caveats on tomo imagines***   
     1. 研究区域外的不均匀结构不能被忽略  
        ```
        外部结构的污染效应在数学上与研究区域内结构的污染效应相同，在远震层析成像中，通过将入射波前的形状和方向作为未知数，并在层析反演过程中求解，可以减少外部结构的偏置效应。

        ```
    
    2. 远震层析成像不能够得到研究区域内的三维结构
        ```
        对每一层的波速变化的估计是相对于该层的平均值，这些（每一层）平均值的绝对值仍然未知;
            1. Wave-speed variations are known only in the horizontal directions. Variations in the vertical direction are not calculable
            2. smearing pronblem（拖尾效应）: 模糊问题加剧了解决异常深度问题的难度，该问题导致异常沿着射线束被拉长，在远震层析成像的情况下，射线束主要是陡倾斜的，成像结果是人为的将异常进行垂直拉长（图1）。
        
        层析成像的垂向分辨率很差，对于面波，在上地幔中30-50km的速度变化是很难分辨的，对于远震体波成像则更甚。
        ```
        ![图1](.\Fig\Fig1_Depth_leakage.png "图1： Depth leakage (downward smearing) in a seismic tomography inversion (after Eken et al., 2008).")
    
    3. Crust Correct
        ```
        远震射线近乎垂直的入射到地壳之中，为了获得台站下方的地幔结构，必须对地壳以及由于缺乏交叉射线而无法解析的浅层地幔的任何部分进行校正。准确地知道适当的地壳修正是很重要的，如果处理不当，基于估计地壳结构的修正可能会错误地传播到地幔结构图像中。
        
        在地壳结构复杂的区域，消除地壳非均匀性结构的影响能够显著地提高地幔速度成像结果的可靠性。
        ``` 

    4. 不能可靠的获得异常体的幅值
        ```
        地震层析成像中的反演问题是个欠定问题，没有唯一解。

        采用奥卡姆剃刀准则选取平滑变化的模型
        ```
    
    5. 非均匀的射线覆盖
        ```
        The Earth’s mantle is not uniformly sampled by seismic waves.
         1. The structure of much of the mantle is essentially unobtainable
   
         2. 有些地区只能由几乎平行的射线组成的准平行射线束来对地幔进行采样
        ```

    6. 分辨率和棋盘测试
        ```
        远震层析成像的有效深度最多与观测阵列的孔径相等，并且只有当使用的射线在位置和方向上均匀分布时才有效果。在任何方式的成像结果中，我们首先应该关注的就是其分辨率问题，只有在知道其分辨率的情况下才能够对成像模型做出地球动力学方面的解释。

        反演结果中的深层和外围特征在所有情况下都是不可靠的，不应加以解释。 R 的棋盘测试是乐观的和误导的，因为它们是最优的阻尼逆
        ```
    7. 可重复性
        ```
        一般来说，只能对使用不同方法和不同数据集中获得的模型中相同的速度异常进行地质解释，而对于没有普遍存在的异常特征，在解释的过程中需要格外小心。
        ```
    8. 相对速度和绝对速度
        ```
        相对波速图与绝对波速图有着截然不同的视觉印象，而绝对波速图描述的是真实的地球地震结构

        The question of what is an appropriate reference model is particularly important in the case of subduction zones. If the starting model LVZ wave speeds are too high, the imaged slab will appear to be broken

        However, on local and regional scales, mineralogical and chemical heterogeneities, as well as partial melt, crystal size and the presence of hydrogen and carbon may have greater effects than temperature on seismic wave speed and cannot be ignored

        ```