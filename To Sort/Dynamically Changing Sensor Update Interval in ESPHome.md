By default in ESPHome, sensor `update_interval` is static—you define it in YAML, and it can’t be changed dynamically at runtime.

However, there are workarounds to achieve the same result: dynamically control when or how often a sensor updates **while the ESP32 is running**.

## 🔧 Workarounds to Dynamically Control Update Behavior

### 1. **Use `component.update()` In an Automation or lambda**

You can manually trigger a sensor update at any time.

```yaml
sensor:
  - platform: dht
    pin: D2
    temperature:
      name: "Room Temp"
      id: temp_sensor
    humidity:
      name: "Humidity"
    update_interval: never  # prevent automatic updates
```

Now trigger it manually:

```yaml
interval:
  - interval: 60s
    then:
      - component.update: temp_sensor
```

Then you can use a `number` component to control the interval dynamically:

```yaml
number:
  - platform: template
    name: "Sensor Interval (s)"
    id: sensor_interval
    min_value: 10
    max_value: 600
    step: 10
    restore_value: true
    initial_value: 60
```

And dynamically change the interval:

```yaml
interval:
  - id: sensor_updater
    interval: 60s  # this will be updated
    then:
      - component.update: temp_sensor

script:
  - id: update_sensor_interval
    mode: restart
    then:
      - lambda: |-
          auto new_interval = (uint32_t)(id(sensor_interval).state * 1000);
          id(sensor_updater).set_interval(new_interval);
```

Trigger that script whenever `sensor_interval` changes:

```yaml
on_value:
  then:
    - script.execute: update_sensor_interval
```

### 2. **Use Conditional Polling via `if`**

You can always run updates at a fixed rate but **gate the logic** using flags or states, like this:

```yaml
interval:
  - interval: 10s
    then:
      - if:
          condition:
            lambda: 'return id(enable_sensor_updates);'
          then:
            - component.update: temp_sensor
```

Where `enable_sensor_updates` is a `switch:` or `boolean:` you toggle from the web UI or automation.
