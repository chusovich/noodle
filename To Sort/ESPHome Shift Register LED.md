## ESPHome Shift Register LED
## Setup
```yaml
esphome:
  name: shift_register_array

external_components:
  - source: github://esphome/esphome
    refresh: 0s
  # ... other configurations

sn74hc595:
  - id: 'shift_register_hub'
    data_pin: GPIOXX  # Change to your ESP's Data pin
    clock_pin: GPIOYY # Change to your ESP's Clock pin
    latch_pin: GPIOZZ # Change to your ESP's Latch pin
    sr_count: 2       # Set the number of daisy-chained shift registers
```
### Defining Each LED
```yaml
# Define a switch for each pin on the shift register
switch:
  - platform: gpio
    name: "LED 1"
    pin:
      sn74hc595: shift_register_hub
      number: 0
  - platform: gpio
    name: "LED 2"
    pin:
      sn74hc595: shift_register_hub
      number: 1
  - platform: gpio
    name: "LED 3"
    pin:
      sn74hc595: shift_register_hub
      number: 2
  # ... and so on for all 8 * `sr_count` pins
```
### Effects
```yaml
light:
  - platform: custom
    type: light
    name: "Shift Register LEDs"
    effects:
      - name: "Chase Effect"
        update_interval: 100ms
        lambda: |-
          static int led_index = 0;
          static int direction = 1;

          // Clear all LEDs
          it.sn74hc595.set_shift_data(0);
          it.sn74hc595.write_shift_data();

          // Set the current LED
          it.sn74hc595.set_shift_data(1 << led_index);
          it.sn74hc595.write_shift_data();

          // Update for the next frame
          led_index += direction;
          if (led_index == 7) {
            direction = -1;
          }
          if (led_index == 0) {
            direction = 1;
          }

```