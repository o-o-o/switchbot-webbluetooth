# SwitchBotをWebBluetooth経由で動作させる

WebBluetooth APIを利用し、ブラウザ上からSwitchBotを操作するコードです。

2019-03-02時点でWebBluetooth APIはGoogle Chromeでのみサポートされています。  
Google Chrome以外での検証は行っていません。

## セットアップ

### Google Chrome

Experimental Web Platform featuresを有効化する必要があります。

chrome://flags/#enable-experimental-web-platform-features

Google Chromeの再起動が必要です。

## サンプル

https://jun.enomoto.me/switchbot-webbluetooth/index.html

## 実行

Webサーバなどを立てて試してください。  
https環境が必要です。

## SwitchbotのBluetoothに関する情報

| 名前 | 備考 |
|-----|-----|
| サービス UUID | cba20d00-224d-11e6-9fb8-0002a5d5c51b |
| キャラクタリスティック UUID | cba20002-224d-11e6-9fb8-0002a5d5c51b |
| Pressの値 | 0x570x010x00 |
| Onの値 | 0x570101 |
| Offの値 | 0x570102 |

## 最短のコード

```javascript
const device = await navigator.bluetooth.requestDevice({
  acceptAllDevices: true,
  // filters: [{ name: 'WoHand' }],
  optionalServices: ['cba20d00-224d-11e6-9fb8-0002a5d5c51b']
});
const server = await device.gatt.connect();
const service = await server.getPrimaryService('cba20d00-224d-11e6-9fb8-0002a5d5c51b');
const characteristic = await service.getCharacteristic('cba20002-224d-11e6-9fb8-0002a5d5c51b');
characteristic.writeValue(new Uint8Array([0x57, 0x01, 0x00]));
```