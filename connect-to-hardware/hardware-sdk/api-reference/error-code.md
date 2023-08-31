# Error Code

This document provides a comprehensive guide to the error codes used in the Hardware Wallet JS-SDK. These error codes help developers identify and handle various scenarios that may arise when interacting with the hardware wallet.

### 1. Introduction

The Hardware SDK is equipped with a range of error codes that provide insight into the nature of issues encountered during usage. These error codes allow for efficient error handling and troubleshooting in your application.

### 2. Error Code Structure

Each error code is represented by a constant variable in the SDK, which allows easy identification and handling. Additionally, an associated message provides a brief description of the error. You can see all the source code in [here](https://github.com/OneKeyHQ/hardware-js-sdk/blob/728279dc70c5bde56c5d72d86598815f977b0c7f/packages/shared/src/HardwareError.ts#L46).

### 3. Common Errors

* **UnknownError (0):** An unexpected error occurred. Check the message property for more details.

### 4. Device Errors

* **DeviceFwException (101):** Firmware version mismatch.
* **DeviceUnexpectedMode (102):** Device is in an unexpected mode.
* **DeviceListNotInitialized (103):** Device list is not initialized.
* **SelectDevice (104):** Please select the connected device.
* **DeviceNotFound (105):** Device not found.
* **DeviceInitializeFailed (106):** Device initialization failed.
* **DeviceInterruptedFromOutside (107):** Device operation interrupted from outside.
* **DeviceUnexpectedBootloaderMode (108):** Device should be in bootloader mode.
* **DeviceInterruptedFromUser (109):** Device operation interrupted by the user.
* **DeviceCheckDeviceIdError (110):** Device ID in the features is not the same.
* **DeviceNotSupportPassphrase (111):** Device does not support passphrase.
* **DeviceCheckPassphraseStateError (112):** Device passphrase state error.
* **DeviceNotOpenedPassphrase (113):** Passphrase not opened for the device.
* **DeviceOpenedPassphrase (114):** Passphrase is opened for the device.
* **NotInitialized(200):** Device is not initialized.

### 5. Iframe Errors

* **IFrameNotInitialized (300):** IFrame is not initialized.
* **IFrameAleradyInitialized (301):** IFrame is already initialized.
* **IFrameLoadFail (302):** IFrame load failed.
* **IframeTimeout (303):** IFrame initialization timed out.
* **IframeBlocked (304):** IFrame is blocked.

### 6. Method Errors

* **CallMethodError (400):** Runtime error during method execution.
* **CallMethodNotResponse (404):** Method does not respond.
* **CallMethodInvalidParameter (405):** Invalid parameter passed to the method.
* **FirmwareUpdateDownloadFailed (406):** Firmware update download failed.
* **CallMethodNeedUpgradeFirmware (407):** Method not supported, firmware update required.
* **CallMethodDeprecated (408):** Method is deprecated.
* **FirmwareUpdateLimitOneDevice (409):** Only one device allowed during firmware update.
* **FirmwareUpdateManuallyEnterBoot (410):** Manual entry required for firmware update.
* **FirmwareUpdateAutoEnterBootFailure (411):** Failed to automatically enter boot mode for firmware update.
* **NewFirmwareUnRelease (412):** New firmware has not been released yet.
* **UseDesktopToUpdateFirmware (413):** Update firmware using OneKey desktop client.
* **NewFirmwareForceUpdate (414):** Mandatory firmware update required.

### 7. Network Errors

* **NetworkError (500):** Network request error.

### 8. Transport Errors

* **TransportNotConfigured (600):** Transport is not configured.
* **TransportCallInProgress (601):** Transport call in progress.
* **TransportNotFound (602):** Transport not found.
* **TransportInvalidProtobuf (603):** Invalid protobuf for transport.

### 9. Bluetooth Errors

* **BleScanError (700):** BLE scan error.
* **BlePermissionError (701):** Bluetooth permission required.
* **BleLocationError (702):** Location permissions for the application are not available.
* **BleRequiredUUID (703):** UUID is required.
* **BleConnectedError (704):** Connected error is always a runtime error.
* **BleDeviceNotBonded (705):** Device is not bonded.
* **BleServiceNotFound (706):** BLEServiceNotFound: service not found.
* **BleCharacteristicNotFound (707):** BLEServiceNotFound: service not found.
* **BleMonitorError (708):** Monitor Error: characteristic not found.
* **BleCharacteristicNotifyError (709):** Characteristic Notify Error.
* **BleWriteCharacteristicError (710):** Write Characteristic Error.
* **BleAlreadyConnected (711):** Already connected to device.
* **BleLocationServicesDisabled (712):** Location Services disabled.
* **BleTimeoutError (713):** The connection has timed out unexpectedly.
* **BleForceCleanRunPromise (714):** Force clean Bluetooth run promise.
* **BleDeviceBondError (715):** Bluetooth pairing failed.

### 10. Runtime Errors

* **RuntimeError (800):** Runtime error.
* **PinInvalid (801):** Invalid PIN.
* **PinCancelled (802):** PIN entry cancelled by user.
* **ActionCancelled (803):** Action cancelled by user.
* **FirmwareError (804):** Firmware installation failed.
* **ResponseUnexpectTypeError (805):** Response type is not expected.
* **BridgeNetworkError (806):** Bridge network error.
* **BridgeTimeoutError (807):** Bridge network timeout.
* **BridgeNotInstalled (808):** Bridge not installed.
* **PollingTimeout (809):** Polling timeout.
* **PollingStop (810):** Polling stopped.
* **BlindSignDisabled (811):** BlindSign is disabled on the device.
* **UnexpectPassphrase (812):** Unexpected passphrase encountered.
* **FileAlreadyExists (813):** NFT file already exists.
* **CheckDownloadFileError (814):** Check download file error.
* **NotInSigningMode (815):** Not in signing mode.

### 11. Low-Level Transport Errors

* **LowlevelTrasnportConnectError (900):** Low-level transport connect error.

***
