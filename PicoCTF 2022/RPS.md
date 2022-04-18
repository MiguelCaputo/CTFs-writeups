# RPS

## Flavor Text

Here's a program that plays rock, paper, scissors against you. I hear something good happens if you win 5 times in a row.

## How to solve it

We need to win 5 times in a row, we could write a program to break the **strstr** comparation or we can bruteforce it and have fun like:

```
import pyautogui, time

time.sleep(4)

while True:
 pyautogui.typewrite("1")
 pyautogui.press("enter")
 time.sleep(0.5)
 pyautogui.typewrite("paper")
 pyautogui.press("enter")
 time.sleep(0.5)
```

Just run this payload, click on the netcat session and pray that there are 5  "rocks" in a row, when that happens you get the flag.
