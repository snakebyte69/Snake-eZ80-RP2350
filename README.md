# Snake-eZ80-RP2350
New Retro Design utilising eZ80, 65C816 and RP2350 (Pico2)

Finalising design at this stage
Constructive criticism welcome

RP2350 Specifications

# Key Features of PIO for Parallel Bus: #
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
