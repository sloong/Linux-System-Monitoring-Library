# Lightweight Linux Monitoring Library
For several Projects on embedded Linux devices and Linux Servers i wanted to monitor the system load so therefore, 
i created this Library where i can get the CPU Load / Ethernet Load and Memory consumption, to send it via Websocket to 
a backend application or print it on a LCD display or send the data via MQTT: 



### Features:

The library consists of STL and c++17 only, no thirdparty library is used (except for the websocket example and Unittests). 



* CPU Usage:
  * total CPU Usage
  * CPU Usage per Core
  * CPU Information
    
* Networkinterface load:
  * Bytes/Bits per Second
  * Total Received or Transmitted Bytes

* Memory Usage:
  * System Memory Usage
  * Total Memory
  * Memory Usage per Process
  
* Linux Utils and Features:
  * is remote Device Online - (parse Ping response)
  * getPID by Name
  * getOSVersion
  * SysUptime
  * Number of Threads by PID 
  * setApplication as Deamon


## How to build:
Clone the project: `git clone git@github.com:fuxey/Lightweight_LinuxSystemMonitor.git`

#### build the library:

    mkdir build
    cd build
    cmake ..
    cmake --build . --target linuxmonitoring
    
find the library in the folder `lib` of the `build` folder

#### build simple linux load monitor (No thirdpary dependencies): 
    mkdir build
    cd build
    cmake ..
    cmake --build . --target simple_linuxsystemMonitor
    
find the executable in the folder `bin` of the `build` folder

#### build linuxsystem websocket service (thirdpary dependencies needed):

    git submodule update --init --recursive
    mkdir build
    cd build
    cmake ..
    cmake --build . --target simple_linuxsystemMonitor
    
find the executable in the folder `bin` of the `build` folder



## Examples:

### Linux Monitoring Websocket Server:
This app starts a websocket Server which offers the linuxsystem monitor values as a Json object.
#### Monitorig Data are presented in JSON: 
therefore the most popular [C++ JSON lib](https://github.com/nlohmann/json) is used + the very cool [Json to C++](https://app.quicktype.io/) code generator tool. 
    
    {
      "Linuxsystemmonitoring": {
        "linuxethernet": [
          {
            "iFace": "eth0",
            "BitsRXSecond": "undef",
            "BytesRXSecond": "89Byte/s",
            "BytesRXTotal": 92824691,
            "BytesTXSecond": "22Byte/s",
            "BytesTXTotal": 5783311275,
            "BytesTotal": 5876135966,
            "BytesTotalPerSecond": "97Byte/s"
          }
        ],
        "MemoryUsage:": {
          "MemoryUsage_KIB": 11195216,
          "MemoryUsage_perc": 34.0,
          "MemoryUsage_totalKIB": 32893064,
          "MemoryUsage_of_process:": 349822
        },
        "CPU": {
          "CPUUsage": 7.99,
          "CPU_Type": "Intel I7",
          "num_of_cores": 12,
          "MultiCore": [
            "CPU0:4.85%",
            "CPU1:7.77%",
            "CPU2:11.76%",
            "CPU3:9.52%",
            "CPU4:4.04%",
            "CPU5:1.98%",
            "CPU6:6.86%",
            "CPU7:4.90%",
            "CPU8:15.69%",
            "CPU9:5.05%",
            "CPU10:17.48%",
            "CPU11:7.92%",
            "CPU12:9.90%",
            "CPU13:9.52%",
            "CPU14:7.92%",
            "CPU15:9.71%"
          ],
          "MultiUsage": [
            4.85,
            7.77,
            11.76,
            9.52,
            4.04,
            1.98,
            6.86,
            4.9,
            15.69,
            5.05,
            17.48,
            7.92,
            9.9,
            9.52,
            7.92,
            9.71
          ]
        },
        "system": {
          "SysUptime": 37008,
          "SysUptime_days": 0,
          "SysUptime_hours": 10,
          "SysUptime_min": 16,
          "SysUptime_sec": 48
        }
      },
      "type": "system"
    }
 
[for detailed Information](./example/LinuxSystemMonitor_WebsocketService/Readme.md)


#### Simple Monitor:
Main aim of this example, is to show how to interact with the library. The app loops through the results of linux monitor lib and prints out memory load, cpu load, network load as the following print shows :

    memory load: 47.76% maxmemory: 16133756Kb used: 7705056Kb  Memload of this Process 6260KB 
    cpu load: 50.8292% of cpu: : Intel(R) Core(TM) i7-10750H CPU @ 2.60GHz
    mulitcore [ 52.48 % 48.45 % 47.06 % 51 % 54.46 % 49.5 % 53.92 % 51.49 % 47.52 % 52.48 % 48.45 % 54.37 %  ]
    network load: wlp0s20f3 : 21.1mBit/s RX Bytes Startup: 37696419 TX Bytes Startup: 1734702657
    
    
[for detailed Information](./example/simplemonitor/Readme.md)

    

## Todo:
* CPU Usage per Process
* example -> monitoring and creating graph with grafana
* example -> avr lcd bridge
* Testing: more Catch2 tests
* Data Export in JSON, BSON,MSGPack, XML
* logging and average of 30min, 2h, 6h, 24h , 2d, 7d. 
* Documentation
* ~~ethernetparser optimization~~ 
* ~~memoryparser optimization~~

# Get in Contact: 
If there are any Bugs or Request for extensions or features, feel free to
[email me](fuxeysolution@gmail.com).


#### Used 3rdparty libs(for the examples):

* [cxxopts - argument parser lib (MIT) ](https://github.com/jarro2783/cxxopts)
* [nlohmann json - very handy json lib (MIT) ](https://github.com/nlohmann/json)
* [quicktype - json object to cpp code](https://app.quicktype.io/)
* [uWebsockets - handy lightweight high performance websocket lib (Apache 2.0) ](https://github.com/uNetworking/uWebSockets)
* [uSockets - socket lib which uWebsockets built on top (Apache 2.0)](https://github.com/uNetworking/uSockets)
* [Catch2 - testing Framework (BSL-1.0)](https://github.com/catchorg/Catch2)