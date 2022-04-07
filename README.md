# LearnBot Simulation Protocol

The LearnBot Simulation Protocol aims to standardize the communication between the **Logical Simulator** ([PiCode](https://picode.com.br) side) and the **Physical Simulator** ([Oorbit](https://oorbit.com.br) side) during the LearnBot simulation.

The image below illustrates the simulation flow
![Simulation Flow](./assets/code-simulation-flow.png)

## State

The simulation state is managed by the **Logical Simulator** and upon changes an [state message](#state-message) is sent to the **Physical Simulator**.

### State Message

The [state message](#state-message) contains information about the current state of the components used by the **Physical Simulator**. For simplicity, the message (see below) is represented in the YAML format. However, the actual implementation is a JavaScript Object.

```yaml
simulation: board # or circuit
type: LearnBot v1.2
data:
  components:
    builtin:
      inputs:
        temperature: # 0 to 100%
        ldr: # 0 to 100%
        button_0: # 0 or 1 (B0)
        button_1: # 0 or 1 (B1)
        button_2: # 0 or 1 (B2)
        button_3: # 0 or 1 (B3)
        button_4: # 0 or 1 (BT2)

      outputs:
        buzzer: # 0 to 100%
        led_0: # 0 to 100% (pin 9)
        led_1: # 0 to 100% (pin 10)
        led_2: # 0 to 100% (pin 11)
        led_3: # 0 or 1    (pin 12)
        led_4: # 0 or 1    (pin 13)
        led_rgb: [R, G, B] # R, G and B are numeric values ranging from 0 to 100%
        led_power: # 0 or 1

    external:
      inputs:
        ultrasonic:
          value: # 0 to 100%
          trigger_pin: # 7 or 8
          echo_pin: # 7 or 8

      outputs:
        servo_motor_0:
          value: # 0 to 100%
          signal_pin: # 3 or 11

        servo_motor_1:
          value: # 0 to 100%
          signal_pin: # 3 or 11

        dc_motor_0: # MA
          speed: # 0 to 100% (pin 5)
          direction: # 0 or 1 (clockwise or counter-clockwise)
          positive_pin: # 9 or 10
          negative_pin: # 9 or 10

        dc_motor_1: # MB
          speed: # 0 to 100% (pin 6)
          direction: # 0 or 1 (clockwise or counter-clockwise respectively)
          positive_pin: # 4 or 12
          negative_pin: # 4 or 12
```

## Events

Events are managed by the **Physical Simulator** and upon changes a message in the [state message](#state-message) or [event message](#event-message) format is sent to the **Logical Simulator**.

### Event Message

The [event message](#event-message) contains only information about the components that changed their value or configuration (e.g. pin mapping). For simplicity, the message (see below) is represented in the YAML format. However, the actual implementation is a JavaScript Object.

```yaml
simulation: board # or circuit
type: LearnBot v1.2
data:
  # optional
  components:
    - name: button_1
      type: builtin
      value: 0 # 0 or 1

    - name: servor_motor_0
      type: external
      signal_pin: 3 # 3 or 11

    - name: ultrasonic
      type: external
      value: 38 # 0 to 100%
      trigger_pin: 7 # 7 or 8
      echo_pin: 8 # 7 or 8

  # optional
  loading:
    - progress: 0 # 0 to 100%
```
