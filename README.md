pyhackrf-ctypes
==============
pyhackrf-ctypes is a Python wrapper for libhackrf.<br>
I create this project because  I want a simple Python interface to my hackrf board.
At First I use pyusb to directly get the data from HackRF,  then I found pyusb can only move data at a  rate of  5MiB/S, too slow for HackRF.So I choose to use ctypes .

# Dependencies

* libusb,NumPy
* Python 2.7.x/3.3+
* [libhackrf](https://github.com/mossmann/hackrf/tree/master/host)

        sudo apt-get install python-dev,libusb-1.0-0 



## Examples

```python
import pylibhackrf ,ctypes

hackrf = pylibhackrf.HackRf()

if hackrf.is_open == False:
    hackrf.setup()
    hackrf.set_freq(100 * 1000 * 1000)
    hackrf.set_sample_rate(8 * 1000 * 1000)
    hackrf.set_amp_enable(False)
    hackrf.set_lna_gain(16)
    hackrf.set_vga_gain(20)    
    hackrf.set_baseband_filter_bandwidth(1 * 1000 * 1000)  

def callback_fun(hackrf_transfer):
    array_type = (ctypes.c_ubyte*length)
    values = ctypes.cast(hackrf_transfer.contents.buffer, ctypes.POINTER(array_type)).contents
    #iq data here
    iq = hackrf.packed_bytes_to_iq(values)    
    return 0

hackrf.start_rx_mode(callback_fun)
```

# Todo
There are a few remaining functions in libhackrf  haven't been wrapped.

# License
Do What The Fuck You Want To Public License.
