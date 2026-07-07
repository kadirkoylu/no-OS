IIO Demo no-OS Example Project
==============================

.. no-os-doxygen::

.. contents::
    :depth: 3

Overview
--------

The IIO Demo project runs an IIO (Industrial I/O) server on the target board,
exposing demo ADC and DAC devices to any IIO client. The demo devices use
loopback buffers, so data written to the DAC can be read back from the ADC,
making this project a self-contained reference for the no-OS IIO framework that
requires no external hardware.

If you are not familiar with the ADI IIO Application, please take a look at:
`IIO No-OS <https://wiki.analog.com/resources/tools-software/no-os-software/iio>`_

If you are not familiar with the ADI IIO-Oscilloscope Client, please take a look
at:
`IIO Oscilloscope <https://wiki.analog.com/resources/tools-software/linux-software/iio_oscilloscope>`_

No-OS Build Setup
-----------------

Please see: https://wiki.analog.com/resources/no-os/build

No-OS Supported Examples
------------------------

Each example is selected with its own ``--variant`` (the ``.conf`` files in the
project directory):

* ``iio`` - IIOD server exposing the demo ADC and DAC devices over UART.
* ``iio_timer_trigger`` - IIOD server using a hardware timer trigger for
  buffered data acquisition.
* ``iio_usb_uart`` - IIOD server exposed over a USB CDC-ACM serial interface
  instead of a physical UART.

Interacting with the device
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once the board is running, an IIO client such as ``iio_writedev`` / ``iio_readdev``
can be used to write to and read from the demo devices.

Write to device:

.. code-block:: bash

   cat sample.dat | iio_writedev -u serial:/dev/ttyUSB0,921600 -b 400 demo_device

Read from device:

.. code-block:: bash

   iio_readdev -u serial:/dev/ttyUSB0,921600 -b 400 -s 6400 demo_device > sample.dat

On Windows, use the ``COM`` port name for the serial URI:

.. code-block:: bash

   Get-Content ascii.dat | iio_writedev -u serial:COM9,921600 -b 100 -s 100 demo_device_output
   iio_readdev -u serial:COM9,921600 -b 100 -s 100 demo_device_input voltage0 voltage1

No-OS Supported Platforms
-------------------------

Maxim Platform
~~~~~~~~~~~~~~

**Build Command**

Available variants: ``iio``, ``iio_usb_uart``.
Available boards: ``max32650fthr``, ``max32655fthr``, ``max32665fthr``,
``max78000fthr`` (``iio``); ``ad-apard32690-sl``, ``max32650fthr``,
``max32665fthr`` (``iio_usb_uart``).
Replace ``--variant`` / ``--board`` accordingly.

.. code-block:: bash

   export MAXIM_LIBRARIES=</path/to/MaximSDK/Libraries>

   cd no-OS

   # build the project (iio example on the max32650fthr board)
   python tools/scripts/no_os_build.py build \
      --project iio_demo --variant iio --board max32650fthr

   # build and flash (requires a connected debug probe)
   python tools/scripts/no_os_build.py build \
      --project iio_demo --variant iio --board max32650fthr \
      --probe openocd --flash

STM32 Platform
~~~~~~~~~~~~~~

**Build Command**

Available variants: ``iio``, ``iio_timer_trigger``, ``iio_usb_uart``.
Available boards: ``sdp-ck1z`` (all three variants); ``nucleo-f413zh``
(``iio_usb_uart``).
Replace ``--variant`` / ``--board`` accordingly.

.. code-block:: bash

   export STM32CUBEMX=</path/to/stm32cubemx>
   export STM32CUBEIDE=</path/to/stm32cubeide>

   cd no-OS

   # build the project (iio example on the sdp-ck1z board)
   python tools/scripts/no_os_build.py build \
      --project iio_demo --variant iio --board sdp-ck1z

   # build and flash (requires a connected debug probe)
   python tools/scripts/no_os_build.py build \
      --project iio_demo --variant iio --board sdp-ck1z \
      --probe openocd --flash

Pico Platform
~~~~~~~~~~~~~

The Pico platform is built through the CMake build flow (see the build guide:
https://wiki.analog.com/resources/no-os/build).

**Build Command**

Available variants: ``iio``.
Available boards: ``rpi-pico``.

.. code-block:: bash

   cd no-OS

   # build the project (iio example on the Raspberry Pi Pico)
   python tools/scripts/no_os_build.py build \
      --project iio_demo --variant iio --board rpi-pico

   # build and flash (requires a connected debug probe)
   python tools/scripts/no_os_build.py build \
      --project iio_demo --variant iio --board rpi-pico \
      --probe openocd --flash
