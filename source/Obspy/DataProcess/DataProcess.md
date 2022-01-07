# 1 Obspy: Data Process

## 1.1 仪器响应
Reference：<https://docs.obspy.org/packages/autogen/obspy.core.trace.Trace.remove_response.html#obspy.core.trace.Trace.remove_response>

用法：
`Trace.remove_response(inventory=None, output='VEL', water_level=60, pre_filt=None, zero_mean=True, taper=True, taper_fraction=0.05, plot=False, fig=None, **kwargs)`

参数说明：

 `inventory`: 台站信息，及下载得到的`stationxml`

 `output`: 输出单位：
 ```  
        "DISP": 位移，m  
        "VEL" : 速度，m/s  
        "ACC" : 加速度,m/s/s    
 ```
 `water_level`: water level for deconvoloution, 不太明白这个参数的作用

 `pre_filt`: 带通滤波，
    
    pre_filt=[0.005, 0.01, 10, 25]
列表或元组定义了余弦锥度的四个角频率(f1, f2, f3, f4)，该锥度在f2和f3之间，当f1 < f < f2和f3 < f < f4时，锥度为0。 

`zero_mean`: 去均值

`taper`: 两端尖灭

`taper_fraction`: Taper fraction of cosine taper to use.

`plot`: 画图展示在频率域去仪器响应的步骤，可以分为3步：1. `pre_filt taper function`; 2. `仪器响应`，3. `invert instrument response` 
 
