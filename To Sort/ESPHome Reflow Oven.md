## ESPHome Reflow Oven
```yaml
esphome:
  name: heater_controller
  friendly_name: Heater Controller

esp32:
  board: esp32dev

# --- System setup
logger:
api:
ota:
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

# --- Thermocouple (example: MAX31855)
spi:
  clk_pin: GPIO18
  miso_pin: GPIO19
  mosi_pin: GPIO23  # not used by MAX31855

sensor:
  - platform: max31855
    name: "Heater Temperature"
    id: heater_temp
    cs_pin: GPIO5
    update_interval: 1s

# --- Solid State Relay
output:
  - platform: gpio
    pin: GPIO26
    id: heater_relay

switch:
  - platform: output
    name: "Heater SSR"
    output: heater_relay
    id: heater_ssr

# --- PID controller
climate:
  - platform: pid
    name: "Heater PID"
    sensor: heater_temp
    default_target_temperature: 100 °C
    heat_output: heater_relay
    id: heater_pid
    control_parameters:
      kp: 0.5
      ki: 0.02
      kd: 0.1
      min_integral: -0.5
      max_integral: 0.5
    visual:
      min_temperature: 25
      max_temperature: 300
      temperature_step: 1

# --- Globals
globals:
  - id: profile_step
    type: int
    restore_value: no
    initial_value: '0'

  - id: profile_length
    type: int
    restore_value: no
    initial_value: '0'

  - id: running_profile
    type: bool
    restore_value: no
    initial_value: 'false'

# --- Example profiles
# Format: { {Temp°C, Duration seconds}, {...}, ... }
# Add more as needed
substitutions:
  profile1: |-
    { {100, 300}, {150, 600}, {200, 300}, {150, 600} }

  profile2: |-
    { {120, 600}, {180, 600}, {220, 300} }

  profile3: |-
    { {80, 300}, {100, 300}, {120, 300}, {150, 300}, {100, 300} }

# --- Select entity in HA
select:
  - platform: template
    name: "Heat Profile Selector"
    id: profile_selector
    optimistic: true
    options:
      - "Profile 1"
      - "Profile 2"
      - "Profile 3"
    initial_option: "Profile 1"

# --- Profile runner script
script:
  - id: run_profile
    mode: restart
    then:
      - lambda: |-
          // pick profile based on HA selection
          std::string selected = id(profile_selector).state.c_str();
          if (selected == "Profile 1") {
            static const struct { int temp; int duration; } profile[] = ${profile1};
            const int steps = sizeof(profile)/sizeof(profile[0]);
            id(profile_length) = steps;
            id(profile_step) = 0;
            id(running_profile) = true;
            ESP_LOGI("profile", "Running Profile 1 with %d steps", steps);

            while (id(profile_step) < id(profile_length) && id(running_profile)) {
              auto step = profile[id(profile_step)];
              ESP_LOGI("profile", "Step %d: %d°C for %d sec", id(profile_step)+1, step.temp, step.duration);
              id(heater_pid).make_call().set_target_temperature(step.temp).perform();
              delay(step.duration * 1000);
              id(profile_step)++;
            }

          } else if (selected == "Profile 2") {
            static const struct { int temp; int duration; } profile[] = ${profile2};
            const int steps = sizeof(profile)/sizeof(profile[0]);
            id(profile_length) = steps;
            id(profile_step) = 0;
            id(running_profile) = true;
            ESP_LOGI("profile", "Running Profile 2 with %d steps", steps);

            while (id(profile_step) < id(profile_length) && id(running_profile)) {
              auto step = profile[id(profile_step)];
              ESP_LOGI("profile", "Step %d: %d°C for %d sec", id(profile_step)+1, step.temp, step.duration);
              id(heater_pid).make_call().set_target_temperature(step.temp).perform();
              delay(step.duration * 1000);
              id(profile_step)++;
            }

          } else if (selected == "Profile 3") {
            static const struct { int temp; int duration; } profile[] = ${profile3};
            const int steps = sizeof(profile)/sizeof(profile[0]);
            id(profile_length) = steps;
            id(profile_step) = 0;
            id(running_profile) = true;
            ESP_LOGI("profile", "Running Profile 3 with %d steps", steps);

            while (id(profile_step) < id(profile_length) && id(running_profile)) {
              auto step = profile[id(profile_step)];
              ESP_LOGI("profile", "Step %d: %d°C for %d sec", id(profile_step)+1, step.temp, step.duration);
              id(heater_pid).make_call().set_target_temperature(step.temp).perform();
              delay(step.duration * 1000);
              id(profile_step)++;
            }
          }

          id(running_profile) = false;
          ESP_LOGI("profile", "Profile finished or aborted.");

# --- HA control buttons
button:
  - platform: template
    name: "Start Heat Profile"
    on_press:
      - script.execute: run_profile

  - platform: template
    name: "Stop Heat Profile"
    on_press:
      - lambda: |-
          id(running_profile) = false;
          ESP_LOGW("profile", "Profile aborted!");
          
```
     
