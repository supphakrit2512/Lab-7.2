# Lab-7.2

import RPi.GPIO as GPIO
import time
from RPLCD.i2c import CharLCD

SW1 = 27
SW2 = 17

GPIO.setmode(GPIO.BCM)
GPIO.setup(SW1, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(SW2, GPIO.IN, pull_up_down=GPIO.PUD_UP)

lcd = CharLCD('PCF8574', 0x27)
lcd.clear()

message = "LAB 7"
lcd_columns = 16

current_position = 0
direction = 1

def display_message(position):
    lcd.clear()
    lcd.write_string(" " * position + message)

try:
    display_message(current_position)

    while True:
        if GPIO.wait_for_edge(SW1, GPIO.FALLING):
            if current_position < lcd_columns - len(message):
                current_position += 1
            display_message(current_position)
            time.sleep(0.3)

        elif GPIO.wait_for_edge(SW2, GPIO.FALLING):
            if current_position > 0:
                current_position -= 1
            display_message(current_position)
            time.sleep(0.3)

except KeyboardInterrupt:
    pass

finally:
    GPIO.cleanup()
    lcd.clear()
    print("\nBye..")
