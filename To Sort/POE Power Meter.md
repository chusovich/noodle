#nix #poe #python #esphome #systemd 
## Hardware
### Poe Device

https://www.adafruit.com/product/3847

## Current/Voltage Sensor (INA226)

https://esphome.io/components/sensor/ina226/

https://www.amazon.com/s?k=ina226&crid=15L8SY4PZ0TMF&sprefix=ina22%2Caps%2C233&ref=nb_sb_noss_2

## Python Script
### Prerequisites

Enable I²C:

```bash
sudo raspi-config 
# Interface Options → I2C → Enable
```

Install libraries:

```bash
sudo apt update
sudo apt install -y python3-pip pip3 
install adafruit-circuitpython-ina226 paho-mqtt
```

### Example Script

- Make executable (optional): `chmod +x /home/pi/ina226_mqtt.py`
- Run it: `python3 /home/pi/ina226_mqtt.py`

```python
import time
import board
import busio
from adafruit_ina226 import INA226
import paho.mqtt.client as mqtt

# ------------------
# MQTT configuration
# ------------------
MQTT_BROKER = "192.168.1.10"   # change to your broker IP
MQTT_PORT = 1883
MQTT_BASE_TOPIC = "sensors/ina226"

PUBLISH_INTERVAL = 5  # seconds

# ------------------
# INA226 setup
# ------------------
i2c = busio.I2C(board.SCL, board.SDA)
ina = INA226(i2c)

# Optional: set shunt resistance if needed
# ina.shunt_resistance = 0.1  # ohms

# ------------------
# MQTT client setup
# ------------------
client = mqtt.Client()
client.connect(MQTT_BROKER, MQTT_PORT, 60)
client.reconnect_delay_set(min_delay=1, max_delay=60)#
client.loop_start()

print("INA226 MQTT publisher started")

# ------------------
# Main loop
# ------------------
try:
    while True:
        voltage = ina.bus_voltage        # volts
        current = ina.current            # milliamps
        power = ina.power                # milliwatts

        client.publish(f"{MQTT_BASE_TOPIC}/voltage", voltage)
        client.publish(f"{MQTT_BASE_TOPIC}/current_mA", current)
        client.publish(f"{MQTT_BASE_TOPIC}/power_mW", power)

        print(f"V={voltage:.3f}V  I={current:.2f}mA  P={power:.2f}mW")

        time.sleep(PUBLISH_INTERVAL)

except KeyboardInterrupt:
    print("Stopping INA226 MQTT publisher")

finally:
    client.loop_stop()
    client.disconnect()

```

## NixOS Setup
### Enable I2C on the Pi
In your configuration.nix:
```nix
{
  hardware.i2c.enable = true;

  users.users.pi.extraGroups = [ "i2c" ];
}
```
Test using:
```bash
i2cdetect -y 1
```
### Define a Python Environment
```nix
{
  environment.systemPackages = [
    (pkgs.python3.withPackages (ps: with ps; [
      adafruit-circuitpython-ina226
      paho-mqtt
    ]))
  ];
}
```
Test with: 
```bash
python3 -c "import adafruit_ina226, paho.mqtt.client; print('OK')"
```
### Create a systemd service
In configuration.nix:
```nix
{
  systemd.services.ina226-mqtt = {
    description = "INA226 MQTT Publisher";
    wantedBy = [ "multi-user.target" ];

    serviceConfig = {
      ExecStart = "${pkgs.python3}/bin/python3 /home/pi/ina226_mqtt.py";
      Restart = "always";
      RestartSec = 5;

      User = "pi";
      Group = "i2c";
    };
  };
}
```
- Start using: `sudo systemctl start ina226-mqtt`
- Check status using `systemctl status ina226-mqtt` and `journalctl -u ina226-mqtt -f`