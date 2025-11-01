# STM32 Bluetooth Development Board (stm32withBluetooth)

**A compact STM32WB55-based development board with USB-C, RTC & SWD — designed for BLE-enabled embedded projects.**


---

## At-a-glance
- **Main MCU:** STM32WB55CEU6 (Cortex-M4 + BLE radio). (Ref: `U3`)
- **Power:** MIC5365-3.3YD5 linear regulators (3.3V). (Refs: `U`, `U2`)
- **Clocking:** 32 MHz crystal + 32.768 kHz RTC crystal
- **Connectivity:** USB Type-C connector (power / USB2.0)
- **Debugging:** SWD Tag-Connect footprint (TC2030)
- **Peripherals:** LEDs, push-button, mounting holes, fiducials
- **Special footprints:** Bluetooth-specific footprint / antenna footprint present (`bluetoothLib` folder)
- **Outputs included:** KiCad schematic (`*.kicad_sch`), PCB (`*.kicad_pcb`), footprints (`*.pretty`), 3D STEP models, and Gerbers under `Manufacturing/`.



---

## Contents of this repository
```
/stm32withBlueto0th.kicad_sch        # KiCad schematic
/stm32withBlueto0th.kicad_pcb        # KiCad PCB file
/*.pretty/                           # custom footprints (including bluetooth footprint)
3D/                                  # STEP models (STM32WB, crystals, etc.)
Manufacturing/                       # Gerber files for PCB manufacturing
/stm32withBlueto0th-backups/         # backup ZIPs and prior versions
README.md (this file)
```

---

## Key components (auto-detected / inferred)
> Below are the main parts identified in the schematic. Reference names follow the schematic where available.

- **U3** — `STM32WB55CEU6` (main MCU with integrated BLE radio)  
- **U**, **U2** — `MIC5365-3.3YD5` (3.3V LDO regulators)  
- **J1** — `USB_C_Receptacle_USB2.0_16P` (USB-C connector footprint)  
- **J2** — `Conn_ARM_SWD_TagConnect_TC2030` (SWD / Tag-Connect debug header)  
- **X1** — `32 MHz crystal` (main oscillator for MCU)  
- **Y? / FLT** — `32.768 kHz` (RTC crystal footprint present; 3D models show `NX2012SA-32.768K`)  
- **DLF162500LT-5028A1** — footprint present in `bluetoothLib` (likely antenna or related footprint)  
- Passive components: decoupling caps (100nF etc.), bulk caps (4.7uF), resistors (5k1, 220R, ...), inductors (L - power/filter), LEDs, push-button (SW)  
- Mechanical: mounting holes, fiducials, board outline and edge cuts (Gerbers included)


---

## Board functions & intended use
This board appears designed as a small STM32WB55 development / application board for BLE-enabled projects that require:
- USB (power / potential USB data),
- programming via SWD Tag-Connect or USB DFU (if firmware supports it),
- RTC support (32.768 kHz crystal),
- on-board antenna footprint / matching network for BLE,
- stable 3.3V supply from onboard LDO(s).

Use-cases: BLE peripherals, sensor nodes, gateway prototypes (with additional modules), low-power development.

---

## How to build and flash (recommended workflow)

### 1. Open the project
- Use **KiCad (2024/2025)** — the schematic/pcb files indicate KiCad 2024/2025 file formats.  
- Open `stm32withBlueto0th.kicad_sch` in Eeschema and `stm32withBlueto0th.kicad_pcb` in Pcbnew.

### 2. Generate BOM & Gerbers
- BOM (CSV): Eeschema → Generate BOM (or run a script to parse `*.kicad_sch`).  
- Gerbers already provided under `Manufacturing/` if you want to order boards.

### 3. Programming options
- **SWD Tag-Connect**: Use an ST-LINK (or any SWD programmer) with a Tag-Connect cable connected to the TC2030 footprint (`J2`). If no TC cable, use small wires to the labelled SWD pads.  
- **USB DFU (optional)**: If firmware implements USB DFU (and USB lines are connected to MCU's USB pins & VBUS detection), you can flash via DFU over USB-C. Verify schematic that MCU USB D+/D- are connected correctly and that VBUS is present on the USB connector.  
- **Toolchain:** Use **STM32CubeIDE** or GCC arm toolchain + STM32CubeWB middleware (for wireless stacks). STM32CubeWB package contains BLE stack examples for STM32WB series.

### 4. Flashing steps (example using ST-LINK + STM32CubeIDE)
1. Connect ST-LINK to the Tag-Connect footprint (or SWD wires).  
2. Open or create the firmware project in STM32CubeIDE (select MCU: STM32WB55CEUx).  
3. Build and Run → Debug to program flash.

---

## Notes, observations & suggestions (from analysis)
1. **No CAN:** If your aim is automotive / CAN interfacing, add an external CAN transceiver (e.g. SN65HVD230) and a CAN connector (DB9/OBD or screw terminal) plus 120Ω termination. STM32WB has CAN only on some MCU families — double-check pin availability or switch to an STM32 with bxCAN if heavy CAN work is planned (e.g., STM32F1/F4/F7 families or some F4/F7 parts with CAN).  
2. **Antenna / RF:** Project contains Bluetooth footprint and 3D part models. Verify antenna keepout, matching network, and ground plane per STM32WB datasheet and antenna vendor recommendations before mass-production. RF layout rules matter!  
3. **Power:** Two MIC5365 LDOs appear — ensure proper decoupling and thermal consideration. If you need low-power sleep modes for BLE, consider using low-Iq regulator or switch to buck converter for higher efficiency depending on current draw.  
4. **USB:** If you plan USB device functionality, ensure ESD and VBUS protection components are present (TVS, VBUS resistors). I saw VBUS nets in schematic; double-check for differential pair routing and correct USB pull-up/pull-down resistors if needed.  
5. **Test points & silkscreen:** Add clear test points for SWD, VCC, GND, and important signals for easier debugging/manufacturing testing.

---

## Next steps I can do for you (choose one or more)
- [ ] Generate a **detailed BOM (CSV)** automatically from the schematic (refs, values, footprints).  
- [ ] Extract a **component list with footprints** and produce a printable BOM + place file.  
- [ ] Produce a **hardware overview section** with annotated PCB screenshots (I can create PNGs of the PCB from the KiCad files if you want).  
- [ ] Add a **“How to flash”** step-by-step with exact pinouts for Tag-Connect and example STM32CubeIDE project skeleton (I can also provide a sample `main.c`).  
- [ ] Convert README to **dual-language (English + Persian)**.

---

## License
Add your preferred license file to the repo. If you want, I can add a recommended license (e.g., MIT) and include it in the README.

---

## Author
Your name (please replace): **_Your Name Here_**  
Auto-generated analysis performed by script on included KiCad project files (`stm32withBlueto0th.zip`).

---

### Quick disclaimer
This README and the component identification were autogenerated by parsing the KiCad schematic and PCB files in this zip. Some values/footprints (e.g., custom footprints, exact antenna tuning components) might require manual verification. If you want, I can produce a fully verified BOM by cross-checking every symbol in the schematic and matching footprints—tell me and I’ll generate it next.

