# LogiLink-PDU8P01-ESPHome
Yaml config that converts the LogiLink PDU8P01 to ESPHome, including power measurement

I ended up using the Waveshare ESP32 S3 ETH (can be found here https://www.waveshare.com/esp32-s3-eth.htm)

It has enough GPIO pins despite also having an Ethernet board attached and supports PoE (which i did not use in my case since the PDU is always powered anyway).
