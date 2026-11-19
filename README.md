# Alvik Sim Hub

Simulators for Arduino Alvik robot training, made to prepare for the Arduino Day competition. Three simulator pages live in one file. Open `index.html` in a browser and it runs right away, with no install and no internet connection needed.

Online : https://irap.club/arduino-alvik-sim/

## General use

| Task | How |
|---|---|
| Switch page | Click a tab at the top, or press the left and right arrow keys while the tab bar is focused |
| Open a page directly | Add `#arena`, `#tap` or `#linetrack` to the end of the link |
| Light or dark mode | Click the sun or moon icon at the top right. The choice applies to every page and is remembered |
| Fullscreen | Click the fullscreen icon at the top right to hide the top bar. Press `Esc` or the button in the corner to leave |
| Come back later | The last opened page is remembered, and each page keeps its state when you switch tabs |

---

## 1. Coding (`#arena`)

Lift & Park game field, 800 x 800 mm. The robot must lift a stand until all 4 legs are off the ground, then park it in the blue area. Park all 3 stands to finish. The screen shows the fields of Team A and Team B side by side.

### Match controls

| Control | Purpose |
|---|---|
| Field layout | Choose layout 1 triangle, layout 2 right row, or layout 3 scattered |
| Random field | Pick a random layout, the same way as on match day |
| Start Setup | Start the 60 second Setup timer, followed by the 90 second match |
| Start match / Skip Setup | Skip Setup and start the match at once |
| Watch two team demo | Both teams play automatically to show how the mission is done |
| Resume / Restart | Continue from a pause, or reset the field |
| Team A sim speed | Set the Team A robot speed (mm/s) |
| Playback speed | Run the simulation at 1x, 2x or 4x |
| Top view | Switch the field between the 3D view and the top view |

The clock at the top right shows the current phase and the time left. Each team card shows parked stands, the weight being lifted, the time to leave the red box, the time of the first lift and the finish time. The result bar tells which team won and by which rule.

### Play modes

| Mode | Team A | Team B |
|---|---|---|
| C++ code | Runs the code written on this page | Plays automatically |
| Manual | Driven with the keyboard | Plays automatically |
| Two team demo | Plays automatically | Plays automatically |

Team B runs at 92% of the Team A speed, to use as a time reference.

### Keyboard driving

| Key | Effect |
|---|---|
| Arrow keys or `W` `A` `S` `D` | Drive the robot along the directions of the field map |
| `Space` | Lift or put down a stand (the robot must be centered under it) |
| `P` | Pause or resume |

### Arduino C++ code

Write code the same way as in the Arduino IDE (Integrated Development Environment), with `setup()` and `loop()`, and call commands through the `alvik` object.

| Part | Purpose |
|---|---|
| Examples | Load example code, such as the full 3 stand mission, the basics (drive, lift, put down, follow a line), reading sensors to Serial, an empty template, and library examples (drive, line_follower, hand_follower, hot_wheels, color_calibration, modulino) |
| Compile & Run | Check the code and run it on the field (shortcut `Ctrl` + `Enter` or `Cmd` + `Enter`) |
| Stop | Stop the running code |
| Download .ino | Save the code as an `.ino` file to open in the Arduino IDE |
| Open file | Load an `.ino` file from the computer |
| Status bar | Shows the compile result, the line count and the cursor position |

Code is saved in the browser automatically. Press `Tab` to indent.

### Serial Monitor

Shows messages from `Serial.print()` with the `millis()` time, and can send text into the code (`Serial.available()`, `Serial.read()`, `Serial.readStringUntil()`, `Serial.parseInt()`). The line ending can be New Line, No line ending or CR + LF.

### Live sensor panel

Shows the values the code reads at that moment, and can draw them on the field.

| Value | Source |
|---|---|
| Line L C R | `get_line_sensors()` (black floor about 900, white floor about 40) |
| ToF (Time of Flight) 5 zones and top / bottom | `get_distance()`, `get_distance_top()`, `get_distance_bottom()` in cm |
| Color | `get_color_label()` gives white, black, blue or red |
| Pose | `get_pose()` x, y and angle θ from odometry |
| IMU (Inertial Measurement Unit) yaw | `get_orientation()` |
| Wheels | Left and right wheel speed in RPM (Revolutions Per Minute) |
| Servo A/B and lift height | Servo angle and the height the stand is lifted |
| Collision | Reports hits on stand legs or the field edge |

### Touch buttons and simulated events

There are 7 touch buttons like on the robot (✕, ▲, ✓, ◀, ●, ▶, ▼) to press while code runs, and an option to press OK automatically at start. There are also simulated "Lift the robot" for `get_lifted()` and "Shake" for `get_shake()`.

### Lifter, Team B and realism settings

