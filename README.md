# python-mtdev

Python bindings for mtdev.

Project mtdev is available at https://launchpad.net/mtdev

## Installation

`# python setup.py install`

## Test with a multitouch device

`# python mtdev-test.py /dev/input/eventX`

### mtdev-test.py
```python
#!/usr/bin/env python

import mtdev
import sys

slot = 0

# open the device
dev = mtdev.Device(sys.argv[1])

while True:
	# if no activity, sleep :)
	if dev.idle(1000):
		continue
	# read all available data
	while True:
		data = dev.get()
		if data is None:
			break
		# change the slot number
		if data.type == mtdev.MTDEV_TYPE_EV_ABS and data.code == mtdev.MTDEV_CODE_SLOT:
			slot = data.value
		print(dict(slot=slot, code=hex(data.code), type=data.type, value=data.value))
```

## Useful sources
* [Multi-touch (MT) Protocol][mtdevprot]
* [mtdev functions][mtdevfunc]
* [Understanding evdev][whotevdev]
* [mtview][mtview]

[mtdevprot]: https://www.kernel.org/doc/Documentation/input/multi-touch-protocol.txt
[mtdevfunc]: https://bazaar.launchpad.net/~mtdev-team/mtdev/trunk/view/head:/include/mtdev.h
[whotevdev]: https://who-t.blogspot.it/2016/09/understanding-evdev.html
[mtview]: https://github.com/whot/mtview
