## Setup

Derived from Amazon Web Services IoT MQTT Subscribe/Publish Example
This is an adaptation of the [AWS IoT C SDK](https://github.com/aws/aws-iot-device-sdk-embedded-C) "subscribe_publish" example for ESP-IDF.

Put thing certificates (ID-pem\*) and root CA in `main/certs`. Rename them:

- remove `ID-` prefix
- `mv root-ca.crt aws-root-ca.pem`

Compile project

```bash
cd ind_light
make menuconfig
```

Here choose:

- _Component Config_ > _Amazon Web Services IoT Platform_ (dev thing endpoint should already be setup)
- _Serial Flasher Config_ > _Default Serial Port_ (default is `/dev/ttyUSB0` for Linux)

```bash
make flash
```

## Testing

```bash
make monitor
```

in a 2nd terminal

Named animation

```bash
aws --region eu-west-1 lambda invoke \
  --function-name indus_light_controller \
  --payload '{"target":"poc","payload":{"anim":"red"}}' \
  --invocation-type Event /tmp/lambda.out
```

Custom animation (here: red light on)


```bash
aws --region eu-west-1 lambda invoke \
  --function-name indus_light_controller \
  --payload '{"target":"poc","payload":{"custom_anim":{"l":["r"]}}}' \
  --invocation-type Event /tmp/lambda.out
```
