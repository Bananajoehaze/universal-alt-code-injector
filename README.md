


I am currently learning how to build a "BadUSB" device using
a Raspberry Pi Pico. Since my own keyboard layout is German
(QWERTZ), I ran into constant issues with incorrect character mapping and broken special
characters while testing my scripts.
---------------------------------------------------------------------
---------------------------------------------------------------------




Example code from me, its important to have numlock active.

:)



---------------------------------------------------------------------
---------------------------------------------------------------------

import time
import usb_hid
from adafruit_hid.keyboard import Keyboard
from adafruit_hid.keycode import Keycode
from adafruit_hid.keyboard_layout_us import KeyboardLayoutUS

# Hardware-Schnittstellen (Core-Modul)
kbd = Keyboard(usb_hid.devices)
layout = KeyboardLayoutUS(kbd)

def alt_code(number_string):
    """
    Universeller Alt-Code-Übersetzer.
    Zwingt Windows, das Zeichen unabhängig vom Tastaturlayout korrekt zu deuten.
    """
    kbd.press(Keycode.ALT)
    # Keypad-Codes für 0-9 auf dem HID-Layer
    kp = [98, 89, 90, 91, 92, 93, 94, 95, 96, 97]
    for digit in number_string:
        code = kp[int(digit)]
        kbd.press(code)
        kbd.release(code)
    kbd.release(Keycode.ALT)

def ultra_safe_type(text):
    """
    High-Speed Injektion.
    Übersetzt kritische Hacker-Symbole und URL-Parameter in stabile Alt-Codes.
    """
    for char in text:
        if char == ':': alt_code("058")
        elif char == '/': alt_code("047")
        elif char == '.': alt_code("046")
        elif char == '-': alt_code("045")
        elif char == '+': alt_code("043") # Fix für attrib +h
        elif char == '\\': alt_code("092")
        elif char == '>': alt_code("062")
        elif char == '$': alt_code("036")
        elif char == '_': alt_code("095")
        elif char == '?': alt_code("063") # Fix für YT-Links
        elif char == '=': alt_code("061") # Fix für YT-Links
        elif char == '&': alt_code("038")
        elif char == ';': alt_code("059")
        elif char == '|': alt_code("124") # Fix für Powershell Pipe
        elif char == '"': alt_code("034")
        elif char == "'": alt_code("039")
        elif char == ' ': kbd.send(Keycode.SPACE)
        elif char == 'z': layout.write('y') # Layout-Swap
        elif char == 'Z': layout.write('Y')
        elif char == 'y': layout.write('z')
        elif char == 'Y': layout.write('Z')
        else:
            layout.write(char)

# --- ABLAUF-LOGIK ---

kbd.send(Keycode.KEYPAD_NUMLOCK)
time.sleep(0.1)


kbd.send(Keycode.GUI, Keycode.R)
time.sleep(0.1)
