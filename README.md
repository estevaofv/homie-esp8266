Homie for ESP8266
=================

![homie-esp8266](homie-esp8266.jpg)

An Arduino for ESP8266 implementation of [Homie](https://git.io/homieiot), an MQTT convention for the IoT.

## Features

* Automatic connection/reconnection to Wi-Fi/MQTT
* [Cute JSON configuration file](https://github.com/marvinroger/homie-esp8266/wiki/5.-JSON-configuration-file) to configure the credentials of the device
* [Cute API / Web UI / App](https://github.com/marvinroger/homie-esp8266/wiki/6.-Configuration-API) to get information about the device and to remotely send the configuration to the device
* [OTA support](https://github.com/marvinroger/homie-esp8266/wiki/4.-OTA)
* Available in the [PlatformIO registry](http://platformio.org/#!/lib/show/555/Homie)
* Pretty straightforward sketches, a simple light for example:

```c++
#include <Homie.h>

const int PIN_RELAY = D1;

HomieNode light("light", "light");

bool lightOnHandler(String message) {
  if (message == "true") {
    digitalWrite(PIN_RELAY, HIGH);
    Homie.setNodeProperty(light, "on", "true"); // Update the state of the light
    Serial.println("Light is on");
  } else if (message == "false") {
    digitalWrite(PIN_RELAY, LOW);
    Homie.setNodeProperty(light, "on", "false");
    Serial.println("Light is off");
  } else {
    return false;
  }

  return true;
}

void setup() {
  pinMode(PIN_RELAY, OUTPUT);
  digitalWrite(PIN_RELAY, LOW);
  Homie.setFirmware("awesome-relay", "1.0.0");
  light.subscribe("on", lightOnHandler);
  Homie.registerNode(light);
  Homie.setup();
}

void loop() {
  Homie.loop();
}
```

## Requirements, installation and usage

The project is documented on the [Wiki](https://github.com/marvinroger/homie-esp8266/wiki), with a *Getting started* guide and every piece of informations you will need.
