<div align="center">

# 💾 ROM – Read-Only Memory

**Design Purpose:** Designed to retain non-volatile data; the data is not modified frequently.
**Data Entry:** Data can be written during the manufacturing process.
**Microcomputer Programs:** Stores microcomputer programs (firmware) since it is non-volatile.
**Applications:** Found pre-programmed in devices like calculators, home appliances, and security systems.

</div>

---

<img width="653" height="660" alt="resim" src="https://github.com/user-attachments/assets/ae6ba87d-2370-4008-a096-1a61276cc80a" />

> [!NOTE]
> ## 🏗️ ROM Architecture
> A basic ROM architecture consists of **4 main components** at the hardware level. In a sample architecture, a **16 x 8 capacity** structure can be considered, capable of holding 8-bit data in each of its 16 different addresses.

### 🟢 1. Row Decoder
* **Function:** Reads a portion of the address information from the processor and activates the target row.
* **Schematic Detail:** Utilizes a **1-of-4 decoder** logic that takes `A0` and `A1` inputs. Based on the incoming 2-bit address, this decoder selects only one of the 4 rows (Row 0, 1, 2, 3).

### 🟢 2. Column Decoder
* **Function:** Reads the remaining part of the address information to select the correct column, allowing only the data in that specific column to pass to the data bus.
* **Schematic Detail:** Uses `A2` and `A3` inputs to target one of the 4 columns (Column 0, 1, 2, 3), again utilizing a **1-of-4 decoder** logic.

### 🟢 3. Register Array
* **Function:** The cell matrix where the actual data is stored. A "Register" is located at every intersection of the row and column lines.
* **Schematic Detail:** A total of **16 Registers** are formed by the intersection of 4 rows and 4 columns. Each register stores **8-bit** data (a word). To read data, that specific register must receive an "active" (Enable - E) signal from both its row and its column.

### 🟢 4. Output Buffers
* **Function:** Output gates that ensure the data read from the register is safely transferred out of the chip (to the processor or data bus).
* **Schematic Detail:** The 8-bit data bus arrives at the output buffers. However, for the data to be reflected on the pins (`D0` - `D7`), the **CS (Chip Select)** signal must be activated. Otherwise, the pins remain in a "High-Z" (High Impedance) state and do not occupy the data bus.

---

> [!NOTE]
> ## 💿 Types of ROM
> * **Mask-Programmed ROM:** Data is "masked" and printed onto the chip during the manufacturing stage. It can never be modified by the user. Used in mass-produced chips.
> * **PROM (Programmable ROM):** Manufactured blank. It can be programmed once using a special device. If a mistake is made, there is no going back; the chip becomes useless.
> * **EPROM (Erasable Programmable ROM):** The data inside can be erased using ultraviolet (UV) light. Once erased, it can be reprogrammed.
> * **EEPROM (Electrically Erasable PROM):** Data can be erased using electrical signals instead of light. The data inside can be updated without removing the chip from the circuit.
> * **Flash Memory:** The most widely used type today (USB drives, SSDs). It is an advanced version of EEPROM that is much faster and can be erased in blocks.

---

## 🔍 Detailed Features of ROM Types

> [!TIP]
> ### Mask-Programmed ROM Features
> * **Written by the Manufacturer:** Programs are written directly into the chip by the manufacturing company according to specific requirements.
> * **Use of Photographic Negative (Mask):** A "mask" is used to control electrical connections. This physically engraves the data into the chip's circuitry.
> * **Mass Production is Required for Economy:** It becomes economically viable only when manufactured in very high volumes to offset the high masking costs.
> * **Non-Reprogrammable:** Once data is processed into the physical structure, it cannot be modified or erased.
> * **Application Example:** Used as a character generator in older CRT monitors.

<img width="1223" height="894" alt="resim" src="https://github.com/user-attachments/assets/37af7cfa-ad02-4de0-a1c7-f7029a7f1ccd" />

