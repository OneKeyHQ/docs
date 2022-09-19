# deviceVerify

Get the hardware signature information cert and signature fields from the device and submit them to the onekey server to verify the reliability of the device. Reading signature data requires confirmation at the device.

```typescript
const result = await HardwareSDK.deviceVerify(connectId, params);
```

## Params

****[**Optional common params**](common-params.md)\


* `dataHex` — _optional_ `string` A random string of hex string, such as a timestamp passed in hex string.

## Example

```typescript
HardwareSDK.deviceVerify(connectId, {
    dataHex: '183533a058f'
});
```

## Result

```typescript
{
    success: true,
    payload: {
        cert: "3082019f30820146a003020102020204d2300a06082a8648ce3d040302308196310b300906035504061302434e3110300e06035504080c074265694a696e673110300e06035504070c074861694469616e311e301c060355040a0c15424958494e20474c4f42414c20434f2e2c204c54443110300e060355040b0c07446576656c6f703111300f060355040b0c08426978696e4b6579311e301c06035504030c15424958494e20474c4f42414c20434f2e2c204c5444301e170d3232303830313035333630375a170d3332303732393035333630375a301b3119301706035504030c10426978696e32323038303130303030393059301306072a8648ce3d020106082a8648ce3d030107034200047f5b531a6a9c36db95fcd82650c3d4a58860da90cb2b8fdc616442a17365b24fcde1a4da81f428377450f05a9cea66da8eb9fcd719c01e394ac3420cea106eba300a06082a8648ce3d0403020347003044022032360b86e049cdd7f2b7047081a7bfa9559db65153844eec4d2ef1d544f8229802205e53bee52fa3d884de346c0d0c13b45e2b0c9511c8a4575a901dac3d68b52a10"
        signature: "cff64e664420fa4f581292d0ba5b279cf481843048e75899222d7cb8d19f00bcb15f851a5278fe3187e336669d1eea4ccb84b2f8f4e7b127da203a4eafe87b3a"
    }
}
```

Error

```typescript
{
    success: false,
    payload: {
        error: string, // error message
        code: number // error code
    }
}
```
