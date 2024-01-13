# Links

The GPIO interface is the simplest interface usable to access GPIOs.

https://www.kernel.org/doc/Documentation/gpio/sysfs.txt

You can use the *sysfs* interface from the Bash command line.

# Demo

### Bash demo

## Simple C demo

## Polling GPIO sysfs interface

Don't forget setting the *edge* file in the *sysfs* to properly detect the state change.

```
"edge" ... reads as either "none", "rising", "falling", or
		"both". Write these strings to select the signal edge(s)
		that will make poll(2) on the "value" file return.

		This file exists only if the pin can be configured as an
		interrupt generating input pin.
```