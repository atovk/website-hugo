---
title: "java通过oshi获取系统和硬件信息 / Oshi get system and hardware information"
date: 2019-09-27T22:42:49+08:00
keywords:
- echosence
- 技术分享
- 
description : "通过"
draft: false
tags: ["linux", "java", "monitor"]
categories: ["linux", "java", "monitor"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---


> OSHI 是用java写的系统监控工具接口，提供主要的监控指标信息，具体接口作用如下：

## maven 依赖引入

```xml

<dependency>
    <groupId>com.github.oshi</groupId>
    <artifactId>oshi-core</artifactId>
    <version>4.0.0</version>
</dependency>

```

## API 操作

```java

// 初始化系统信息对象
SystemInfo systemInfo = new SystemInfo();

// 获取硬件信息
HardwareAbstractionLayer hardware = systemInfo.getHardware();

// 获取操作系统进程相关信息
OperatingSystem operatingSystem = systemInfo.getOperatingSystem();

```

### 操作BIOS系统信息

> 获取 BIOS 系统信息

```java

ComputerSystem computerSystem = hardware.getComputerSystem();
Firmware firmware = computerSystem.getFirmware();
String name = firmware.getName();
String description = firmware.getDescription();
String firmwareManufacturer = firmware.getManufacturer();
String releaseDate = firmware.getReleaseDate();
String firmwareVersion = firmware.getVersion();

```

### 传感器信息

> 风扇/温度信息

```java

Sensors sensors = hardware.getSensors();
int[] sensorsFanSpeeds = sensors.getFanSpeeds();
double sensorsCpuVoltage = sensors.getCpuVoltage();
double sensorsCpuTemperature = sensors.getCpuTemperature();

```

### 内存信息

> 获取硬件内存信息

```java

GlobalMemory memory = hardware.getMemory();

long memoryTotal = memory.getTotal();
long memoryAvailable = memory.getAvailable();
long memoryPageSize = memory.getPageSize();
VirtualMemory memoryVirtualMemory = memory.getVirtualMemory();

```

### CPU 线程信息

> 获取机器硬件执行，cpu频率/型号/核心相关信息

```java

CentralProcessor processor = hardware.getProcessor();

```

### 显示器信息

> 显示器相关型号/分辨率之类信息

```java

Display[] displays = hardware.getDisplays();

```

### 磁盘信息

> 当前磁盘的硬件信息，读写状态，分区信息等

```java

HWDiskStore[] diskStores = hardware.getDiskStores();

```

### 网卡信息

> 获取网卡详细信息，mac/ip4/6地址，读写状态，中断/错误等信息

```java

NetworkIF[] networkIFs = hardware.getNetworkIFs();

```

### 电源状态

> ...

```java

PowerSource[] powerSources = hardware.getPowerSources();

```

### 声卡信息

> 获取名称/描述等

```java

SoundCard[] soundCards = hardware.getSoundCards();

```

### USB 信息

> 获取USB接口信息，可以过滤出正在使用的USB接口，及相关详细信息，true 树形返回

```java

UsbDevice[] usbDevices = hardware.getUsbDevices(true);

```

## 系统进程相关信息

### 获取操作系统位数（23/64）

```java

int bitness = operatingSystem.getBitness();

```

### 获取指定线程下的子线程

> 传入父进程ID，设置返回进程数量及进程排序方法，返回该父进程下指定数量排序下的子进程数

```java

OSProcess[] childProcesses = operatingSystem.getChildProcesses(1, 2, OperatingSystem.ProcessSort.CPU);

```

### 获取操作系统类别

> linux/MACOS/unix/windows 等

```java

String family = operatingSystem.getFamily();

```

### 获取当前系统下所有文件分区信息

> 获取文件系统分区信息，剩余空间，挂载信息等 系统存储状态

```java

FileSystem fileSystem = operatingSystem.getFileSystem();
OSFileStore[] fileStores = fileSystem.getFileStores();

```

### 系统生产厂家

> 能获取系统厂商信息

```java

String manufacturer = operatingSystem.getManufacturer();

```

### 获取网络参数

> 系统中网卡信息，dns信息，域名信息，ip4/6信息

```java

NetworkParams networkParams = operatingSystem.getNetworkParams();

String[] dnsServers = networkParams.getDnsServers();
String domainName = networkParams.getDomainName();
String hostName = networkParams.getHostName();
String ipv4DefaultGateway = networkParams.getIpv4DefaultGateway();
String ipv6DefaultGateway = networkParams.getIpv6DefaultGateway();

```

### 获取指定进程信息

> 传入进程号，获取该进程详细信息，所属组/用户/状态等

```java

OSProcess process = operatingSystem.getProcess(1121);

```

### 获取但前系统中的进程数

> 获取当前系统中所有进程数

```java

int processCount = operatingSystem.getProcessCount();

```

### 根据排序规则 返回指定线程数

> 根据排序规则 返回指定线程数

```java

OSProcess[] processes = operatingSystem.getProcesses(10, OperatingSystem.ProcessSort.CPU);

```

### 获取进程ID list内所有近程

> 返回指定进程ID下所有进程实例

```java

List<OSProcess> processList = operatingSystem.getProcesses(new ArrayList<>());

```

### 获取系统内总线程数

> 获取系统内所有线程句柄数

```java

int threadCount = operatingSystem.getThreadCount();

```

### 获取系统启动时间

```java

long systemBootTime = operatingSystem.getSystemBootTime();

```

### 获取系统版本信息

```java

OperatingSystemVersion version = operatingSystem.getVersion();

```

