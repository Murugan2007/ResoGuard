​#ResoGuard: Structural Resonance Monitor
​Making Cities Safer, One Vibration at a Time
​Hi there! I'm Murugan, and this is ResoGuard.
​I built this project because of the heart-breaking news reports about under-construction metro bridges failing. My goal was to create a device that "listens" to a bridge's natural heartbeat (resonance) and screams an alarm before something goes wrong.

​#What does it do?
​ResoGuard sits on a structure (like a bridge or even my study table!) and monitors how it shakes.
​Finds the "Sweet Spot": Every building has a frequency where it shakes the most (Resonance). My device finds and saves this.
​Smart Monitoring: If the structure starts hitting that dangerous resonance again, ResoGuard triggers a loud alarm and flashes a warning.
​Smart City Ready: After construction is done, the device can switch modes to control street lights via WiFi using an ESP32.

​#The Gear I Used
​Brain: STM32F411E-DISCO (Cortex-M4)
​Eyes: On-board 9-axis MEMS sensor (LSM303DLHC)
​Screen: 0.96" SSD1306 OLED (To show live Hz)
​Alarm: Passive Buzzer + my Arctic Fox Jet Speaker for high-volume alerts
​IoT Wingman: ESP32 (for future WiFi connectivity)

#How it Works (The Science)
​I used Digital Signal Processing (DSP). The STM32 takes raw vibrations and runs a Fast Fourier Transform (FFT).
​Think of it like this: The FFT takes a messy "noise" and turns it into a clear "note" (Frequency). If that note matches the bridge's natural tone, we have a problem!
​Currently, the project is in Simulation Mode for the FOSSEE Hackathon demo, showing how the UI and Alarm logic react when a specific resonance threshold (14.5 Hz) is met.
​#Project Structure
​/Src - All the STM32 C code (the magic happens here).
​/Inc - Header files for the OLED and Math libraries.
​/Hardware - My circuit plans (Si2300 MOSFET and Flyback diode setup).

​#Open Source Love
​This project was built using 100% open-source tools:
​STM32CubeIDE for coding.
​CMSIS-DSP for the heavy math.
​KiCad for thinking about the PCB.
​Special thanks to the FOSSEE team at IIT Bombay for the inspiration!

​#Contact
​If you want to talk about robotics, ECE, or 3D printing, feel free to reach out!
​Developer: Murugan u
Mail:murugan.250167@ece.ritchennai.edu.in
​College: Rajalakshmi Institute of Technology (RIT), Chennai


psst I'm a total beginner and haven't completed any projects 100% yet but as i learn them thorough, one day I'll definetly complete them :)
​💡 Final Tip for Murugan:
​You are officially ready! You have the Code, the Abstract, the Methodology, and now the README.
​Would you like me to double-check the final steps for uploading this to your Google Drive to make sure nothing gets missed?
