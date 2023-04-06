import serial
import time

# Configure the serial communication with the GSM module
ser = serial.Serial('/dev/ttyUSB0', 9600, timeout=1)
ser.flush()

# Function to send AT commands to the GSM module
def send_command(command, timeout=1):
    ser.write((command + '\r\n').encode())
    time.sleep(timeout)
    response = ser.read(ser.in_waiting).decode('utf-8')
    return response

# Function to send an SMS message
def send_sms(number, message):
    # Configure the GSM module to send SMS messages
    send_command('AT+CMGF=1')
    send_command('AT+CMGS="{}"'.format(number))
    ser.write((message + "\r\n").encode())
    ser.write(bytes([26]))
    time.sleep(1)
    response = ser.read(ser.in_waiting).decode('utf-8')
    return response

# Example usage
phone_number = "+1234567890"
message = "Hello from Python!"
send_sms(phone_number, message)
