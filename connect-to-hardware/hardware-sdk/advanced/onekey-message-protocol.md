# OneKey Message Protocol

This page describes legacy Protocol V1 framing. It is not the Protocol V2 frame format. Detect the protocol through the SDK transport; do not infer it from the USB descriptor or device name.

In this V1 framing, messages are split into 64-byte packets.

The first packet has the following structure:

| offset | length | type          | contents                                                                                               |
| ------ | ------ | ------------- | ------------------------------------------------------------------------------------------------------ |
| 0      | 3      | char\[3]      | '?##' magic constant                                                                                   |
| 3      | 2      | BE uint16\_t  | numerical [message type](https://github.com/OneKeyHQ/firmware/blob/touch/common/protob/messages.proto) |
| 5      | 4      | BE uint32\_t  | message size                                                                                           |
| 9      | 55     | uint8\_t\[55] | first 55 bytes of message encoded in Protocol Buffers (padded with zeroes if shorter)                  |

Subsequent packets have the following structure:

| offset | length | type          | contents                                                                               |
| ------ | ------ | ------------- | -------------------------------------------------------------------------------------- |
| 0      | 1      | char\[1]      | '?' magic constant                                                                     |
| 1      | 63     | uint8\_t\[63] | following bytes of message encoded in Protocol Buffers (padded with zeroes if shorter) |
