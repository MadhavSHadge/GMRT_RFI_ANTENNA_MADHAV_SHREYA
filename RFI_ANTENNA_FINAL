#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Mon Aug 06  11:32:20 2024

@author: madhav
"""

#importing all the library files needed
import sys
import os
from PyQt5.QtWidgets import (
    QApplication, QMainWindow, QWidget, QVBoxLayout, QHBoxLayout, QLabel,
    QLineEdit, QPushButton, QGroupBox, QSpacerItem, QSizePolicy, QMessageBox
)
from PyQt5.QtCore import Qt, QTimer
from PyQt5.QtGui import QPalette, QColor
import serial

# Define the file path for saving motor settings
file_path = '/home/madhav/Documents/FINALS/IMPROVED FROM GUI_VERSION_7/motor_settings.txt'

# GUI main window 
class MotorControlGUI(QMainWindow):
    def __init__(self, motor_control):
        super().__init__()
        self.motor_control = motor_control
        self.pwm_value = None  # Initialize PWM value as None
        self.initUI()
        self.init_serial()
        self.init_timer()

    # For serial communication, declare ports and baudrate here.
    def init_serial(self):
        self.serial_port = serial.Serial('/dev/ttyUSB0', 9600, timeout=1)  # Adjust COM port as needed

    # For periodically updating the encoder value
    def init_timer(self):
        self.timer = QTimer()
        self.timer.timeout.connect(self.update_encoder_value)
        self.timer.start(1000)  # Check every 1000 ms (1 second)

    # For sending commands to Arduino
    def send_serial_command(self, command):
        if self.serial_port.is_open:
            self.serial_port.write(command.encode())
        else:
            QMessageBox.critical(self, "Serial Port Error", "Serial port is not open.")

    # Update encoder value from serial port
    def update_encoder_value(self):
        if self.serial_port.in_waiting:
            data = self.serial_port.readline().decode().strip()
            if data:
                self.encoder_value_label.setText(f'Current Encoder Value: {data}')

    # Creating the GUI of the Antenna Controlling
    def initUI(self):
        self.setWindowTitle('Arduino Motor Control')
        self.setGeometry(100, 100, 800, 600)
        self.setFixedSize(800, 300)
        self.setDarkMode()
        
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        main_layout = QVBoxLayout()
        central_widget.setLayout(main_layout)
    
        # First Row: Set Pulses per Revolution, Set Gear Ratio, Set Antenna Angle
        first_row_layout = QHBoxLayout()
        main_layout.addLayout(first_row_layout)
    
        # Set Pulses per Revolution
        pulses_group = QGroupBox('ENCODER SETTINGS')
        pulses_group.setStyleSheet("""
                                   QGroupBox {
                                       font-weight: bold;
                                       font-size: 13px;
                                       }
                                   """)
        pulses_layout = QVBoxLayout()
        pulses_group.setLayout(pulses_layout)
        
        pulses_title_label = QLabel('Pulses Per Revolution of Encoder')
        pulses_layout.addWidget(pulses_title_label)
    
        self.pulses_entry = QLineEdit()
        pulses_layout.addWidget(self.pulses_entry)
    
        self.submit_pulses_button = QPushButton('Submit Pulses')
        self.submit_pulses_button.clicked.connect(self.submit_pulses)
        pulses_layout.addWidget(self.submit_pulses_button)
    
        first_row_layout.addWidget(pulses_group)
    
        # Set Gear Ratio
        gear_ratio_group = QGroupBox('Set Gear Ratio')
        gear_ratio_group.setStyleSheet("""
                                   QGroupBox {
                                       font-weight: bold;
                                       font-size: 13px;
                                       }
                                   """)
        gear_ratio_layout = QVBoxLayout()
        gear_ratio_group.setLayout(gear_ratio_layout)
        
        gear_ratio_title_label = QLabel('ANTENNA RATIO')
        gear_ratio_layout.addWidget(gear_ratio_title_label)
    
        self.gear_ratio_entry = QLineEdit()
        gear_ratio_layout.addWidget(self.gear_ratio_entry)
    
        self.submit_gear_ratio_button = QPushButton('Submit Gear Ratio')
        self.submit_gear_ratio_button.clicked.connect(self.submit_gear_ratio)
        gear_ratio_layout.addWidget(self.submit_gear_ratio_button)
    
        first_row_layout.addWidget(gear_ratio_group)
    
        # Set Antenna Angle
        pulse_group = QGroupBox('Set Antenna Angle')
        pulse_group.setStyleSheet("""
                                   QGroupBox {
                                       font-weight: bold;
                                       font-size: 13px;
                                       }
                                   """)
        pulse_layout = QVBoxLayout()
        pulse_group.setLayout(pulse_layout)
        
        pulse_title_label = QLabel('ROTATION ANGLE')
        pulse_layout.addWidget(pulse_title_label)
    
        pulse_label = QLabel('Enter Angle In Degrees:')
        pulse_layout.addWidget(pulse_label)
        
        self.pulse_entry = QLineEdit()
        pulse_layout.addWidget(self.pulse_entry)
    
        self.submit_angle_button = QPushButton('Submit Angle')
        self.submit_angle_button.clicked.connect(self.submit_angle)
        pulse_layout.addWidget(self.submit_angle_button)
        
        first_row_layout.addWidget(pulse_group)

        # Second Row: Direction Controls, Speed Controls, Encoder Value
        second_row_layout = QHBoxLayout()
        main_layout.addLayout(second_row_layout)

        # Direction Controls
        direction_group = QGroupBox('Direction Controls')
        direction_group.setStyleSheet("""
                                   QGroupBox {
                                       font-weight: bold;
                                       font-size: 13px;
                                       }
                                   """)
        direction_layout = QVBoxLayout()
        direction_group.setLayout(direction_layout)
    
        self.forward_button = QPushButton('Clockwise')
        self.forward_button.clicked.connect(self.forward)
        direction_layout.addWidget(self.forward_button)
    
        self.backward_button = QPushButton('Anticlockwise')
        self.backward_button.clicked.connect(self.backward)
        direction_layout.addWidget(self.backward_button)
    
        self.direction_button = QPushButton('Direction Find')
        self.direction_button.clicked.connect(self.direction_count)
        direction_layout.addWidget(self.direction_button)
    
        second_row_layout.addWidget(direction_group)
    
        # Speed Controls
        motor_controls_group = QGroupBox('Speed Controls')
        motor_controls_group.setStyleSheet("""
                               QGroupBox {
                                   font-weight: bold;
                                   font-size: 13px;
                                   }
                               """)
        motor_controls_layout = QVBoxLayout()
        motor_controls_group.setLayout(motor_controls_layout)

        self.speed_label = QLabel('Enter Speed (PWM 0-255):')
        motor_controls_layout.addWidget(self.speed_label)

        self.speed_entry = QLineEdit()
        motor_controls_layout.addWidget(self.speed_entry)

        self.speed_set_button = QPushButton('Speed Set')
        self.speed_set_button.clicked.connect(self.set_speed)
        motor_controls_layout.addWidget(self.speed_set_button)
        
        second_row_layout.addWidget(motor_controls_group)
    
        # Encoder Value
        encoder_group = QGroupBox('Encoder Value')
        encoder_group.setStyleSheet("""
                                   QGroupBox {
                                       font-weight: bold;
                                       font-size: 13px;
                                       }
                                   """)
        encoder_layout = QVBoxLayout()
        encoder_group.setLayout(encoder_layout)
    
        self.encoder_value_label = QLabel('Current Encoder Value: N/A')
        encoder_layout.addWidget(self.encoder_value_label)
    
        self.read_encoder_button = QPushButton('Read Encoder Value')
        self.read_encoder_button.clicked.connect(self.read_encoder)
        encoder_layout.addWidget(self.read_encoder_button)
    
        second_row_layout.addWidget(encoder_group)

        # Third Row: Submit Parameters, Reset Parameters, Reset Antenna Position
        third_row_layout = QHBoxLayout()
        main_layout.addLayout(third_row_layout)
    
        self.submit_button = QPushButton('Submit Parameters')
        self.submit_button.clicked.connect(self.submit_parameters)
        third_row_layout.addWidget(self.submit_button)
    
        self.reset_button = QPushButton('Reset Parameters')
        self.reset_button.clicked.connect(self.reset_all)
        third_row_layout.addWidget(self.reset_button)
    
        self.reset_antenna_button = QPushButton('Reset Antenna Position')
        self.reset_antenna_button.clicked.connect(self.reset_antenna_position)
        third_row_layout.addWidget(self.reset_antenna_button)

        # Spacer
        spacer = QSpacerItem(20, 40, QSizePolicy.Minimum, QSizePolicy.Expanding)
        main_layout.addItem(spacer)

        # Initialize settings from file
        self.read_motor_settings()

    # This will set the antenna position to encoder count of 0
    def reset_antenna_position(self):
        self.pulse_entry.clear()
        self.send_serial_command('R')
        QMessageBox.information(self, "Reset Antenna Position", "Antenna position has been reset.")
        self.motor_control.write_log("Antenna position reset")

    # Read files from the encoder
    def read_motor_settings(self):
        if os.path.exists(file_path):
            with open(file_path, 'r') as file:
                lines = file.readlines()
                if len(lines) >= 2:
                    self.pulse_entry.setText(lines[1].strip())

    # Send pulses per revolution
    def submit_pulses(self):
        pulses_per_rev = self.pulses_entry.text().strip()
        self.send_serial_command(f"P:{pulses_per_rev}")

    # Send the gear ratio
    def submit_gear_ratio(self):
        gear_ratio = self.gear_ratio_entry.text().strip()
        self.send_serial_command(f"G:{gear_ratio}")

    # Send angle entered
    def submit_angle(self):
        angle = self.pulse_entry.text().strip()
        self.send_serial_command(f"A:{angle}")

    # Error handling for speed PWM
    def set_speed(self):
        speed_text = self.speed_entry.text()
        try:
            speed = int(speed_text)
            if 0 <= speed <= 255:
                command = f'M:{speed}'  # Sending 'M' command with speed value to Arduino
                self.send_serial_command(command)
                self.speed_label.setText(f'Speed Set: {speed}')
            else:
                self.speed_label.setText('Error: Value out of range (0-255)')
        except ValueError:
            self.speed_label.setText('Error: Invalid input. Please enter a number between 0 and 255.')

    # Handle forward button click event
    def forward(self):
        self.send_serial_command('F')
        QMessageBox.information(self, "Direction", "Motor is moving forward.")
        self.motor_control.write_log("Motor moving forward")

    # Handle backward button click event
    def backward(self):
        self.send_serial_command('B')
        QMessageBox.information(self, "Direction", "Motor is moving backward.")
        self.motor_control.write_log("Motor moving backward")

    # Read encoder value
    def read_encoder(self):
        self.send_serial_command('E')  # Command to read encoder value
        # Data will be updated asynchronously by the timer

    # Handle direction count button click event
    def direction_count(self):
        self.send_serial_command('D')
        QMessageBox.information(self, "Direction Count", "Direction count command sent.")
        self.motor_control.write_log("Direction count command sent")

    # Reset all parameters to default
    def reset_all(self):
        self.pulses_entry.clear()
        self.gear_ratio_entry.clear()
        self.pulse_entry.clear()
        self.speed_entry.clear()
        QMessageBox.information(self, "Reset", "All parameters have been reset.")
        self.motor_control.write_log("All parameters reset")

    # Handle settings submission
    def submit_parameters(self):
        self.submit_pulses()
        self.submit_gear_ratio()
        self.submit_angle()
        self.set_speed()

    # Dark mode settings
    def setDarkMode(self):
        palette = QPalette()
        palette.setColor(QPalette.Window, QColor(53, 53, 53))
        palette.setColor(QPalette.WindowText, Qt.white)
        palette.setColor(QPalette.Base, QColor(25, 25, 25))
        palette.setColor(QPalette.AlternateBase, QColor(53, 53, 53))
        palette.setColor(QPalette.ToolTipBase, Qt.white)
        palette.setColor(QPalette.ToolTipText, Qt.white)
        palette.setColor(QPalette.Text, Qt.white)
        palette.setColor(QPalette.Button, QColor(53, 53, 53))
        palette.setColor(QPalette.ButtonText, Qt.white)
        palette.setColor(QPalette.BrightText, Qt.red)
        palette.setColor(QPalette.Highlight, QColor(142, 45, 197).lighter())
        palette.setColor(QPalette.HighlightedText, Qt.black)
        self.setPalette(palette)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    motor_control = None  # This should be replaced with the actual motor control object if used
    mainWin = MotorControlGUI(motor_control)
    mainWin.show()
    sys.exit(app.exec_())
