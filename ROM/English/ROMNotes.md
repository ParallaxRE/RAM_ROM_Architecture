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
* **Schematic Detail:** The 8-bit data bus arrives at the output buffers. However, for the data to be reflected on the pins (`D0` - `D7`), the **CS (Chip Select)** signal must be activated. Otherwise, the pins remain in a "High-Z" (High Imped
