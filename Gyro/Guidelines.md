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
## MPU6050 Configuration Registers

### SMPLRT_DIV (Sample Rate Divider)
- **Register Address:** `0x19`
- **Description:** Controls the output data rate of the sensor

**Sample Rate Equation:**
**Sample Rate = Gyro Output Rate / (1 + SMPLRT_DIV)**



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



---

### GYRO_CONFIG
- **Address:** `0x1B`  
- **Bit Fields:**
  - `[7]` XG_ST  
  - `[6]` YG_ST  
  - `[5]` ZG_ST  
  - `[4:3]` FS_SEL  
  - `[2:0]` Reserved  

**Full-scale Range Selection (FS_SEL):**

| FS_SEL | Range      | Sensitivity (LSB/°/s) |
|--------|-----------|-----------------------|
| 0      | ±250 °/s  | 131                   |
| 1      | ±500 °/s  | 65.5                  |
| 2      | ±1000 °/s | 32.8                  |
| 3      | ±2000 °/s | 16.4                  |

---

### ACCEL_CONFIG
- **Address:** `0x1C`  
- **Bit Fields:**
  - `[7]` XA_ST  
  - `[6]` YA_ST  
  - `[5]` ZA_ST  
  - `[4:3]` AFS_SEL  
  - `[2:0]` Reserved  

**Full-scale Accelerometer Range (AFS_SEL):**

| AFS_SEL | Range   | Sensitivity (LSB/g) |
|---------|---------|--------------------|
| 0       | ±2 g    | 16384              |
| 1       | ±4 g    | 8192               |
| 2       | ±8 g    | 4096               |
| 3       | ±16 g   | 2048               |

---

### ACCEL_XOUT_H and Sensor Data Registers
- **Starting Address:** `0x3B`  
- **Register Map:**

| Data          | Address|
|---------------|--------|
| ACCEL_XOUT_H  |0x3B    |
| ACCEL_XOUT_L  |0x3C    |
| ACCEL_YOUT_H  |0x3D    |
| ACCEL_YOUT_L  |0x3E    |
| ACCEL_ZOUT_H  |0x3F    |
| ACCEL_ZOUT_L  |0x40    |
| TEMP_OUT_H    |0x41    |
| TEMP_OUT_L    |0x42    |
| GYRO_XOUT_H   |0x43    |
| GYRO_XOUT_L   |0x44    |
| GYRO_YOUT_H   |0x45    | 
| GYRO_YOUT_L   |0x46    |
| GYRO_ZOUT_H   |0x47    |
| GYRO_ZOUT_L   |0x48    |

---

### WHO_AM_I
- **Address:** `0x75`  
- **Description:** Returns `0x68` if I²C communication is successful
---
## REG Mapping​

| Register Name   | Address (Hex) | Use / Function                                                                 |
|-----------------|---------------|-------------------------------------------------------------------------------|
| SMPLRT_DIV      | 0x19          | Sample Rate Divider. Sets the rate at which the sensor outputs data.          |
| CONFIG          | 0x1A          | DLPF (Digital Low Pass Filter) and bandwidth settings for gyro and accel.     |
| GYRO_CONFIG     | 0x1B          | Gyroscope full-scale range and self-test configuration.                        |
| ACCEL_CONFIG    | 0x1C          | Accelerometer full-scale range and self-test configuration.                    |
| ACCEL_XOUT_H    | 0x3B          | High byte of X-axis accelerometer data (paired with ACCEL_XOUT_L for full data). |
| WHO_AM_I        | 0x75          | Device ID register. Returns 0x68 for MPU6050 to confirm communication.        |
