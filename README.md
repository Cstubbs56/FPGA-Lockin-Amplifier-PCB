# FPGA-Lockin-Amplifier-PCB
## Purpose
This PCB acts as both DAQ and a lock in amplifier for the Waterloo Rocketry 2026 fiber optic gyroscope payload. To accomplish this, it 
- Biases the fibre optic loop by outputting a sinusoid to a piezoelectric crystal via a DAC with fibre optic wrapped around the PZT.
- Reads the output of the interferometer through a photodiode into a transimpedance amplifier and ADC.
- Calculates I and Q components then exports to an SD card going through the microcontroller.

## Power Electronics
The PCB needs 24V, 5V, 3.3V, 2.5V, and 1.2V. The 24V is used for biasing the piezoelectric crystal, and goes through a 0-20V output 16 bit DAC. 5V and 3.3V are used for the analog and digital circuitry, while the 2.5V and 1.2V are solely for powering the FPGA.

## Analog Electronics
As mentioned, there is an ADC reading a transimpedance amplifier. The transimpedance amplifier was sized based on an estimated maxium output power from the interferometer of 20mW into a photodiode with an approximate response of 0.55A/W. The other side is the aforementioned DAC, which is setup to output 0-20V with a sinusoid from the FPGA.

## Logging
The FPGA being used does nt have enough extra space in the fabric to interact with a filesystem and therefore the FPGA sends results to the microcontroller to be held in RAM before outputting to the SD card. There is a piece of flash memory connected to the FPGA, but this is used for configuration.

## FPGA
Since the FPGA's non-volatile configuration memory can only be written to once, the FPGA is used with volatile configuration. This means it must configure itself each time it powers on. It uses SPI for configuration, which is hooked up to both a flash ROM and a SPI header that can be used for programming the memory from a computer.  

## Laser
The laser on the PCB is a 200mW optical power infrared laser. It draws typically around 300mA in operation.
