# Android Network Tools ![image](./app/src/main/res/mipmap-xhdpi/ic_launcher.png)

[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-AndroidNetworkTools-green.svg?style=true)](https://android-arsenal.com/details/1/3112)
[![CircleCI](https://circleci.com/gh/stealthcopter/AndroidNetworkTools.svg?style=svg)](https://circleci.com/gh/stealthcopter/AndroidNetworkTools)
<a href="https://www.buymeacoffee.com/stealthcopter"><img src="https://cdn.buymeacoffee.com/buttons/v2/arial-yellow.png" height="20px"></a>

Disappointed by the lack of good network apis in android / java I developed a collection of handy networking tools for everyday android development.

* Port Scanning
* Subnet Device Finder (discovers devices on local network)
* Ping
* Wake-On-Lan
* & More :)

## General info

The javadoc should provide all information needed to understand the methods, but if not feel free to add a issue in github and I'll address any questions! :)

# Support
If you love what I'm doing with Android Network Tools and my other projects, you can now support my work directly! By buying me a coffee ☕, you're not just fueling my caffeine needs – you're helping me dedicate more time to developing and improving these open source projects. Every coffee counts and brings more innovation to the cybersecurity world. Thank you for your support – it means the world to me and the open source community!

<a href="https://www.buymeacoffee.com/stealthcopter"><img src="https://cdn.buymeacoffee.com/buttons/v2/arial-yellow.png" height="50px"></a>

### Sample app

示例应用程序发布在Google Play&F-Droid上，以便您快速、轻松地测试库。享受吧！如果你的测试结果不同，请给我们反馈。

[<img src="https://fdroid.gitlab.io/artwork/badge/get-it-on.png"
     alt="Get it on F-Droid"
     height="70">](https://f-droid.org/packages/com.stealthcotper.networktools/)
[<img src="https://play.google.com/intl/en_us/badges/images/generic/en-play-badge.png"
     alt="Get it on Google Play"
     height="70">](https://play.google.com/store/apps/details?id=com.stealthcotper.networktools)

## Usage

### Add as dependency
这个库尚未在Maven Central中发布，在此之前，您可以将其添加为库模块或使用JitPack.io

添加远程maven url
```groovy

    repositories {
        maven {
            url "https://jitpack.io"
        }
    }
```

然后添加库依赖项。记得在这里查看最新版本

```groovy
    dependencies {
        compile 'com.github.stealthcopter:AndroidNetworkTools:0.4.5.3'
    }
```

### Add permission
需要internet权限(obviously...）
```xml
  <uses-permission android:name="android.permission.INTERNET" />
```

### Port Scanning

一个简单的基于java的TCP/UDP端口扫描程序，快速易用。默认情况下，它会根据地址是localhost、local network还是remote来猜测扫描时使用的最佳超时和线程。您可以通过调用setNoThreads（）和setTimeoutMillis（）来重写这些参数
```java
    // Synchronously
    ArrayList<Integer> openPorts = PortScan.onAddress("192.168.0.1").setMethodUDP().setPort(21).doScan();

    // Asynchronously
    PortScan.onAddress("192.168.0.1").setTimeOutMillis(1000).setPortsAll().setMethodTCP().doScan(new PortScan.PortListener() {
      @Override
      public void onResult(int portNo, boolean open) {
        if (open) // Stub: found open port
      }

      @Override
      public void onFinished(ArrayList<Integer> openPorts) {
        // Stub: Finished scanning
      }
    });

```

### Subnet Devices

查找与当前设备在同一子网中响应ping的设备。您可以使用setTimeOutMillis（）[默认值2500]设置ping的超时值，使用setNoThreads（）设置线程数[默认值255]
```
    // Asynchronously
    SubnetDevices.fromLocalAddress().findDevices(new SubnetDevices.OnSubnetDeviceFound() {
        @Override
        public void onDeviceFound(Device device) {
            // Stub: Found subnet device
        }

        @Override
        public void onFinished(ArrayList<Device> devicesFound) {
            // Stub: Finished scanning
        }
    });

```

### Ping

如果设备上有本机ping二进制文件（有些设备没有），则使用本机ping二进制文件；如果没有，则返回到端口7上的TCP请求（echo请求）。
```java
     // Synchronously 
     PingResult pingResult = Ping.onAddress("192.168.0.1").setTimeOutMillis(1000).doPing();
     
     // Asynchronously
     Ping.onAddress("192.168.0.1").setTimeOutMillis(1000).setTimes(5).doPing(new Ping.PingListener() {
      @Override
      public void onResult(PingResult pingResult) {
        ...
      }
    });
```

注意：如果我们不得不使用TCP端口7（java方式）来检测设备，我们会发现比使用本机ping二进制文件要少得多。如果这是一个问题，您可以考虑添加一个ping二进制文件到您的应用程序或设备，使它始终可用。

注意：如果您想要一个更高级的portscanner，您应该考虑将nmap编译到您的项目中并使用它。

### Wake-On-Lan

向IP/MAC地址发送Wake-on-Lan数据包

```java
      String ipAddress = "192.168.0.1";
      String macAddress = "01:23:45:67:89:ab";
      WakeOnLan.sendWakeOnLan(ipAddress, macAddress);
```

### Misc

其他有用方法：

```java
      // Get a MAC Address from an IP address in the ARP Cache
      String ipAddress = "192.168.0.1";
      String macAddress = ARPInfo.getMacFromArpCache(ipAddress);
```

## Building

这是一个标准的gradle项目。

# Contributing

我欢迎拉请求，问题和反馈。

Fork it
创建特性分支（git checkout-bmy-new-feature）
提交您的更改（git Commit-am“添加了一些功能”）
推送到分支（git Push originmy-new-feature）
创建新的请求请求
