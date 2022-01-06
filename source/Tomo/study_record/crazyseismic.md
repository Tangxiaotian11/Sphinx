# 2. 利用 Crazyseismic 拾取相对到时

## 2.1 数据准备
 目前只接受处理SAC格式的地震数据。

 在 SAC 格式数据中，头文件中必须包含以下信息：
  1. 地震事件信息
     1. 发震时刻  
     header.o
     2. 地震位置：经度/纬度/深度  
     header.evla, header.evlo, header.evdp
  2. 台站信息
     1. 台站位置：经度/纬度  
      header.stla, header.stlo
     2. 台站和台网名称  
      header.kstnm, header.knetwk
  3. 数据信息
     1. 数据开始和结束时间  
      header.b, header.e
     2. 数据采样率  
     header.delta
     3. 数据点数   
     header.npts

### **1st: 将台站信息`xml`格式 转化成 `dataless SEED`格式**
软件需求： `stationxml-seed-converter`: 它是一个将`dataless SEED` 
和`FDSN-StationXML`格式进行相互转化的工具。 

环境需求：`Java`

下载地址： <https://github.com/iris-edu/stationxml-seed-converter>

命令：
```
java -jar PATH/TO/stationxml-seed-converter-2.1.0.jar --input /PATH/TO/StnXML_file.xml --output /PATH/TO/StnXML_file.dataless

java -jar /PATH/TO/stationxml-seed-converter-2.1.0.jar --input /PATH/TO/Dataless_file.dataless --output /PATH/TO/Dataless_file.xml

java -jar /PATH/TO/stationxml-seed-converter-2.1.0.jar /PATH/TO/METADATA --output /PATH/TO/NEW/METADATA
```

通常我们要进行的是大量数据的批处理,在这里我使用了一个`shell`脚本来实现批处理
```bash
#!/bin/sh

# 将“stationxml”文件转化为“dataless”文件
# 遍历文件夹并输出
input_path=/d/work/Tomo/Tomo/2019_11_6_20_52_56.85/station
output_path=/d/work/Tomo/Tomo/2019_11_6_20_52_56.85/station_dataless
tool_path=/d/work/Tomo/Tomo/src/stationxml-seed-converter-2.1.0.jar 

echo tool_path
for line in `ls $input_path`
do 
# delete xml in of each line
echo $output_path/${line%.xml}.dataless 
java -jar $tool_path --input $input_path/$line --output $output_path/${line%.xml}.dataless
done
```

### **2nd: `miniseed` to `sac`**
   程序： `rdseed`  
   From:  <http://ds.iris.edu/pub/programs/> (这里面有很多地震学的程序，有时间可以都了解一下)

   从dataless SEED 中提取波形数据
   ```
   rdseed -df infile.miniseed -g infile.dataless
   ```
   从dataless SEED 中提取RESP仪器响应
   ```  
   rdseed -Rf infile.dataless 
   ```
   dataless SEED 中提取 PZ 仪器响应 
   ``` 
   rdseed -pf infile.dataless
   ```

   同样对数据要进行批处理，所以这里仍然使用`shell`脚本实现批处理
   ```bash
   #!/bin/sh
   event=2019_11_6_20_52_56.85
   minidata_dir=./$event/waveform
   dataless_dir=./$event/station_dataless
   sac_dir=./$event/waveform_sac
   rdseed_dir=./rdseedv5.3/rdseed.rh6.linux_64
   response_dir=./$event/response_PZ
   echo $rdseed_dir
   # 1. get waveform
   for line in `ls $minidata_dir`
   do
   echo $minidata_dir/$line
   $rdseed_dir -df $minidata_dir/$line -g $dataless_dir/${line%.*.*H*}.dataless -q $sac_dir
   done

   # 2. 提取仪器响应 PZ
   for line in `ls $minidata_dir`
   do
   echo $minidata_dir/$line
   $rdseed_dir -pf $dataless_dir/${line%.*.*H*}.dataless -q $response_dir
   done
   ```
这里很奇怪的错误是`shell`中利用`/dir/dir`是不正确的，而应该是`./dir/dir`，
但是这种在`stationxml_to_dataless`程序中又是正确的。还不是很明白为什么，因为对
`shell`接触的比较少

### **3rd: 将地震事件信息写入到`.SAC`中**