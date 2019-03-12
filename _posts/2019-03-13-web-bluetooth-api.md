---
layout: post
title: “网页连接Bluetooth"
comments: true
description: ""
keywords: “Bluetooth,蓝牙"
---



连接 Tinkamo 马达，并让它转起来

```javascript
  const NAME = 'Tinka';
  const SERVER = 0xfffa;
  const CHARACTERISTIC = 0xfffc;
  let myDevice;

  buttonConnect.addEventListener('pointerup', function(event) {
    navigator.bluetooth.requestDevice({
        filters: [{ namePrefix: NAME }],
        optionalServices: [SERVER]
      })
      .then(device => {
        myDevice = device;
        return device.gatt.connect();
      })
      .then(server => {
        return server.getPrimaryService(SERVER);
      })
      .then(service => {
        return service.getCharacteristic(CHARACTERISTIC);
      })
      .then(characteristic => {
        let expended = new Uint8Array([0x5A, 0xAB, 0x0A, 0x00, 0x00, 0x00, 0x05, 0x00, 0x00, 0x00, 0x60])
        return characteristic.writeValue(expended);
        // return characteristic.readValue();
      })
      .then(value => {
        console.log(value)
        // console.log('Battery percentage is ' + value.getUint8(0));
      })
      .catch(error => { console.log(error); });
  });

  buttonDisconnect.addEventListener('pointerup', function(event) {
    if (myDevice) myDevice.gatt.disconnect();
  });
```



踩坑

- 目前只有 Chrome 浏览器支持
- 需要 HTTPS
- 处于安全考虑，调起蓝牙模块需要手动触发



相关阅读

- [nteract with Bluetooth devices on the Web](https://developers.google.com/web/updates/2015/07/interact-with-ble-devices-on-the-web)
- [通过 Web 控制蓝牙设备：WebBluetooth入门](https://blog.csdn.net/eyeofangel/article/details/87890418)
- [Web Bluetooth API 初探](https://zhuanlan.zhihu.com/p/20657057)