---
> [!WARNING]
> ### PROM (Programmable ROM) Features
> * **Fusible-Link MROM:** Contains thousands of tiny conductive bridges called "fuses."
> * **User-Programmable:** The manufacturer sells this chip blank. The user writes their data into it using a special device (PROM programmer).
> * **OTP (One Time Programmable):** Once a fuse is blown, there is no turning back.
> * **Non-Erasable:** Once programmed, the data inside can never be erased or overwritten.

<img width="509" height="465" alt="resim" src="https://github.com/user-attachments/assets/817d0c5c-96e3-44fb-882e-8481d09d665a" />

---
> [!WARNING]
> ### EPROM (Erasable Programmable ROM) Features
> * **Special Voltage:** Requires a special, higher-than-normal operating voltage (Usually between 10-25V) to program.
> * **Timing:** A specific amount of time is required to write data into the cells.
> * **Erasability:** The data inside can be erased using **Ultraviolet (UV) light**. (It features a transparent quartz window on top for this purpose).
> * **Examples:** 2732 (4K x 8) and 2764 (8K x 8) versions.

<img width="1177" height="594" alt="resim" src="https://github.com/user-attachments/assets/2bf23c08-bab0-44f4-ade6-59a307d5431b" />

---
> [!TIP]
> ### EEPROM (Electrically Erasable Programmable ROM)
> * **Electrical Erasure:** Data can be erased using electrical signals. The required high voltage is generated internally by the chip's own circuitry.
> * **Fast Writing:** Writing time is approximately 5 microseconds (5µs) per cell.
> * **Auto-Erase:** The circuitry automatically erases the target cell during the write process (it handles the overwriting operation).
> * **Historical Examples:** Intel 2816 and 2864.

<img width="600" height="321" alt="resim" src="https://github.com/user-attachments/assets/f73c2236-7e26-4e89-af76-42f06b5b183c" />

---
> [!TIP]
> ### Flash Memory Features
> * **Higher Density:** Can store much more data (Gigabytes) in a smaller physical area compared to EEPROM.
> * **Faster:** Has a much faster erase/write cycle than EEPROM.
> * **Two Different Erase Modes:** Bulk Erase (clears the entire chip) and Sector Erase (targets a specific block).
> * **Write Time:** Completes the writing process in a very short time, typically around 10 microseconds (10µs) per cell.

<img width="1118" height="663" alt="resim" src="https://github.com/user-attachments/assets/c9a74e4e-97fb-4182-8553-1fc39832ccbd" />

---

> [!NOTE]
> ## ⚙️ ROM Applications
> * **Firmware:** Stores the initial data and program codes that run the moment the device is powered up (e.g., BIOS).
> * **Data Table (Look-Up Table):** Used for quick "look-ups" of constantly needed fixed data (e.g., Trigonometric tables).
> * **Data Converter:** Used to convert one data type into another (e.g., converting BCD code to 7-segment display code).

---

## ⚡ Function Generator using ROM and DAC

<img width="1045" height="329" alt="resim" src="https://github.com/user-attachments/assets/111ad65e-7144-4d9b-a09b-27fe04dfd8d5" />

> [!TIP]
> ### 1. Addressing Unit (8-bit Counter)
> * **Function:** It acts as the system's "timer". With every clock pulse (CLK), it increments the count by 1 (00→01→02⋯→FF).
> * **Engineering Detail:** The speed of the counter determines the frequency of the output wave. Running the counter faster generates a higher-frequency signal.

> [!TIP]
> ### 2. Storage Unit (256 x 8 ROM)
> * **Function:** Acts as a "Look-Up Table" (LUT) containing the mathematical samples of a waveform (Sine, Triangle, etc.).
> * **Why ROM?:** Forcing the CPU to perform complex calculations in real-time is power-intensive and slow. Reading pre-calculated data from a ROM can be executed millions of times per second.
> * **Capacity:** 256×8 means the amplitude of the wave at 256 different points is recorded with 8-bit precision.

> [!TIP]
> ### 3. Conversion Unit (8-bit DAC and Filter)
> * **DAC (Digital-to-Analog Converter):** Converts the digital numbers coming from the ROM into a proportional analog voltage level.
> * **RC Filter:** The DAC output looks like a staircase (has sharp edges). This filter "smooths" those corners, providing a clean, continuous analog wave (Sine).
