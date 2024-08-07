# GMRT_RFI_ANTENNA_MADHAV_SHREYA
Has files that shouldn't be changed or overwrites. 

# Motor Control with Encoder Feedback for Arduino

This project provides an Arduino sketch and a Python GUI application to control a motor with encoder feedback. It allows for precise control of the motor's position and speed, and can handle commands sent via serial communication to set parameters and control the motor.

## Components Used

- Arduino board
- Motor driver (L298N)
- Motor with encoder
- Power supply
- Connecting wires

## Pin Configuration

- **Motor Driver Enable Pin (PWM)**: 9
- **Motor Driver Input 1**: 8
- **Motor Driver Input 2**: 7
- **Encoder Pin A**: 2
- **Encoder Pin B**: 3

## Arduino Code

The provided Arduino sketch includes the following functionality:

- Setup and initialization of pins and interrupts.
- Conversion of pulses to maximum encoder value for a full 360-degree rotation.
- Serial communication to receive commands for setting parameters and controlling the motor.
- Functions to control motor direction, speed, and position based on encoder feedback.

### Commands

The Arduino listens for the following commands sent via serial communication:

- **'A'**: Set antenna angle
- **'P'**: Set pulse count per revolution
- **'G'**: Set gear ratio
- **'F'**: Rotate motor clockwise
- **'B'**: Rotate motor anti-clockwise
- **'D'**: Check rotation direction
- **'R'**: Reset antenna position to zero
- **'S'**: Stop the motor
- **'M'**: Set motor speed

### Functions

- **setup()**: Initializes the pins, serial communication, and encoder interrupts.
- **conversion()**: Converts pulses to maximum encoder value for a 360-degree rotation.
- **updateEncoder()**: Updates the encoder value based on the pulses received.
- **forward(int speed, long requiredPulsesforRotation)**: Rotates the motor clockwise.
- **backward(int speed, long requiredPulsesforRotation)**: Rotates the motor anti-clockwise.
- **stopMotor()**: Stops the motor.
- **directionCheck(long currentEncoderValue)**: Checks the rotation direction of the antenna.
- **requestStop()**: Software interrupt to stop the motor at any point.
- **reset(long requiredPulsesForRotation)**: Resets the antenna position to zero.

## Usage

1. Upload the provided Arduino sketch to your Arduino board.
2. Connect the motor driver, motor, and encoder to the Arduino board as per the pin configuration.
3. Open the Serial Monitor in the Arduino IDE and set the baud rate to 9600.
4. Send commands via the Serial Monitor to control the motor and set parameters.

### Example Commands

- Set antenna angle to 90 degrees:
  ```
  A90
  ```
- Set pulse count per revolution to 200:
  ```
  P200
  ```
- Set gear ratio to 2:
  ```
  G2
  ```
- Rotate motor clockwise:
  ```
  F
  ```
- Rotate motor anti-clockwise:
  ```
  B
  ```
- Check rotation direction:
  ```
  D
  ```
- Reset antenna position to zero:
  ```
  R
  ```
- Stop the motor:
  ```
  S
  ```
- Set motor speed to 150:
  ```
  M150
  ```

## Notes

- Ensure the motor driver and motor are properly connected and powered.
- Adjust the speed value (PWM) as needed for your specific motor and application.
- The encoder values are updated in real-time, and the motor control functions ensure precise movements based on the required pulses for rotation.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

# Motor Control GUI Application

This Python application provides a graphical user interface (GUI) for controlling a motor through an Arduino. 
The GUI allows users to set various motor parameters, control the motor's direction and speed, and read encoder values.
The application communicates with the Arduino via serial communication.

## Features

- **Set Pulses Per Revolution**: Configure the encoder settings.
- **Set Gear Ratio**: Define the gear ratio for the antenna.
- **Set Antenna Angle**: Specify the rotation angle in degrees.
- **Set Clock For Rotation**: Set a timer to control the duration of rotation.
- **Direction Controls**: Rotate the motor clockwise or anticlockwise.
- **Speed Controls**: Set and display the motor's speed.
- **Read Encoder Value**: Read the current value from the encoder.
- **Submit and Reset Parameters**: Submit motor parameters to the Arduino and reset all settings.
- **Dark Mode**: The application features a dark mode theme for better visibility and user experience.
- **Logging**: Log all actions and commands sent to the Arduino for record-keeping and debugging purposes.

## Requirements

- Python 3.x
- PyQt5
- pyserial

## Installation

1. **Clone the Repository**:
    ```bash
    git clone https://github.com/yourusername/motor-control-gui.git
    cd motor-control-gui
    ```

2. **Install Dependencies**:
    ```bash
    pip install PyQt5 pyserial
    ```

## Usage

1. **Connect the Arduino**: Ensure your Arduino is connected to your computer via USB. Adjust the serial port in the code if necessary (default is `/dev/ttyUSB0`).

2. **Run the Application**:
    ```bash
    python motor_control_gui.py
    ```

3. **Use the GUI**:
    - **Encoder Settings**: Enter the pulses per revolution and click "Submit Pulses".
    - **Gear Ratio**: Enter the gear ratio and click "Submit Gear Ratio".
    - **Antenna Angle**: Enter the desired rotation angle and click "Submit Angle".
    - **Set Clock for Rotation**: Enter hours, minutes, and seconds, then click the appropriate buttons to start or stop the timer.
    - **Direction Controls**: Click "Clockwise" or "Anticlockwise" to rotate the motor in the desired direction.
    - **Speed Controls**: Set the speed using the calculated PWM value and click "Speed Set".
    - **Encoder Value**: Click "Read Encoder Value" to read the current encoder value.
    - **Submit Parameters**: Click "Submit Parameters" to send all settings to the Arduino.
    - **Reset Parameters**: Click "Reset Parameters" to clear all settings.
    - **Reset Antenna Position**: Click "Reset Antenna Position" to reset the antenna position to the encoder count of 0.

## File Structure

- `motor_control_gui.py`: Main application code.
- `motor_settings.txt`: File to save and load motor settings.
- `motor_logs/`: Directory to store log files.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contributing

Contributions are welcome! Please fork the repository and create a pull request with your changes.

## Issues

For any questions or feedback, please contact madhavhadge@gmail.com.