| Setting | Use |
|---|---|
| Lifter servo port | Connect the lifter to Servo A (`alvik.servo_A`) or Servo B (`alvik.servo_B`) |
| Team B | Turn the demo AI on to compare times, or off to practice alone |
| Servo angle down / up | Angles that put the lifter at the bottom and lift a stand |
| Max lift height | How high the lifter can lift (mm) |
| Servo speed | Servo turning speed (degrees per second) |
| Left / right wheel mismatch | 0% ideal, 0.5% small, 1% medium, 3% large. Tests whether dead reckoning code can still park |
| Lift tolerance off center | The largest offset from the stand center that can still lift (mm) |

Changed values take effect on the next run.

### Supported functions

Same names as the Arduino_Alvik 1.1 library.

| Group | Functions |
|---|---|
| Motion | `move()`, `rotate()`, `drive()`, `set_wheels_speed()` (max 70 RPM), `brake()`, `is_target_reached()`, `get_pose()`, `reset_pose()`, `left_wheel` / `right_wheel` |
| Sensors | `get_line_sensors()`, `get_distance()`, `get_distance_top()`, `get_distance_bottom()`, `get_color_label()`, `get_color()`, `get_color_raw()`, `get_orientation()`, `get_gyros()`, `get_touch_ok()` and the other buttons |
| Lifter, LEDs and Arduino | `servo_A.set_position()`, `set_servo_positions()`, `left_led.set_color()`, `delay()`, `millis()`, `Serial.print()`, `Serial.printf()`, `map()`, `constrain()`, `abs()`, `random()`, `sin()` |
| FreeRTOS | `xTaskCreate()`, `vTaskDelay()`, `vTaskDelete()`, `pdMS_TO_TICKS()` |
| Modulino | ModulinoPixels, ModulinoDistance, ModulinoButtons, ModulinoBuzzer, ModulinoKnob, ModulinoMovement, ModulinoThermo |

Supported types are `int`, `float`, `bool`, `String`, arrays, `struct`, `enum`, `#define` and `static`. Pointers, classes and other libraries are not supported. ESP-NOW, Wi-Fi, micro-ROS and firmware updates need the real board.

---

## 2. Planner (`#tap`)

Plan a path by clicking on the top view field map (in cm). The page works out the `rotate()` angle and the `move()` distance, then builds Arduino_Alvik code.

### Modes

| Mode | Use |
|---|---|
| Path | Click where the robot should go. Each click adds a rotate and a move from the last position |
| Measure | Click two points to measure a distance on the field |
| Move start | Change the robot start position |

### Field options

- **Field :** layout 1 triangle, layout 2 right row, or layout 3 scattered
- **Snap :** snap the click to a line crossing when close, every 1 cm, every 5 cm, or no snap
- **Allow reverse :** if the target is behind, drive backwards instead of turning almost a full circle

The "If you click here" panel shows the turn angle (positive turns left) and the distance before you click, with the robot's current position and heading. The buttons turn left 90°, turn right 90°, +15° and −15° adjust the heading, and "Standard start" puts the robot back in the middle of the red box.

### Command list

Shows every command in order, with the total distance and an estimated time. Numbers in the list can be edited directly.

| Button | Purpose |
|---|---|
| + Manual move | Add a move with a custom distance |
| + Lift / + Put down | Add a lift or put down step |
| Undo | Remove the last command (or press `Backspace`) |
| Clear | Remove all commands |

### Run simulation

Press "Run" to watch the robot follow the commands, at 1x, 2x or 4x. The "turn error" and "move error" sliders simulate errors in rotate and move. The dashed orange line is the path the robot really takes with those errors, so you can see how far the end point drifts.

### Arduino_Alvik code

- **Commands only :** just the `rotate()` and `move()` lines, to paste into existing code
- **Full sketch :** a complete program with `setup()` and `loop()`
- **Copy :** copy the code for the Arduino IDE
- **Run in Coding :** send the code and the field layout to the Coding page and run it at once

---

## 3. LineTrack (`#linetrack`)

Teaches line following with 3 IR (Infrared) sensors. Each sensor reads 0 or 1 (1 means it sees the black line), which gives 2³ = 8 states.

### Modes

| Mode | Use |
|---|---|
| Manual | Slide the robot off the line to the left or right, and watch the IR values and the code decision |
| Run | Let the robot follow the line by itself, and watch how it steers back |

### Controls

- Drag the robot on the field, use the slider, or press the left and right arrow keys
- Preset positions : center, slightly left, far left, slightly right, far right, off the line
- While running : push left, push right, pause, put back on the line, and set the sim speed

### What is shown

| Part | Meaning |
|---|---|
| IR lights on the robot | Left, center and right values, and the 8 state table with the current state highlighted |
| Decision | What the robot does (straight, soft turn, hard turn, search for the line) and the left / right wheel speeds |
| lastSide | The side where the line was last seen, used to pick the search direction when the line is lost |
| if-else code | Highlights the condition running at that moment |
| PWM (Pulse Width Modulation) settings 0 to 255 | Set the wheel speed for each case, then press "Copy as code" or "Defaults" |
