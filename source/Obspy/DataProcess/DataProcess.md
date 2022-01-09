# 1 Obspy: Data Process

## 1.1 仪器响应
Reference：<https://docs.obspy.org/packages/autogen/obspy.core.trace.Trace.remove_response.html#obspy.core.trace.Trace.remove_response>

关于仪器响应的具体介绍，可以参`seisman`的博客：<https://blog.seisman.info/tags/%E4%BB%AA%E5%99%A8%E5%93%8D%E5%BA%94/>

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

 `pre_filt`: 带通滤波，定一个地震学所关注的频率段，
    
    pre_filt=[0.005, 0.01, 10, 25]
 f1、f2、f3、f4，满足 `f1<f2<f3<f4` ，其中 f4 必须 小于或等于 50Hz，对于其他参数没有过多要求。

`zero_mean`: 去均值

`taper`: 两端尖灭

`taper_fraction`: Taper fraction of cosine taper to use.

`plot`: 画图展示在频率域去仪器响应的步骤，可以分为3步：1. `pre_filt taper function`; 2. `仪器响应`，3. `invert instrument response` 
 
