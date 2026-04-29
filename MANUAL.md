# ZMK Split Keyboard Manual

This article focuses on the usage method of split keyboard based on ZMK firmware. Refer to the ZMK document and make modifications based on the configuration library provided here.

Root repository for Eyelash Corne: https://github.com/a741725193/zmk-new_corne

## Usage 

### Power / Charging 
Corne and Sofle keyboards have a toggle switch (power switch）on the side of the keyboard, left and right both have one on each side . They are all turned to the lower side to be in the open state, and the switch needs to be turned on when charging. 
The charging time is generally 6-8 hours, depending on the battery capacity. When charging, the green indicator light stays on. Charging completed, the green indicator light goes out. 
When the power switch is disconnected and USB power is connected, the green light flashes.

### Restart / Flashing
There will also be a press switch near the keyboard switch, pressing it will restart the keyboard.
Plug in the USB cable and press the keyboard twice in a row within 0.5 seconds to enter firmware flashing mode. When flashing firmware mode, the blue light is in a breathing state. The firmware flashing mode is not connected to USB, and the blue light flashes rapidly. 
After connecting the keyboard to USB and entering firmware flashing mode, the keyboard will turn into a USB disk in your computer. Put the left and right firmware into the virtual USB disk of the two keyboards ,then complete the firmware update. 

There is no need to distinguish the order of firmware flashing between the left and right keyboards.

### Pairing
After the keyboard is turned on, the left and right hands will automatically connect, and the left hand keyboard is the main keyboard. The pairing process of the left and right keyboards is completed during the first firmware update. Subsequent firmware updates will not clear the configuration information of the left and right hand connections. So after updating the firmware separately, the left and right keyboards will still remain connected. 
If you need to reset the connection status of your left and right hands, you can flash the reset firmware separately and clear the pairing information. After clearing the pairing information and flashing in the keyboard firmware that needs to be updated, the left and right hands will automatically connect to another split keyboard that can be searched at this time but has not yet been connected.

The method of connecting the keyboard to the computer. The left hand is the main keyboard. After turning on the power switch, add a new device on the Bluetooth page of the computer or phone, or click on the option named 'corner/sofle' to complete the automatic connection. 
When the connection is abnormal, it is necessary to clear the Bluetooth configuration information and then reconnect. To clear the configuration information, press BT_CLEAR_ALL (on the second layer of the keyboard, a combination of keys is required) to complete the operation of clearing the bluetooth configuration information. Then reconnect. 

The keyboard can store 4-5 Bluetooth configuration files. Each file can be connected to a computer/phone. To achieve the requirement of controlling multiple computers with one keyboard. The current configuration file will be displayed on the keyboard screen. The meanings of various icons on the screen will be introduced later. It should be noted that when keyboard configuration 1 is connected to a computer, and the user accidentally touches the config2 keycode afterwards.Then your keyboard will not send the keycode to your computer.The serial number of the configuration file can be displayed on the
screen.

### Keymap
The functions of each button need to be seen in the keymap diagram, and many buttons are
implemented using combination keys. You can view it in the product introduction of the keyboard purchase, and screenshots are attached in the folder. Users can also directly browse the button icons of the GitHub repository.

For example, to enter layer 1, you need to press the key &mo1, and layer 1 can be
named the number layer or other names. After pressing the layer change key, the name of this layer will be displayed below the main keyboard screen. We will introduce how to modify the keymap later.

## Gotchas

### Bluetooth
BT_CLEAR_ALL cleared and reconnected.

**Clear Bluetooth connection**: 
In case of abnormal Bluetooth connection, it can be BT_SEL_0 BT_SEL_1 BT_SEL_2 - these   are the Bluetooth configuration files that the keyboard can store. 

### Power output switch
RGB lights have a characteristic that the small chips inside the beads will continuously consume power. Even if the RGB lights are turned off. So, the default setting of the keyboard is to automatically cut off the positive power supply of the RGB lights when they are turned off. But the keyboard retains the keycode to enable output. 
If the user accidentally touches the EP_ON keycode, the positive power supply of the RGB light will be turned on. It will consume additional power. Delete the EP_ON keycode when editing the keymap to avoid accidentally touching it. In daily use, turning on or off the RGB keyboard will automatically activate or deactivate the output. If you find that the keyboard battery life is poor, it is possible that you accidentally touched EP_ON.

More RGB control key codes can be viewed in the images in the document or checked on the ZMK official website.

RGB_ON RGB_OFF turn on and turn off the RGB

### Bluetooth and USB channel selection
As long as there is a plug inserted into the USB type C port,the keycodes will not be sent to the Bluetooth channel. So when charging the keyboard, keycodes will not sent to the Bluetooth channel. If charging the keyboard and we want to send the key code to
Bluetooth, we can use the OUT_BL keycode to force the use of the Bluetooth channel.
OUT_BL sent to the Bluetooth channel forcibly OUT_USB keycode sent to the USB channel

### Keyboard state control
- &Soft_off keyboard immediately enters soft shutdown state
- &sys_deset restart keyboard
- &The bootloader enter the bootloader flashing state, or you can enter it by double clicking the reset physical switch

## Modifiers
On the shift layer of a regular keyboard, pressing shift+the number "1" will output "!", while on a keyboard without numbers like Corne, you can directly use the key combination to input symbols such as "!" &Kp EXCL "!" &kp AT "@" &kp HASH "#" and so on

The corresponding relationship is as follows, which can be viewed in the GitHub repository and illustrated in the README document.

### Tap dance

In the keymap, you can see the key &td0, it defined at the beginning of the keymap file.

```
td0: td0 {
compatible = "zmk,behavior-tap-dance";
label = "TD0";
#binding-cells = <0>;
bindings = <&kp LEFT_SHIFT>, <&kp CAPS>;
```

The meaning is that short and long presses are both for left shift function, and double clicking is for capslock function. 
Users can add TD1, TD2, modify TD0, or move to other button positions on keyboard.

### Layers
For Corne, the thumb key code on the keyboard is &lt 3 SPACE , and its function is to long press to enter layer 3 and short press to output the space keycode. The right hand &lt 3 ENTER, it means short press to output ENTER and long press to enter layer 3. 
For the new version of Corne and Sofle , pressing the left knob is also the space key, and pressing the right joystick is the enter key. 
If you need others functions, you can also change it by yourself.

### Knob and Joystick
The function of rotating the knob is to adjust the volume, pressing it will sent SPACE to PC.The joystick function is direction keys in the first layer, and pressing the key is ENTER. 
The other layers simulate mouse movement. Pressing is the left mouse button. The name of the knob is EC11. The other layers of EC11 serve the function of rolling mouse wheels. 
Although the joystick has the function of a mouse, it is far less user-friendly than a mouse. So it is only a temporary backup and cannot replace the mouse fully .

## Hardware
Both Corne and Sofle wireless series keyboards are equipped with the nrf52840 MCU and use
ZMK firmware. The new version of the Corne is equipped with a 1500mAh (504060) lithium battery, while the Sofle is equipped with a 2000mAh lithium battery (505060). The battery interface is a PH2.0 socket. 
The green indicator LED is the charging indicator function. If it stays on, it means charging is in progress. If it flashes, it means the battery is not connected or the power switch is not turned on. If the green LED turn off, it means charging is complete. 

It is not necessary to frequently turn off/on the power switch during normal use. Only when the keyboard is expected to be unused for more than six months, should the power be turned off to avoid damage to the lithium battery.

### Status LEDs
The blue LED indicates the working status of the main control MCU. The breathing status
indicates that the chip is in BootLoader state and the USB has been connected to the computer.
Fast flashing indicates that the chip is in BootLoader state, but unable to establish a connection with the computer. 
After the main control chip starts up normally and runs the ZMK firmware, it
should be turned off.
The indicator LED for the new version of the Corne and sofle are located at the USB-C interface, which can be vaguely seen.
The keyboard toggle switch controls the connection between the battery and the main control chip. Usually, flipping to the downside is in the open state.
The press switch on the side of the keyboard is reset. Pressing it twice can put the keyboard into bootLoader mode for firmware updates.

The keyboard screen cover is made of tempered glass material and is fixed with M2 * 2 screws for easy maintenance. In this way, when replacing the positioning plate in the future, it will be convenient to remove the tempered glass panel. All embedded nuts on the keyboard use the M2 * 2 * 3.2 specification. You can also use M2 * 2 * 3 copper nuts

### Other specs
- The screws used to fix the keyboard PCB are M2 * 4, and the embedded copper nuts are also M2 * 2 * 3.2 in size. The screws used to fix the positioning board and PCB are M2 * 4 self tapping screws.
- The keyboard foot pad specification is 10 * 2 rubber foot pad.
- The main control core board equipped on the keyboard is 52840micro.
- The key hardware matrix can be obtained by viewing dtsi and dts files. They are corne.dtsi, corne_left.dts and corne_right.dts. The file location of Sofle is the same, with the name sofle.dtsi solfe_left.dts

> If you have hardware issues you can contact 380465425@qq.com

## Troubleshooting
1. The keyboard is unresponsive. Please check if the power switch is turned on.
2. Not fully charged. Please check if the switch is turned on and the charging status can be determined by the green indicator LED status.
3. The keyboard cannot connect to the computer by Bluetooth. Clear the Bluetooth connection on the keyboard and reconnect using the BT_CLEAR_ALL keycode。
4. Poor Bluetooth signal on keyboard. Check if the Bluetooth antenna on your desktop computer is installed correctly. It can also be tested by connecting other mobile phones and computers. Sometimes firmware flashing again can solve signal connectivity.
5. The right-handed keyboard cannot connect to the main keyboard (left-handed
keyboard). Check if the power switch is turned on and observe the logo on the screen debug. The keyboard has been paired before shipment. If the left and right keyboards need to be re-paired. They need to be connected to the computer separately using USB cables, then double-click the reset switch, finally flash the setting_reset firmware to clear the pairing information between the left and right hands of the keyboard. Afterwards, flash the normal firmware for the left and right keyboards separately.After swiping back into the keyboard, they will automatically connect to each other. Which one flashing first is not important.The pairing status check of the keyboard can be determined through key presses, specific logos on the screen, and backlight linkage control.
6. If some button cannot be triggered. Check if the pins of the switch are inserted into the hot swappable switch socket. Because one foot of the button switch is relatively soft and prone to tilting, it makes the pin difficult to accurately insert the switch socket .
7. The keyboard bluetooth is connected, but the keys on the computer do not respond. Perhaps the bluetooth configuration has been changed. As mentioned earlier, the computer is connected to configuration 1. If you accidentally touch other configurations, the computer will not receive the keycode.
8. After connecting to the computer using USB-C, it cannot be triggered (Bluetooth not
connected). Perhaps the data cable lacks data communication capability and can only be charged.
9. The right-handed keyboard cannot be independently connected to a computer for use. The
USB-C port on the right-hand keyboard is only used for firmware updates and charging. The main control terminal of the keyboard is the left-handed keyboard, and any key code needs to be sent to the left-handed keyboard first, and then sent to the computer. So the right-handed keyboard does not have the function of being used independently.
10. The battery life of the left keyboard is not as good as that of the right. Normal phenomenon, because the left-hand keyboard is the main keyboard, it consumes more electricity and has a shorter battery life.
11. The keyboard power switch is damaged. It can be used with USB-C power supply plugged in, but there is no response during charging. The green light will flash forever. The power switch model for the new version of the corne is MINIMKS12C01, while the old versions of the Corne and sofle use the regular size MKS12C02. Users who cannot buy switches can also directly short-circuit the switch pads. Suggest sending it back to the merchant for after-sales processing. The reset press switch model for the new version of Core and Sofle is TS24CA.
12. The signal of the left and right hands is poor and often disconnected. Firstly, try flashing the firmware again to resolve the issue. If the problem persists, you can flash the reset firmware . In addition, if there is severe interference, you can change the WiFi to the 5G frequency band and check if it is WiFi signal interference. The normal communication distance between left and right keyboards is about 0.6-0.8 meters.
13. There is a noticeable delay in pressing the button. The reason is poor signal, and the troubleshooting method is consistent with the previous method for handling poor bluetooth signal.
14. Adding certain keycodes to the keymap file will result in an error or no response when compiled. The newly added keycodes must be defined in the header file. The solution is to seek help from the ZMK discussion group.Using the graphical tool keymap_editor to modify keymaps can avoid most editing errors. Another document will have a separate introduction.
15. Use ZMK tool yourself and choose NICE!Nano and CORNE to make new firmware.But they
cannot be used after flashing. As mentioned earlier, the hardware definition of a keyboard may be the same or different. We need to use the GitHub repository provided by the seller for compilation,.
16. The battery life is particularly short. The power-ext function can be manually turned off by pressing the EP_OFF keycode. If the backlight is turned on and off again, power-ext will also be turned off.
