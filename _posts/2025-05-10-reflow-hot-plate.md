---

title : "Mini Reflow Hot Plate"

published : false

---

## Goal

Design a mini reflow soldering station (more precisely, a mini reflow hot plate) with USB-C PD input, capable of running standard reflow profiles (preheat → soak → reflow → cool), power around 60–100 W, and peak temperature 230–250 °C.

## Key Specs

- **Input:** USB-C PD, starting at **20 V / 3 A (60 W)**; if EPR is supported, can reach 28–36 V / 5 A (140–180 W), but more complex.  
- **Heater Plate:** Small aluminum plate or thick copper block (e.g., 50×50×6 mm), surface blackened/anodized to improve heat absorption and temperature uniformity.  
- **Sensing:** K-type thermocouple + amplifier/ADC (or high-temperature NTC/RTD), embedded or attached near the component area.  
- **Control:** MCU runs reflow profile state machine + PID, with safety interlocks (over-temperature, open-circuit sensor, watchdog).  
- **Power Stage:** Low-voltage, high-current MOSFET PWM driving the resistive heater; add current/temperature protection if necessary.  
- **UI:** Knob + small screen (e.g., 0.96” OLED) or buttons + LED indicators; optional USB-C/UART for firmware upgrades and data logging.  