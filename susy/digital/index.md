---
Title: Digital Hardware
Date: 2025-05-20
---

# Microcontroller

The interface between analog hardware and a computer is built around a [STM32F070RB](https://st.com/en/microcontrollers-microprocessors/stm32f070rb.html) microcontroller. This design was chosen for a few reasons:

- High pin count: 44 I/O pins are used in total
- 5V tolerance on 11 I/O pins for parallel port interface
- Integrated USB interface
- Sufficient flash memory to hold 20KB of lookup tables
- Power efficiency at the target frequency of 12MHz

Power is provided through the USB port. The nominal 5V input is regulated down to 3.3V. Separate regulators isolate digital and analog circuits.

Two physical interfaces are supported: 25-pin parallel port, for use with classic Amigas, and USB serial for emulated Amigas. The same [data protocol](#data_protocol) is used with each.

Analog hardware is controlled through a mix of digital inputs, to switch transistors on/off, and DACs to drive voltage-controlled circuits. DACs are driven through a I2C interface.

8-bit DACs are used for amplitude control. 12-bit DACs are used for oscillator frequency control. This gives sufficient linear precision in pitch selection across the 6 octave range. No exponential converters are used.

# Data Protocol

The data protocol carries synthesizer parameter updates at a rate of 50 times per second. This programs every control input of all four audio channels. By varying parameters over time the computer can apply envelopes and low-frequency modulation effects.

Data on the parallel port is provided at approximately 50Hz. The microcontroller synchronizes to the exact update rate. The USB serial interface, on the other hand, accepts batches of updates and backpressures the client at a rate dictated by its internal 50Hz clock. This is more suitable for modern computers which have scheduling jitter.

Parameter updates are internally interpolated to a rate of 200Hz for smoother changes. The protocol allows each parameter to be linearly interpolated or stepped without interpolation. For example: an amplitude envelope would use linear interpolation, while changes in musical notes might step immediately or interpolate for pitch bend effects.

A data frame consists of 4 groups of 8 bytes. Each group controls a different audio channel. The format is as follows (bit numbers in brackets):

- Byte 0: [0] Reserved, [1] Oscillator Synchronization, [2] Reserved, [3] Ring Modulation, [5:4] Oscillator Select, [7:6] Filter Select
- Byte 1: [7:0] Filter Resonance
- Byte 2: [3:0] Oscillator Frequency (high bits), [4] Interpolate Frequency, [5] Interpolate Pulse Width, [6] Interpolate Filter Cutoff, [7] Interpolate Amplitude
- Byte 3: [7:0] Oscillator Frequency (low bits)
- Byte 4: [7:0] Amplitude Stereo Left
- Byte 5: [7:0] Amplitude Stereo Right
- Byte 6: [7:0] Filter Cutoff
- Byte 7: [7:0] Pulse Width

Oscillator frequency is specified in 32ths of a semitone. Other units are 8-bit fractions from 0 to 100%.

# Frequency Calibration

The voltage-controlled oscillator does not have a perfect linear response to its control voltage. The precise mapping of input voltage to frequency also varies slightly across each audio channel.

To ensure accurate frequency control the microcontroller maintains four lookup tables which map fractional semitone inputs to a DAC voltage. These are created by the microcontroller in a one-time calibration process, in which cycles of the pulse wave are counted while varying the input voltage.