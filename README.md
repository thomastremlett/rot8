# rot8

Just a small fork to add a rotation hook, useful for my x1 yoga gen6


## automatic display rotation using built-in accelerometer

Automatic rotate modern Linux desktop screen and input devices. Handy for
convertible touchscreen notebooks like HP Spectre x360, Lenovo IdeaPad Flex or Linux phone like Pinephone.

Compatible with [sway](http://swaywm.org/) and [X11](https://www.x.org/wiki/Releases/7.7/).

### installation

#### packages

Available in:

Arch User Repository: [rot8-git](https://aur.archlinux.org/packages/rot8-git/)

Void Package: [rot8](https://github.com/void-linux/void-packages/tree/master/srcpkgs/rot8)

#### manually build from source

Rust language and the cargo package manager are required to build the binary.

```
$ git clone https://github.com/efernau/rot8
$ cd rot8 && cargo build --release
$ cp target/release/rot8  /usr/bin/rot8
```

or

```
$ cargo install rot8

```

### usage

For Sway map your input to the output device:

```

$ swaymsg input <INPUTDEVICE> map_to_output <OUTPUTDEVICE>

```

Call rot8 from sway configuration file ~/.config/sway/config:

```

exec rot8

```

For X11 set Touchscreen Device

```

rot8 --touchscreen <TOUCHSCREEN>

```

This will start the daemon running, continuously checking for rotations.

There are the following args (defaults):

```

--sleep                 // Set millis to sleep between rotation checks (500)
--display               // Set Display Device (eDP-1)
--touchscreen           // Set Touchscreen Device X11, allows multiple devices (ELAN0732:00 04F3:22E1)
--keyboard              // Set keyboard to deactivate upon rotation, for Sway only
--threshold             // Set a rotation threshold between 0 and 1, higher is more sensitive (0.5)
--normalization-factor  // Set factor for sensor value normalization (1e6)
--invert-x              // Invert readings from the HW x axis
--invert-y              // Invert readings from the HW y axis
--invert-z              // Invert readings from the HW z axis
--oneshot               // Updates the screen rotation just once instead of continuously
--version               // Returns the rot8 version

```

You may need to play with the normalization factor (try multiples of 10) and the axis inversions to get the accelerometer readings to calculate right.

## Hooks

calls a hookscript with the orientation of the display
Still need to make this more universal

/home/tremo/.config/rot8

```
#!/bin/sh

case "$1" in
	normal)
		notify-send "normal"
		;;
	inverted)
		notify-send "inverted"
		;;
	left)
		notify-send "left"
		;;
	right)
		notify-send "right"
		;;
	*) echo "$1" ;;
esac

```
