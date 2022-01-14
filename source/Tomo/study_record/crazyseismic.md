# 2. 数据预处理

整个流程介绍：
   1. 下载地震事件目录：`download_event.py`;
   2. 下载地震事件（waveform & stationxml）: `download_data.py`
   3. 根据需求筛选地震事件，比如我做S波层析成像数据，那么我只需要`N,E`分量:`component_select.py`;
   4. 数据预处理：去均值，去线性趋势，两端尖灭，去仪器响应: `pre_process.py`;
   5. 将保存台站信息的`stationxml`格式文件转化为`dataless`格式文件: `xml_to_dataless.sh`；
   6. 将`miniseed`数据转化为`sac`格式: `miniseed_to_sac.sh`（PS：得到的sac参考时间是头段量
   7. 的参考时间，'iztype'还没有被写入，一般我们以地震的发震时刻为参考时间，所以需要重新修改）
   8. 将事件信息写入到`sac`格式的波形数据之中: `event_write_to_sac.py`
   9. 将`NE`分量转化为`RT`分量: `component_rotate.py`
   
其实整个过程可以用一个脚本来完成，但是我们希望一个程序能简单被多次使用，因而我们一个程序就只是实现一个功能，并且
在后续的过程中，会将这些功能都转化成函数格式，这样就能够方便调用。

## 2.1 `Crazyseismic`数据准备需求
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
   #for line in `ls $minidata_dir`
   #do
   #echo $minidata_dir/$line
   #$rdseed_dir -pf $dataless_dir/${line%.*.*H*}.dataless -q $response_dir
   #done
   ```
这里很奇怪的错误是`shell`中利用`/dir/dir`是不正确的，而应该是`./dir/dir`，
但是这种在`stationxml_to_dataless`程序中又是正确的。还不是很明白为什么，因为对
`shell`接触的比较少

### **3rd: 将地震事件信息写入到`.SAC`中**
利用`SACTrace.read`读取`sac`文件
```python
# 写入地震事件信息
for i in range(len(files)):

    sac = SACTrace.read('%s/%s' % (input_dir, files[i]),headonly=True)
    sac.o = o
    sac.iztype = iztype
    sac.evla = evla
    sac.evlo = evlo
    sac.evdp = evdp

    # 保存写入的地震信息
    sac.write('%s/%s' % (input_dir, files[i]), headonly=True)
```
