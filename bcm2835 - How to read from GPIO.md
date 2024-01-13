This is a quick investigation on how a simple library meant to access GPIO pins on Raspberry PI access the GPIO lines directly without having to go through a kernel driver.

# bcm2835

http://www.airspayce.com/mikem/bcm2835/bcm2835-1.73.tar.gz

```
$ tar -zxvf bcm2835-1.73.tar.gz
$ cd bcm2835-1.73
$ ./configure
$ make
$ sudo make install
```
## Library initialization

```
// Initialise this library.
int bcm2835_init(void)
{
    if (debug) 
    {
	...
	bcm2835_gpio = (uint32_t*)BCM2835_GPIO_BASE;
	bcm2835_pwm  = (uint32_t*)BCM2835_GPIO_PWM;
    ....
	return 1; // Success
    }
    int memfd = -1;
    int ok = 0;
    // Open the master /dev/memory device
    if ((memfd = open("/dev/mem", O_RDWR | O_SYNC) ) < 0) 
    {
	fprintf(stderr, "bcm2835_init: Unable to open /dev/mem: %s\n",
		strerror(errno)) ;
	goto exit;
    }
	
    // GPIO:
    bcm2835_gpio = (volatile uint32_t *)mapmem("gpio", BCM2835_BLOCK_SIZE, memfd, BCM2835_GPIO_BASE);
    if (bcm2835_gpio == MAP_FAILED) goto exit;

```

# About _/dev/mem_

https://man7.org/linux/man-pages/man4/mem.4.html

       Byte addresses in _/dev/mem_ are interpreted as physical memory
       addresses.  References to nonexistent locations cause errors to
       be returned.

## Accessing _/dev/mem_

```
...
openat(AT_FDCWD, "/dev/mem", O_RDWR|O_SYNC) = 3
mmap2(NULL, 33554432, PROT_READ|PROT_WRITE, MAP_SHARED, 3, 0x20000000) = 0xb4d7a000
close(3)                                = 0
fstat64(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0), ...}) = 0
write(1, "read from pin 20: 0\n", 20read from pin 20: 0
```

_mmap2_ maps the content of */dev/mem*, starting at offset *0x20000000*. The *mmap2* call returns that the content of the file will be available starting address *0xb4d7a000*.
## Reading a GPIO value

By adding the call to *bcm2835_set_debug(1)* in our program, we can see what addresses is read in the application memory space:

```
neumann@pi:~/sherbrooke-bcm2835 $ sudo ./read_gpio  20
bcm2835_peri_read  paddr 0xb4fcb034
```

Because the content of */dev/mem* is mapped at *0xb4d7a000*, a simple subtraction gives us the offset in */dev/mem* that is read by the program

    0xb4f90034 - 0xb4d90000 = 0x200034

Because the offset mapped in */dev/mem* is *0x20000000*, we can thus conclude that the address read in the device memory is:

    0x200034 + 0x20000000 = 0x20200034

If we look in the *BCM2835 ARM Peripheral Manual*, we can see the following:

![[Pasted image 20240111145328.png]]

And, in the section about GPIO devices, we can see the following:

![[Pasted image 20240111145310.png]]

## More information

https://raspberrypi.stackexchange.com/questions/99785/what-is-the-correct-way-to-use-dev-gpiomem-with-mmap-to-get-access-to-raspberry