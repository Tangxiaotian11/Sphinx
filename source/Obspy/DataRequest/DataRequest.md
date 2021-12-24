# Seismic data request using Obspy
## obspy.clients.fdsn
FDSN: Federation of Digital Seismograph Networks 
FDSN membership: <https://www.fdsn.org/membership/>
FDSN networks:   <https://www.fdsn.org/networks/>
### Basic FDSN Client Usage
```
from obspy.clients.fdsn import Client
client = Client("RESIF")
```
Client中可选参数：
* Data center: eg: 'IRIS','ETH','RESIF'......
    * 从地震台网查找数据中心：<https://www.fdsn.org/networks/>
    * MetaData Aggregator of IRIS:<http://ds.iris.edu/mda/?initial=Y>

___________________________________________________________________
# 
