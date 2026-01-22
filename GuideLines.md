## I²C Communication with MPU6050

Establishing communication between the MPU6050 and the controller is done using the **I²C protocol**.  
The following configuration is used in this project.

### Hardware Configuration
- **Microcontroller:** STM32 Nucleo F411RE  
- **I²C Pins:**
  - **PB6** → SCL  
  - **PB7** → SDA  
- **MPU6050 Supply Voltage:** 3.3 V  
  - Initially supplied from the Nucleo board  
  - Will be replaced with dedicated power circuitry in the final design  
- **Pull-up Resistors:**  
  - SDA: 1 kΩ  
  - SCL: 1 kΩ  

### I²C Addressing
- **MPU6050 7-bit Address:** `0x68`
- For I²C communication, the address is **left-shifted by 1 bit** to accommodate the Read/Write bit:


---

## MPU6050 Essential Registers

### SMPLRT_DIV (Sample Rate Divider)
- **Register Address:** `0x19`
- **Description:** Controls the output data rate of the sensor

**Sample Rate Equation:**
$
\text{Sample Rate} = \frac{\text{Gyro Output Rate}}{1 + \text{SMPLRT\_DIV}}
$


- Gyro Output Rate =  
  - 8 kHz (DLPF disabled)  
  - 1 kHz (DLPF enabled)

---

### CONFIG (Configuration Register)
- **Register Address:** `0x1A`

**Bit Fields:**
- **[7:6]** EXT_SYNC_SET  
- **[5:3]** DLPF_CFG  
- **[2:0]** Reserved  

**Digital Low Pass Filter (DLPF) Configuration:**

| DLPF_CFG | Bandwidth | Sample Rate |
|----------|-----------|-------------|
| 0        | 260 Hz    | 8 kHz       |
| 1        | 184 Hz    | 1 kHz       |
| 2        | 94 Hz     | 1 kHz       |
| 3        | 44 Hz     | 1 kHz       |
| 4        | 21 Hz     | 1 kHz       |
| 5        | 10 Hz     | 1 kHz       |
| 6        | 5 Hz      | 1 kHz       |
​
