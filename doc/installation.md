# SNESTang/NESTang Installation Instructions

This is a step-by-step guide to install SNESTang 0.6, NESTang 0.9 and later versions for the following FPGA boards.

* **Sipeed Tang Primer 25K dock**. This setup also requires Sipeed Tang SDRAM module, Sipeed Tang DVI pmod, TF-Card pmod and DS2x2 pmod.
* **Sipeed Tang Nano 20K**, along with game controllers and adapter boards sold by Sipeed or you can build a controller breadboard.
* **Sipeed Tang Mega 138K Pro dock** (only for SNESTang), along with Tang SDRAM module and DS2x2 pmod.

You also need a Windows or Linux computer, a Dualshock 2 or SNES controller and a MicroSD card.

## 1. Assembly

For Tang Primer 25K, plug in the modules as follows. You choose either the DS2 pmod or the SNES controller pmod. Make sure the controller pmod goes into the middle slot.

<img src="images/primer25k_setup.jpg" width=400> <img src="images/primer25k_snes_controller.jpg" width=400>

If you want to connect the SNES controllers manually, the following is the necessary wirings (5 wires per controller).

<img src="images/snes_controller_primer25k.png" width=400>

NESTang works with both SNES and NES controllers as they are electronically compatible. Refer to the following pinout diagram to connect the NES socket.

<img src="images/NesSnesPinout.png">

For Tang Nano 20K, both the DS2 controllers and SNES controllers can be connected at the same time. Connect the DS2 adapters as follows,

<img src="images/nano20k_setup.jpg" width=400>

You can glue some [SNES controller connectors](https://vi.aliexpress.com/item/32828261927.html) onto a breadboard like so:

<img src="images/nano20k-breadboard-SNES-port.jpg" width=400>

Then the SNES controllers can be connected as follows,

<img src="images/snes_controller_nano20k.jpg" width=400>

For Tang Mega 138K Pro, the controller pmod goes into the left-most slot,

<img src="images/mega138k_pro_setup.jpg" width=400>

## 2. Download SNESTang and install Gowin IDE (Windows) or openFPGALoader (Linux)

Now download a [SNESTang](https://github.com/nand2mario/snestang/releases) or [NESTang](https://github.com/nand2mario/nestang/releases) release from github. For example, [SNESTang 0.6](https://github.com/nand2mario/snestang/releases/download/v0.6/snestang-0.6.zip). Extract the zip file and you will see the FPGA bitstreams `snestang_*.fs`, `nestang_*.fs` and SNESTang menu firmware `firmware.bin`.

### Installing openFPGALoader under Linux

To install openFPGALoader under Debian or Ubuntu Linux run:

```
sudo apt install openfpgaloader
```

### Install Gowin IDE (Windows)

In order to transfer these files to the board, you need the Gowin IDE from the FPGA manufacturer. It is available for free after registration on their website: https://www.gowinsemi.com/. You can also use the [direct link](http://cdn.gowinsemi.com.cn/Gowin_V1.9.9Beta-4_Education_win.zip) if you do not bother to register. 

Once the Gowin IDE installer finishes downloading, extract, run and install everything, including the "programmer" and device drivers.

<img src="images/install_programmer.png" width=400> <img src="images/install_drivers.png" width=400>

## 3. Install the bitstream (snestang_*.fs) using Gowin

Now plug the FPGA board to your computer using the USB cable that comes with it. Then launch the "Gowin Programmer" application.

<img src="images/programmer_init.png" width=400>

Just press SAVE. Then choose the right Series and Device for your board. Tang Primer 25K is GW5A and GW5A-25A. Tang Nano 20K is GW2AR and GW2AR-18. Tang Mega 138K Pro is GW5AST and GWS5AST-138B.

<img src="images/programmer_series.png" width=400>

Now double click "Bypass" in Operation. In the window that pops up, choose "External Flash Mode" for Access Mode. For Operation, choose "exFlash Erase, Program 5AT" (for Tang Primer 25K and Tang Mega 138K Pro) or "exFlash Erase, Program thru GAO-bridge" (for Tang Nano 20K). Then for File Name, choose the `snestang_*.fs`/`nestang_*.fs` file for your board. Then press "Save" to dispose of the window.

<img src="images/programmer_flash.png" width=300>

Now press the button with a play icon on the toolbar to actually start the transfer.

<img src="images/programmer_doit.png" width=400>

It will take about 30 seconds. Now the FPGA bitstream is ready.

### Install the bitstream using openFPGALoader (Linux)

Linux users need only change into the directory where you extracted the .fs bitstream files and run:

```
openFPGALoader -f snestang_*.fs
```

Replacing the * with the correct model to match your board.

## 4. Install the firmware (firmware.bin)

The final step is to transfer the firmware to the board. The firmware is also stored in the on-board SPI flash chip, albeit at a different address than the bitstream. So we also use the Gowin programmer for that. Double click "exFlash Erase, Program 5AT" (for Tang Primer 25K and Mega 138K Pro), or "exFlash Erase, Program thru GAO-bridge" (for Tang Nano 20K) in the Operation field. In the popup window, press the "..." beside the filename to choose the SNESTang "firmware.bin". In the pop-up, you probably need to change the file type filter to `*.*` to be able to choose the `.bin` file. Then set Start Address to `0x500000`, which is the address for the firmware. Then press "Save" to close the window.

<img src="images/programmer_firmware.png" width=300>

Now press the button with a play icon on the toolbar again to start the transfer.

<img src="images/programmer_doit.png" width=400>

### Installing the firmware with openFPGALoader (Linux)

Whilst still in the same directory where you extracted the (S)NESTang release files, run:

```
openFPGALoader --external-flash -o 0x500000 firmware.bin
```

**That is all!** (S)NESTang installation is now finished.

## Finished

Plug in the controller, HDMI cable and a SD card loaded with ROM files. FAT16, FAT32 or exFAT file systems are supported. Connect the USB cable. You should see this main screen on your monitor/tv, from where you can load your games.

<img src="images/mainscreen.jpg" width=400>

When a game is running, press SELECT-RB (right button) to call out the menu again and load other games.

Happy retro gaming!

