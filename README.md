# Snake-eZ80-RP2350
New Retro Design utilising eZ80, 65C816 and RP2350 (Pico2)

## Finalising design at this stage ##
## Constructive criticism welcome ##

Summary of Design so far

Core Components:
	•	- CPU: Zilog eZ80F91AZ050EK microcontroller
	•	- Coprocessors: Four RP2350 microcontrollers - 3 for Video, 1 for Audio; 65C816 option
	•	Memory:	16Mb (2MB) SRAM for system memory, 8Mb VRAM
	•	RP2350’s internal 8Mb (1MB) PSRAM used as VRAM or shared 8Mb SRAM on board
	•	Storage: SD card with write-protect switch and card-detect pin
	•	Clock Speed: Configurable with 5 jumpers for 50 MHz, 25 MHz, 12.5 MHz, 6.25 MHz, 3.12 MHz
	•	Bus: eZ80 24-bit/16-bit multiplexed connected to the bus of theRP2350s
	•	Bank switching alternates between 16Mb system RAM and RP2350’s internal 8Mb PSRAM (VRAM)
	•	Bus transceivers for optimal performance and minimal parasitics
	•	Power Supply: ATX Standard - External 9V DC (preferred) or 12V DC adapter with a high-quality switch
	•	Diagnostic & Debugging: UART/SWD diagnostic ports, USB-UART converter
	•	Clock Control: The video RP2350 controls the WAIT pin on the eZ80 and the audio RP2350. The audio RP2350 can also control the WAIT pin on the eZ80.

Graphics & Video (Video RP2350):
	•	HDMI Output: Driven by an RP2350 using PicoDVI software
	•	VRAM Handling:
	•	The last RP2350 (Video) only reads from the 8Mb PSRAM (VRAM) - No write access to VRAM
	•	Video Synchronization: RP2350 (Video) controls the WAIT pin for bus arbitration

Audio System (Audio RP2350 - PicoGUS Integration):
	•	Audio Processing: Second RP2350 running PicoGUS
	•	Audio Outputs:
	  •	HDMI Audio Output
	  •	3.5mm Line-Level Audio Jack (with an optional headphone amplifier IC like TPA6132A2)
	  •	2x RCA Line-Level Outputs
	•	Audio Inputs:
	  •	2x RCA Line-Level Inputs
	  •	Line-Level 3.5mm Input
	•	MIDI Support:
	  •	MIDI IN
	  •	MIDI OUT/MIDI THRU
	  •	Additional MIDI port for a third MIDI interface
	  •	MIDI over USB
	•	Bus Access:
	  •	The audio RP2350 reads from and writes to the 16Mb system RAM
	  •	The audio RP2350 controls the WAIT pin on the eZ80 for bus synchronization

Connectivity & I/O:
	•	GPIO Configuration:
	•	eZ80 has GPIO for its 24-bit bus
	•	RP2350s have separate, isolated GPIOs
	•	External device support without conflicts
	•	Expansion Headers:
	•	144-pin header exposing all eZ80 microcontroller pins
	•	Additional expansion header for I2C/SPI peripherals
	•	Communication Interfaces:
	•	Ethernet
	•	USB-UART for debugging
	•	Jumper/Switch Configuration:
	•	Bootloader/flash support for RP2350
	•	DIP switches for bank switching, MIDI routing, audio output selection
	•	Jumper to disconnect DAC audio from the 3.5mm jack

Power & Protection Features:
	•	Voltage Rails:
	•	3.3V and 5V monitoring
	•	Short-Circuit Protection: To limit damage
	•	EMI Reduction & Shielding: Designed to minimize noise
	•	Thermal Management: Copper pours and optional heatsinks for high-power components

Debugging & Status Indicators:
	•	Status LEDs for:
	•	Power
	•	Bus Activity
	•	MIDI Signals
	•	Test Points & Headers: For unused GPIOs on the eZ80 and RP2350

Additional Features:
	•	RTC (Real-Time Clock): Uses DS3231
	•	Backup Battery: CR2032 for RTC or volatile SRAM
	•	Silkscreen Labeling: All connectors, buttons, and jumpers clearly labeled

Design Priorities:

✅ Bus Stability: eZ80 and RP2350s must share the 24-bit bus without crashing or conflicting
✅ Audio-Video Synchronization: Video RP2350 controls WAIT signals to ensure bus coordination
✅ Noise Minimization: Audio and video circuits are placed far from noise sources
✅ High-Quality Components: Best bus transceivers, high-quality power switch, and effective shielding
✅ Future Expansion: GPIO headers, diagnostic ports, and configuration jumpers enable flexibility

This is your full system summary based on everything you’ve shared. Let me know if you need modifications or additional details! 🚀

# RP2350 Specifications #

## Key Features of PIO for Parallel Bus: ##
    1.    Clock Speed: The PIO can run up to the system clock frequency (150 MHz for RP2350), depending on your configuration.
    2.    Parallel Operation: The PIO can output data to multiple pins simultaneously, making it ideal for driving a 16-bit parallel bus.
    3.    Instruction Execution: Each PIO instruction takes 1 clock cycle.

## Data Transfer Speed Calculation ## 
    •    Clock Frequency: Assume 150 MHz for maximum system performance.
    •    PIO Instruction Time: 1 instruction per clock cycle (6.67 ns at 150 MHz).
    •    Output Pins: 16 pins can be updated in parallel in one instruction.

If you configure the PIO to output data continuously:
    •    Maximum Throughput: 150 million 16-bit words per second = 2.4 Gbps (same as before but achieved more consistently due to the PIO’s deterministic timing).
    •    Time per Transfer: 6.67 ns per 16-bit word.

## Practical Considerations ## 
    1.    FIFO Buffering: The RP2350’s PIO has a FIFO that allows you to load data for the PIO to output. This helps maintain high throughput without stalling the CPU.
    2.    DMA Integration: For sustained data rates, you can use DMA (Direct Memory Access) to load the PIO’s FIFO without CPU intervention. This ensures that the PIO can operate at full speed without being bottlenecked by software.
    3.    Strobe or Clock Signal: For synchronization with external devices, you may need to generate an additional strobe or clock signal, which can also be handled by the PIO.

## Practical Speed Estimate ## 

With proper configuration (DMA feeding the PIO and minimal stalling):
    •    Theoretical Max Frequency: 150 MHz.
    •    Realistic Frequency: 140–150 MHz, accounting for minor overhead.

** These specifications are being collected from the Data Sheet, Online Resources and Industry Experts.
They will be refined over time so ensure accuracy, some speeds are hard to pin down and need to be confirmed.
