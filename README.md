<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>MCC CubeSat Summer Camp</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.2/babel.min.js"></script>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: #080c14; }
  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: #080c14; }
  ::-webkit-scrollbar-thumb { background: #1a3a6b; border-radius: 3px; }
</style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const { useState } = React;

const phases = [
  { id: 1, name: "RECRUITMENT", weeks: "Week 1", sessions: "1–3", color: "#00D4FF", icon: "🚀" },
  { id: 2, name: "TRAINING", weeks: "Weeks 2–5", sessions: "4–15", color: "#FFB800", icon: "🧠" },
  { id: 3, name: "BUILD", weeks: "Weeks 6–8", sessions: "16–24", color: "#FF6B35", icon: "🔧" },
  { id: 4, name: "OPERATIONS", weeks: "Week 9", sessions: "25–27", color: "#7CFC00", icon: "📡" },
  { id: 5, name: "DEMO DAY", weeks: "Week 10", sessions: "28–30", color: "#FF4DFF", icon: "🏆" },
];

const allSessions = [
  { phase:1, session:1, title:"You've Been Selected", subtitle:"Agency Formation Day",
    envelope:"URGENT: A satellite has gone silent. Your team has been selected to build the replacement. Report immediately.",
    sprints:[
      {label:"Mission Ignition",desc:"Sealed envelopes on each desk. Students open the NASA-style memo before the instructor says a word. Show N2YO live satellite tracking — point to a moving dot: 'That 10 cm box is what you're building.' Take the class outside."},
      {label:"Agency Formation",desc:"Teams of 5 form their space agency. Vote on a name, sketch a logo. Role cards distributed and laminated: Mission Commander, Payload Engineer, Communications Engineer, Power Engineer, Structures Lead. Each card has a real-world job description."},
      {label:"Logbook Issue",desc:"Every student receives their graph-paper mission logbook. First entry: agency name, mission date, mission objective in their own words, sketch of their role badge. Instructor shows their own engineering logbook — real culture moment."},
    ],
    components:["Laminated role cards","Graph-paper mission logbooks","Sealed mission briefing envelopes"],
    logbook:"Draw your team logo. Write: 'My mission is to...' and 'My role is responsible for...'",
    wow:".",
  },
  { phase:1, session:2, title:"What Is Space, Really?", subtitle:"Orbits, CubeSats & the Mission",
    envelope:"INTEL: The satellite that went silent was in LEO at 500 km. Understand its environment before you can replace it.",
    sprints:[
      {label:"Orbital Mechanics — Analogy First",desc:"Orbit = falling sideways fast enough to keep missing Earth. Demo: spin a ball on a string, let go — it flies straight. Now explain why satellites don't fall. Students draw orbital diagrams in logbook with their own labels."},
      {label:"Live Satellite Tracking",desc:"Open N2YO together. Find the ISS, track it in real time. 'Your satellite will make that sound in 9 weeks.' Students find one satellite from their country and log it."},
      {label:"CubeSat Hall of Fame",desc:"Story-based tour of 3 real missions: ASTERIA (exoplanet hunting), GOMX-3 (aircraft tracking from orbit), and a university CubeSat that failed — and what they learned. Students vote on which mission they'd work on."},
      {label:"Wall of Celebrated Failures",desc:"Introduce the corkboard. Instructor pins the first card — their own real failure. Rule: failures are shared, not hidden. Each student writes their biggest fear about the project on a card, anonymously pinned. Board stays up all 10 weeks."},
    ],
    components:["Corkboard + pushpins","Index cards","Projector with internet"],
    logbook:"Draw the satellite's orbit path. Label: altitude, orbital velocity, ground pass duration.",
    wow:"'",
  },
  { phase:1, session:3, title:"Meet Your Kit", subtitle:"Unboxing, Multimeters & First Circuits",
    envelope:"LOGISTICS: Your hardware kit has arrived. Identify every component and prove you can use your tools safely.",
    sprints:[
      {label:"Kit Unboxing Challenge",desc:"Each team receives their full hardware kit in a sealed box. Challenge: without the manual, identify every component and guess what it does. Students sketch and label each component in logbook: ESP32, BME280, LoRa, OLED, NeoPixel, GPS, MPU-6050, SD card, solar panel, battery, structure frame."},
      {label:"Multimeter Basics",desc:"Measure voltage on a fresh AA battery, USB power bank, and LiPo battery. Measure continuity across a wire. Break continuity — hear the beep stop. 'This tool tells you if electricity can flow. You will use it every session.' Teams race to measure 5 components."},
      {label:"First Circuit — LED on Breadboard",desc:"Wire a simple LED + resistor circuit on a breadboard. No code yet — just power, resistor, LED, ground. Calculate resistor value together (V = IR). Every student wires their own. When LED lights: 'You just completed your first satellite subsystem test.'"},
      {label:"Safety Briefing + Component Catalog",desc:"Safety rules: polarity, voltage levels, never modify while powered. Students create a Component Catalog in their logbook — sketch + name + function for 6 components. Structures Lead stores the kit properly."},
    ],
    components:["Full hardware kit per team","Multimeters","Breadboards","LEDs","Resistor kit","AA batteries"],
    logbook:"Component catalog: sketch and label ESP32, BME280, LoRa, OLED, NeoPixel, GPS. Calculate the correct resistor for your LED.",
    wow:"When the LED lights up for the first time — every student's own circuit, wired by hand.",
  },
  { phase:2, session:4, title:"The Brain Wakes Up", subtitle:"ESP32 + Arduino IDE",
    envelope:"ENGINEERING ALERT: The OBC is offline. Upload firmware and confirm the brain is alive.",
    sprints:[
      {label:"What Is a Microcontroller?",desc:"Analogy: MCU = the brain of the satellite. It reads sensors (senses), runs code (thinks), controls outputs (acts). Show ESP32 board — identify CPU, GPIO pins, USB port, reset button. Students label a board diagram in logbook."},
      {label:"Arduino IDE Setup",desc:"Install ESP32 board support, select correct COM port and board. Every student confirms board is recognized. Show skeleton: setup() runs once, loop() runs forever. Analogy: setup() = launch checklist, loop() = orbit cycle."},
      {label:"First Upload — Blink",desc:"Upload blink code. LED on pin 2 blinks every second. First team to shout 'ALIVE!' gets a mission point. Challenge: change blink speed to morse code for the first letter of your agency name."},
      {label:"Wrong-First Debug Session",desc:"Teams receive intentionally broken blink code — three bugs hidden (wrong pin, missing semicolon, misspelled function). No hints allowed. Find and fix all three. Every bug fixed gets pinned on the Wall of Celebrated Failures."},
    ],
    components:["ESP32 development boards","USB cables","Laptops with Arduino IDE"],
    logbook:"Draw the ESP32 and label 8 GPIO pins. Write: setup() does ___ and loop() does ___. Paste the 3 bugs and their fixes.",
    wow:"First LED blink — the satellite's brain is alive.",
  },
  { phase:2, session:5, title:"Teaching the Brain to Talk", subtitle:"Serial Monitor, Variables & Data Types",
    envelope:"COMMS CHECK: Ground station reports no telemetry from OBC. The brain needs to learn to broadcast status.",
    sprints:[
      {label:"Serial Communication",desc:"Analogy: Serial Monitor = walkie-talkie between ESP32 and laptop. Set baud rate (9600). Print 'Hello from [Agency Name] Space Agency!' Change baud rate to wrong value and see garbage — then fix it."},
      {label:"Variables and Data Types",desc:"int, float, bool, String. Analogy: variables = labeled boxes. Store temperature in a float, mission name in a String. Challenge: print 'Mission [name] — Status: NOMINAL — Uptime: [seconds]' updating every second."},
      {label:"Arithmetic and Formatting",desc:"Convert Celsius to Fahrenheit in code. Format a string with units. Print altitude estimate. Goal: Serial Monitor looks like real telemetry. Students decorate output with headers and separators."},
      {label:"Timed Mission Report",desc:"8 minutes: write a program that prints a fake satellite status report every 5 seconds with 4 values. First team to get all 4 printing correctly gets a mission point."},
    ],
    components:["ESP32 + laptops"],
    logbook:"Copy your final serial output into logbook. Label each line: what value is printed, what unit, why it matters.",
    wow:"When the Serial Monitor starts scrolling fake telemetry — it already feels like a real satellite.",
  },
  { phase:2, session:6, title:"Digital Senses — Inputs", subtitle:"Buttons, NeoPixels & Digital Signals",
    envelope:"PAYLOAD STATUS: The satellite needs to detect events and respond visually. Wire the input/output subsystem.",
    sprints:[
      {label:"Digital vs Analog",desc:"Analogy: digital = light switch (ON/OFF), analog = dimmer switch (0-100%). Show oscilloscope display of a digital square wave vs analog sine wave. Students draw both waveforms in logbook."},
      {label:"Button Input",desc:"Wire a push button to GPIO. Read its state in code. Print 'BUTTON PRESSED' to Serial Monitor when pushed. Challenge: count presses and print the count."},
      {label:"NeoPixel RGB LED 🌈",desc:"Wire a NeoPixel RGB LED (WS2812B). Load FastLED library. Set color with R, G, B values. Challenge: RED when button not pressed, GREEN when pressed. Then: cycle through all colors on each press."},
      {label:"Kinesthetic NeoPixel Demo",desc:"5 students stand in a line — they ARE the NeoPixel strip. Instructor calls out pixel index and color. Students hold up colored cards. Then: pass a 'transmission' token down the strip."},
    ],
    components:["Push buttons","NeoPixel WS2812B LEDs","Resistors","Colored cards for demo"],
    logbook:"Draw both digital and analog waveforms. Write the NeoPixel color code for your agency colors. Sketch the circuit.",
    wow:"Students controlling RGB color with code — the kinesthetic NeoPixel strip coming alive.",
  },
  { phase:2, session:7, title:"The Satellite's Skin", subtitle:"BME280 Sensor — Part 1",
    envelope:"PAYLOAD ALERT: Thermal and pressure readings are critical. Wire the environment sensor. Payload Engineer — this is yours.",
    sprints:[
      {label:"What Does a Satellite Feel?",desc:"In LEO: temperature swings from -40°C to +85°C every 95 minutes. BME280 measures temperature, humidity, pressure. Analogy: BME280 = the satellite's skin — it feels the environment so the OBC knows what's happening."},
      {label:"I2C Communication",desc:"Analogy: I2C = classroom roll call. Teacher (ESP32) calls a name (device address). Only that student (BME280) answers. SDA = the answer line, SCL = the timing bell. Show the I2C address 0x76."},
      {label:"Wire and Read BME280",desc:"Payload Engineer wires the sensor (other roles observe and draw the circuit). Install Adafruit BME280 library. Upload code. Read temperature, humidity, pressure to Serial Monitor. First team: mission point."},
      {label:"First Real Data",desc:"Teams record first sensor reading in logbook (temperature, pressure, humidity with units). Compare readings between teams — why are some slightly different? Calibration, airflow, proximity to laptop heat. Real engineering: data is never perfect."},
    ],
    components:["BME280 sensors","I2C jumper wires","ESP32"],
    logbook:"Draw the I2C wiring diagram. Record first reading: temperature ___°C, pressure ___ hPa, humidity ___%. What does each value tell us about satellite health?",
    wow:"First real sensor data appearing on screen — real numbers from the real world.",
  },
  { phase:2, session:8, title:"Reading the Environment", subtitle:"BME280 Part 2 + OLED Display",
    envelope:"UPGRADE ORDER: Ground station requires visual telemetry display. Install OLED screen on satellite.",
    sprints:[
      {label:"Analyze Yesterday's Data",desc:"Compare sensor readings to phone weather app. How close? Why different? Introduce calibration offset concept. Calculate altitude from pressure using barometric formula. Take sensor outside — pressure drops, altitude increases."},
      {label:"OLED Display Wiring 📺",desc:"Wire SSD1306 0.96\" OLED display via I2C (same bus as BME280). Install Adafruit SSD1306 library. Display 'MCC SPACE CAMP' on screen. Every student sees their first satellite display."},
      {label:"Live Telemetry on OLED",desc:"Display temperature, humidity, pressure, and altitude on OLED updating every second. Format like a real HUD: T:25.3C H:42% P:1013hPa Alt:0m. Challenge: switch between two screens every 3 seconds."},
      {label:"Environmental Challenge",desc:"Take satellite outside. Log OLED readings every 30 seconds for 5 minutes. Return inside, log 2 more minutes. Plot temperature vs time on graph paper. First real data graph of the camp."},
    ],
    components:["SSD1306 0.96\" OLED displays","I2C cables","ESP32 + BME280"],
    logbook:"Draw the OLED wiring. Paste your data table (temperature vs time). Plot the graph.",
    wow:"Live satellite telemetry on a tiny OLED — it looks like actual space hardware.",
  },
  { phase:2, session:9, title:"The Satellite's Heartbeat", subtitle:"Power Systems & Solar Energy",
    envelope:"POWER CRITICAL: EPS readings are anomalous. Without stable power, every other subsystem fails.",
    sprints:[
      {label:"Why Power Is Everything",desc:"Scenario: satellite loses power at 200 km. Everything stops — sensors, radio, computer. EPS = the satellite's heart. Show real CubeSat EPS block diagram. Power Engineer role card moment: 'This is why your role exists.'"},
      {label:"Solar Panel Measurement",desc:"Measure solar panel voltage and current under: direct sunlight, cloudy, shade, different angles (0°, 30°, 60°, 90°). Record in a table. Plot in logbook. Challenge: find the angle that gives maximum power output."},
      {label:"Battery Safety — Wrong-First",desc:"Teams receive a circuit with battery polarity reversed. 'Your satellite is drawing 0 current. Why?' Students diagnose using multimeter. Fix the polarity. Discussion: what protects against this in real satellites? Wire in a protection diode."},
      {label:"Power Budget Card Game 🃏",desc:"Each team receives 6 subsystem power cards (ESP32: 240mA, BME280: 3.6mA, LoRa TX: 120mA, OLED: 25mA, GPS: 50mA, NeoPixel: 60mA) and a 400mA budget card. Solar only generates 300mA. What do you turn off? Teams negotiate and justify."},
    ],
    components:["Solar panels","Multimeters","LiPo batteries","Protection diodes","Power budget game cards"],
    logbook:"Solar panel data table at 5 angles. Power budget calculation: total draw vs available supply.",
    wow:"Power budget card game — real trade-off decisions, just like satellite engineers make.",
  },
  { phase:2, session:10, title:"Storing the Mission", subtitle:"SD Card Data Logging",
    envelope:"DATA INTEGRITY ALERT: All telemetry must be stored locally. Ground contact is only 10 minutes per orbit.",
    sprints:[
      {label:"Why Satellites Log Data",desc:"Analogy: SD card = the satellite's memory — like a flight recorder on a plane. For 85 minutes of every 95-minute orbit, no one is listening. The satellite logs everything and transmits when in range."},
      {label:"SD Card Wiring + SPI",desc:"Wire SD card module via SPI protocol (different from I2C — faster, point-to-point). Install SD library. Create 'MISSION.TXT' and write 'MISSION INITIALIZED'. Read it back. Verify the text appears."},
      {label:"Log BME280 Data",desc:"Write code that logs temperature, humidity, pressure, and timestamp to SD card every 5 seconds. Format: timestamp,temp,humidity,pressure. Let it run 3 minutes. Open the file on a laptop — 36 data points collected autonomously."},
      {label:"Data Analysis",desc:"Open the CSV in a spreadsheet. Plot temperature vs time. Find min and max values. Calculate average. Change something (open a window, move near heat) and repeat. Compare the two datasets. 'This is how satellite scientists find anomalies.'"},
    ],
    components:["SD card modules","Micro SD cards (FAT32)","ESP32 + BME280 + OLED"],
    logbook:"Paste sample CSV data (5 rows). Draw SPI vs I2C comparison. Write: 'Logging data matters because...'",
    wow:"Opening a CSV file that the satellite wrote itself, completely autonomously.",
  },
  { phase:2, session:11, title:"Whispering Across the Field", subtitle:"LoRa Communication — Part 1",
    envelope:"COMMS BLACKOUT: The satellite needs long-range radio. Install LoRa transceiver. Without it, the mission cannot report to ground.",
    sprints:[
      {label:"Radio History + LoRa Introduction",desc:"History: telegraph → AM radio → WiFi → LoRa. Each step: longer range OR higher speed — rarely both. LoRa chose range. Analogy: LoRa = shouting across a football field in a whisper that still reaches. WiFi = normal conversation that only reaches the next room. 868 MHz: wave is 34 cm long."},
      {label:"LoRa Module Wiring",desc:"Wire Ra-02 LoRa module via SPI. Communications Engineer leads. Set frequency, spreading factor, bandwidth. One team programs transmitter, another programs receiver. Both must use identical settings — analogy: same language, same dialect."},
      {label:"First Packet Transmission",desc:"Transmitter sends 'HELLO FROM [AGENCY NAME]' every 2 seconds. Receiver prints received message + RSSI to Serial Monitor. When first packet appears: class celebrates. Timer stops. Log the RSSI value. This is the satellite's first voice."},
      {label:"TELEMETRY Moment 📡",desc:"Now transmit real BME280 data. Ground station receives temperature, pressure, humidity — wirelessly. Every student watches the live values. NeoPixel flashes blue on satellite every time it transmits. Log the first telemetry packet in logbook — sign it, timestamp it."},
    ],
    components:["Ra-02 LoRa modules 868 MHz (×2 per team)","Temporary wire antennas","ESP32 + BME280 + NeoPixel"],
    logbook:"Draw the transmitter and receiver block diagram. Record first packet: content, RSSI, timestamp.",
    wow:"First wireless packet received — data from satellite to ground station through the air.",
  },
  { phase:2, session:12, title:"Making the Radio Smarter", subtitle:"LoRa Part 2 — Reliability & Signal Quality",
    envelope:"SIGNAL DEGRADATION: Packet error rate is 33%. The radio needs tuning. Communications Engineer — diagnose and fix.",
    sprints:[
      {label:"Spreading Factor Deep Dive",desc:"Analogy: SF = speaking speed. SF7 = fast talker (short range, high data rate). SF12 = slow and clear (long range, low data rate). Test: measure PER at SF7 vs SF12 at the same distance. Record both."},
      {label:"Wrong Sync Word — Wrong-First",desc:"Instructor secretly changes sync word on ground station to 0x15 (satellite still uses 0x12). Ground station hears nothing. 'Why? The signal is there — the radio is on — but silence.' Students debug and find the sync word mismatch. Pin the lesson on the Wall of Celebrated Failures."},
      {label:"CRC and AUX Pin",desc:"CRC analogy: tamper-proof receipt — if anyone modifies the package, the checksum breaks. Demonstrate CRC error detection with a bit-flip. AUX pin: traffic light before radio talks. Wrong-first: remove AUX check, watch every third packet corrupt. Add back. Fixed."},
      {label:"Ground Station OLED Upgrade",desc:"Add OLED to the ground station. Display: last received temperature, packet count, RSSI, packet error rate — live. Teams compete: who can run 5 minutes with the lowest PER?"},
    ],
    components:["Ra-02 LoRa modules","OLED for ground station","ESP32"],
    logbook:"SF7 vs SF12 comparison table: PER, range, data rate. Explain CRC in your own words using an analogy.",
    wow:"Ground station OLED showing live mission data — it looks like real mission control.",
  },
  { phase:2, session:13, title:"Knowing Where You Are", subtitle:"GPS Module — Location Payload",
    envelope:"NAVIGATION PAYLOAD: Add GPS tracking to the satellite. The ground station must know where the satellite is at all times.",
    sprints:[
      {label:"How GPS Works",desc:"Analogy: 4 satellites shout their location and time simultaneously. You calculate how long each took to reach you. Where all 4 circles intersect = your position. This is trilateration. Draw diagram in logbook — no formulas, just overlapping circles."},
      {label:"GPS Wiring + First Fix",desc:"Wire Neo-6M GPS module (UART communication). Install TinyGPS++ library. Move to window or outside and wait for GPS fix. When coordinates appear: timer stops. Compare to Google Maps — check accuracy to within 10 meters."},
      {label:"Add GPS to Telemetry",desc:"Combine GPS with BME280. Build a telemetry packet: latitude, longitude, altitude (GPS), temperature, pressure, humidity (BME280). Transmit over LoRa. Ground station receives and displays on OLED."},
      {label:"GPS Tracking Challenge",desc:"Mission Commander takes the transmitting satellite and walks a 100m route around the building. Ground station logs every GPS coordinate received. Plot the route on graph paper using the coordinates. Real satellite ground track exercise, at walking pace."},
    ],
    components:["Neo-6M GPS modules","External GPS antennas","UART jumper wires"],
    logbook:"Record your first GPS coordinates. Plot the GPS tracking route on graph paper.",
    wow:"Plotting a real GPS track of the satellite's walk — the mission has a map.",
  },
  { phase:2, session:14, title:"The Satellite Knows Its Attitude", subtitle:"MPU-6050 Gyroscope & ADCS",
    envelope:"ATTITUDE ANOMALY: The satellite is tumbling. ADCS subsystem must be understood and wired.",
    sprints:[
      {label:"What Is Attitude?",desc:"Not 'mood' — orientation in 3D space. Pitch, roll, yaw explained with a paper airplane. Real problem: a tumbling CubeSat cannot point its antenna at the ground or solar panels at the Sun. Show how reaction wheels fix this — kinesthetic demo: spinning chair + bicycle wheel."},
      {label:"Wire MPU-6050",desc:"Wire MPU-6050 via I2C (now 3 devices on I2C bus: BME280, OLED, MPU-6050). Install MPU6050 library. Read accelerometer and gyroscope values to Serial Monitor. Students tilt the sensor — watch values change in real time."},
      {label:"Live Attitude on OLED",desc:"Display roll, pitch on OLED with visual tilt indicator. Challenge: hold the satellite perfectly level for 10 seconds — steady state test. Then spin it — watch gyroscope values spike."},
      {label:"Detumbling Simulation",desc:"Students use a spinning office chair with a held bicycle pump. Someone spins the chair — student pushes against the floor to stop it (simulate reaction wheel braking). Then repeat with MPU-6050 data: 'when angular velocity exceeds X, the thruster fires.'"},
    ],
    components:["MPU-6050 gyroscope/accelerometer modules","Spinning office chair for demo","OLED display"],
    logbook:"Draw the 6 degrees of freedom: pitch, roll, yaw, X, Y, Z. Record MPU-6050 values at: flat, 45° tilt, vertical, upside down.",
    wow:"Spinning chair + bicycle wheel angular momentum demo — physics becomes physical.",
  },
  { phase:2, session:15, title:"The Full Picture", subtitle:"Systems Architecture + Firmware Design",
    envelope:"MISSION DESIGN REVIEW: All subsystems understood. Time to architect the complete system. One mistake here costs a week.",
    sprints:[
      {label:"Full System Block Diagram",desc:"Students draw the complete satellite architecture on a full logbook page: ESP32 at center, BME280 via I2C, MPU-6050 via I2C, OLED via I2C, GPS via UART, LoRa via SPI, SD card via SPI, battery via power rail, solar panel via power rail, NeoPixel via GPIO."},
      {label:"Peer Teach-Back",desc:"Payload Engineer teaches power system to Power Engineer (and vice versa). Communications Engineer teaches ADCS to Structures Lead. Mission Commander reviews all. 5 minutes per pair direction. No notes allowed. Teaching forces real understanding."},
      {label:"Telemetry Packet Design",desc:"What goes in every transmission? Teams design their packet structure: SAT_ID (1 byte), timestamp (4 bytes), temperature (2 bytes), pressure (4 bytes), humidity (2 bytes), latitude (4 bytes), longitude (4 bytes), altitude (2 bytes), roll (2 bytes), pitch (2 bytes), battery voltage (2 bytes), packet_count (2 bytes). Total = 31 bytes."},
      {label:"Firmware Skeleton Together",desc:"Instructor live-codes the complete firmware skeleton on projector. Students follow along adding comments. State machine: INIT → SENSOR_READ → TRANSMIT → LOG → SLEEP → repeat. Students copy structure into logbook."},
    ],
    components:["Whiteboards or large paper for architecture diagram","All previous components for reference"],
    logbook:"Full system block diagram (full page). Telemetry packet structure with byte counts. State machine diagram.",
    wow:"Seeing the complete architecture they will build — all in one diagram they drew themselves.",
  },
  { phase:3, session:16, title:"Suit Up", subtitle:"Structural Assembly & Thermal Challenge",
    envelope:"BUILD AUTHORIZATION GRANTED: Phase 3 begins. Structure is the foundation. Every other subsystem depends on it.",
    sprints:[
      {label:"Structure Standards",desc:"Why 10 cm × 10 cm × 10 cm? CubeSat standard = any satellite fits any rocket. Analogy: standard shipping container. Hold the 1U frame. Count mounting holes. Discuss: why aluminum 6061-T6? (light, machinable, dissipates heat). Structures Lead takes ownership."},
      {label:"Frame Assembly",desc:"Structures Lead assembles the frame following the assembly checklist in logbook. Other roles draw their subsystem's mounting position — planning where each component goes before it's placed. Mission Commander signs off each step."},
      {label:"Vibration Test — DIY Shake Table",desc:"Mount a temporary component (breadboard) to the frame. Tap the frame with a rubber mallet — watch for vibration. Loose screw? The whole structure resonates. Tighten. Retest. This is the same vibration qualification real CubeSats go through before launch."},
      {label:"Thermal Challenge 🌡️",desc:"Two identical frames. Wrap Frame A in aluminum foil + foam insulation. Leave Frame B bare. Place ice cube inside both. Start timer. Measure internal temperature every 2 minutes with BME280. Which frame keeps the ice longer? Real thermal engineering."},
    ],
    components:["Aluminum 1U CubeSat frame kits","Rubber mallet","Foam insulation + aluminum foil","Ice cubes","BME280 for thermal measurement"],
    logbook:"Assembly checklist. Mounting position plan: draw top-down and side view. Thermal challenge data table.",
    wow:"Thermal challenge — students doing real engineering thermal analysis with ice and foil.",
  },
  { phase:3, session:17, title:"Power On", subtitle:"EPS Assembly — Wiring the Heart",
    envelope:"EPS INTEGRATION: The satellite has a structure. Now give it a heartbeat. All power rails must be verified before any other subsystem is powered.",
    sprints:[
      {label:"Power Rail Planning",desc:"Power Engineer leads. Map the power distribution: solar panel → protection diode → battery → 3.3V regulator → all subsystems. Wire the power rail first — nothing else connects until verified clean."},
      {label:"Measurement Protocol",desc:"Multimeter at every node: solar panel (open circuit voltage), battery (charged voltage), 3.3V rail (must be 3.3V ± 0.1V). Log every measurement. Any value outside tolerance: stop and fix before proceeding."},
      {label:"Current Draw Measurement",desc:"Add each component one at a time. Measure current draw each time. Compare to session 9 power budget cards. Are actual numbers close to estimated? Where are the discrepancies? Power Engineer writes the real power budget in logbook."},
      {label:"NeoPixel Status System 🌈",desc:"Wire NeoPixel status LED to power rail output. Code: GREEN = all power rails nominal, RED = low battery (< 3.5V), BLUE blink = currently transmitting, YELLOW = SD card write in progress. The satellite communicates its own health visually."},
    ],
    components:["LiPo batteries","Solar panels","3.3V regulators","Protection diodes","NeoPixel LED","Multimeters"],
    logbook:"Power rail diagram with measured voltages at each node. Current draw table: estimated vs actual.",
    wow:"NeoPixel turning green when power is verified — the satellite is healthy.",
  },
  { phase:3, session:18, title:"Eyes and Ears", subtitle:"Payload Integration — All Sensors Mounted",
    envelope:"PAYLOAD INTEGRATION: BME280, GPS, MPU-6050, and OLED must all be mounted inside the structure and verified simultaneously.",
    sprints:[
      {label:"Physical Mounting",desc:"Payload Engineer mounts BME280, GPS antenna, MPU-6050, and OLED display to the frame interior. Every other role watches and draws the physical layout. Mission Commander checks clearance and cable routing. Signs off."},
      {label:"I2C Bus Verification",desc:"Run I2C scanner code. All three I2C devices (BME280 at 0x76, OLED at 0x3C, MPU-6050 at 0x68) must appear. Missing address = not properly wired. Diagnose: is it power? SDA/SCL? Address? Fix it before moving on."},
      {label:"All Sensors Reading Simultaneously",desc:"Complete payload firmware: read all 3 sensors, display on OLED cycling through 3 screens, log to SD card, update NeoPixel based on temperature threshold. Everything happening in one loop."},
      {label:"Outdoor Environment Run",desc:"Take the satellite (battery-powered, fully self-contained) outside. Log all sensor data for 10 minutes. Read the SD card. Did altitude change when you walked to a different elevation? Did temperature change in sun vs shade?"},
    ],
    components:["All payload components mounted","SD card","OLED","BME280","GPS","MPU-6050","LiPo battery"],
    logbook:"I2C scan result. Physical mounting sketch (top-down view). 10-minute outdoor data table.",
    wow:"First fully self-contained satellite running outdoors — no laptop, just battery and sensors.",
  },

  { phase:3, session:20, title:"Bring It All Together", subtitle:"Full System Integration",
    envelope:"CRITICAL MILESTONE: All subsystems ready individually. Today they meet for the first time. Integration is where satellites succeed or fail.",
    sprints:[
      {label:"Integration Checklist Briefing",desc:"Mission Commander reads the integration checklist aloud. Each step must be completed in order — no shortcuts. Power off until every connection is verified. Analogy: NASA has 'inhibit' rules — power cannot reach a subsystem until 3 independent checks pass."},
      {label:"Sequential Integration",desc:"Connect in order: power rail → ESP32 → BME280 → OLED → MPU-6050 → GPS → SD card → LoRa → NeoPixel. After each connection: power on, verify the new component works, power off. Mission Commander signs each step."},
      {label:"First Full Power-On",desc:"All systems connected. Power on for the first time. NeoPixel turns GREEN (power good) → starts cycling BLUE (transmitting). Ground station screen lights up with telemetry. All sensor values flowing. SD card logging. OLED updating. The satellite is alive."},
      {label:"Integration Review",desc:"Each role inspects their subsystem in the integrated system. Any loose wires? Cable tie everything. Label cables with colored tape. Mission Commander signs off the complete integration."},
    ],
    components:["All integrated components","Cable ties","Colored tape for cable labeling","Integration checklist printed per team"],
    logbook:"Complete integration checklist (sign each step). Time of first full power-on. Write: 'The hardest part of integration was ___ because ___.'",
    wow:"Full system first power-on — NeoPixel green, OLED updating, ground station receiving — the satellite is alive.",
  },
  { phase:3, session:21, title:"The Brain Gets Smart", subtitle:"Complete Firmware + State Machine",
    envelope:"OBC UPGRADE: The satellite's firmware needs intelligence. Dumb firmware crashes. Smart firmware survives.",
    sprints:[
      {label:"State Machine Architecture",desc:"Real satellites have modes: BOOT → SENSOR_READ → TRANSMIT → LOG → LOW_POWER → ERROR. Draw the state machine diagram in logbook. Code it with a switch-case structure. Each state is a function. Mission Commander names the satellite's modes — agency-themed names."},
      {label:"Error Handling",desc:"What if the BME280 fails? What if the GPS has no fix? What if the SD card is full? Add error handling. If BME280 fails: transmit 999 as value, set NeoPixel to RED, continue transmitting. Never crash. Real satellites must continue operating even with failed subsystems."},
      {label:"Transmission Optimization",desc:"Optimize: read all sensors, build packet, transmit, log — all within a defined duty cycle. Teams choose their transmission interval (10s, 30s, or 60s) and justify the choice based on their power budget from session 17."},
      {label:"SAT_ID and Agency Identity",desc:"Add SAT_ID byte to every packet — each satellite gets a unique 2-digit number. Add agency name string to boot message. Update the ground station firmware: it now displays which satellite sent each packet. Multiple satellites can run simultaneously and be distinguished."},
    ],
    components:["ESP32 + full integrated satellite","Laptops for coding"],
    logbook:"State machine diagram with all transitions. Error handling table: what fails, what satellite does, what NeoPixel shows.",
    wow:"The satellite continues transmitting even after a simulated sensor failure — real resilience.",
  },
  { phase:3, session:22, title:"Five Faults", subtitle:"The Escape Room 🔐",
    envelope:"EMERGENCY: All satellites report anomalies simultaneously. Five critical faults detected — one per subsystem. Diagnose and repair before the mission window closes in 90 minutes.",
    sprints:[
      {label:"Escape Room Briefing",desc:"Instructor pre-sabotages each team's satellite with exactly 5 hidden faults:\n1. Battery polarity reversed (Power)\n2. SDA and SCL swapped (Payload)\n3. Wrong spreading factor (Comms)\n4. Wrong TX interval — 0ms delay, blocks all other code (OBC)\n5. CRC checking wrong byte range (OBC)\nNo hints. Ground station is working correctly."},
      {label:"FAULT HUNT — Round 1 (45 min)",desc:"Teams work through faults using only their tools: multimeter, Serial Monitor, logic, and the Wall of Celebrated Failures. Each role focuses on their subsystem first. Write each diagnosis in logbook before fixing — log the reasoning, not just the fix."},
      {label:"FAULT HUNT — Round 2 (30 min)",desc:"Hints unlocked after 45 minutes — but using a hint costs 2 mission points. First team to get clean telemetry on ground station wins. Every team must eventually complete it — no team left behind."},
      {label:"Failure Debrief",desc:"Every fault found goes on the Wall of Celebrated Failures with a one-line lesson. 'SDA/SCL swapped: always label your wires.' 'Battery polarity: always verify with multimeter before powering on.' Class votes on the most important lesson of all 5."},
    ],
    components:["Full integrated satellites (sabotaged by instructor)","Multimeters","Laptops","Logbooks","Mission point scoreboard"],
    logbook:"For each fault: what you suspected, how you tested, what you found, how you fixed it.",
    wow:"First team to shout 'TELEMETRY CLEAN!' — the entire room reacts.",
  },
  { phase:3, session:23, title:"How Far Can You Reach?", subtitle:"Outdoor Range Test + Link Budget",
    envelope:"RANGE VERIFICATION: Before demo day, the satellite's communication range must be quantified. Walk the satellite out. Log the data. Plot the curve.",
    sprints:[
      {label:"Link Budget Theory",desc:"Analogy: link budget = planning whether your flashlight reaches across a dark field. Transmit power (20 dBm) - cable loss (1 dB) + antenna gain (2 dBi) - path loss + receive antenna gain (2 dBi) - receiver sensitivity (-137 dBm) = link margin. Calculate expected range on paper. Predict RSSI at 50m, 100m, 200m."},
      {label:"Range Walk — Data Collection",desc:"Ground station fixed at classroom window. Satellite team walks: 10m → 25m → 50m → 100m → 150m → maximum range. At each distance: record RSSI from ground station display. Add obstacles: around a corner, through a wall. Record everything."},
      {label:"Plot RSSI vs Distance",desc:"Plot measured RSSI vs distance on graph paper in logbook. Draw the theoretical curve from link budget calculations. Compare: where do they match? Where do they diverge? Obstruction points visible as sudden RSSI drops."},
      {label:"Team Comparison and Analysis",desc:"All teams share their max range and RSSI curves on the whiteboard. Why did some teams reach further? Antenna orientation? Cable quality? Mounting position? 'Team 3 got 40m further — they mounted their antenna vertically and we mounted ours sideways.' Real antenna polarization lesson."},
    ],
    components:["Full satellite systems","Tape measure or measuring wheel","Graph paper in logbooks"],
    logbook:"Link budget calculation table. RSSI vs distance data table (at least 6 distances). Graph: measured vs theoretical curve.",
    wow:"Seeing the satellite talk from the end of the street — more range than students expected.",
  },
  { phase:3, session:24, title:"First Contact", subtitle:"Naming Ceremony + Mission Patch",
    envelope:"MILESTONE: First contact achieved. This satellite is no longer hardware. Give it a name. It is now a spacecraft.",
    sprints:[
      {label:"First Contact Ceremony 🎖️",desc:"In a quiet, deliberate moment: instructor plays Apollo 11 landing audio. The ground station receives the satellite's first-ever outdoor packet — projected on the screen. Every team member signs the logbook with the time and date. 'First contact achieved at [time]. Congratulations.'"},
      {label:"Satellite Naming",desc:"Team votes on the satellite's name. Name goes into the logbook (witnessed and signed by all 5 members). Instructor prints a nameplate on card stock — satellite name + agency name + date of first contact. Taped to the structure. The SAT_ID byte updated to include the satellite's name in the boot message."},
      {label:"Mission Patch Design Final Session 🎨",desc:"Teams finalize their mission patch design: circular, satellite name, agency name, team motto, 868 MHz frequency, launch date, one piece of mission imagery. Sketched on paper. Instructor will digitize between sessions, print on sticker paper, laminate. Every student takes one home."},
      {label:"Demo Day Briefing",desc:"Explain demo day structure: parents attend, satellite transmits live, each team presents for 5 minutes, certificates handed out. Presentation format: agency intro, satellite name and mission, one technical concept explained to parents, live telemetry reading."},
    ],
    components:["Card stock for satellite nameplate","Printer","Sticker paper for mission patches","Laminator"],
    logbook:"Sign and timestamp the First Contact log entry. Write the satellite's name and why you chose it. Sketch the final mission patch design.",
    wow:"Apollo 11 audio + first packet projected on screen + every team member signing the logbook.",
  },
  { phase:4, session:25, title:"Mission Control Is Live", subtitle:"All Satellites Transmitting Simultaneously",
    envelope:"MISSION CONTROL ACTIVATION: All satellites are assigned mission slots. Ground station accepting telemetry from all spacecraft simultaneously.",
    sprints:[
      {label:"Dashboard Activation",desc:"Live mission control dashboard launched on projector. Shows all teams simultaneously: SAT_ID, satellite name, temperature, pressure, humidity, altitude, GPS coordinates, roll, pitch, battery voltage, packet count, packet error rate — all updating live."},
      {label:"Nominal Operations Begin",desc:"All satellites powered on simultaneously. Dashboard updates with real data from every spacecraft. Each team watches their satellite's column. Mission Commander logs the operations start timestamp. NeoPixels blinking blue across the room."},
      {label:"Anomaly Hunting Begins",desc:"Rule of operations: 'If a value looks wrong, log it. Investigate before touching the hardware.' Teams monitor their dashboard column. Any unexpected value gets logged with timestamp. This is how real mission operations teams work."},
      {label:"First Operations Report",desc:"Each Mission Commander gives a 2-minute verbal status update: 'Satellite [name] — all systems nominal / anomaly detected in [subsystem] — investigating.' Class practices the language of mission operations."},
    ],
    components:["Live dashboard software on projector laptop","All satellite systems","Mission logbooks"],
    logbook:"Operations log: timestamp, satellite status, any anomalies observed.",
    wow:"All satellites live on the projector simultaneously — it looks like a real mission control room.",
  },
  { phase:4, session:26, title:"Anomaly Hunters", subtitle:"Fault Diagnosis in Real Operations",
    envelope:"ANOMALY REPORT: Three satellites reporting unexpected values. Ground station RSSI has degraded 15 dB. Investigate and resolve before demo day.",
    sprints:[
      {label:"Anomaly Review",desc:"Review logged anomalies from last session. Which were real problems? Which were expected behavior misread as anomalies? Establish nominal ranges for each sensor — values outside trigger investigation. Write nominal ranges in logbook."},
      {label:"Hidden Fault Challenge",desc:"Instructor secretly introduces one hidden fault into the GROUND STATION firmware (wrong frequency offset — all RSSI values drop 10 dB). Teams notice lower RSSI but satellites are unchanged. Investigation: is it the satellite or the ground station? Teams trace the fault to the source."},
      {label:"PER Competition",desc:"30-minute competition: which satellite maintains the lowest packet error rate over 30 minutes? Track on dashboard. Teams optimize: antenna orientation, transmission interval, spreading factor. Final PER values recorded. Winner gets mission points."},
      {label:"Mission Report Writing",desc:"Each role begins their subsystem report (2 paragraphs): what did you build, what did you learn, what failed, what you'd do differently. Mission Commander writes the executive summary."},
    ],
    components:["All satellite systems","Live dashboard","Logbooks"],
    logbook:"Anomaly log: all anomalies found, root cause, resolution. PER competition result. Subsystem report draft.",
    wow:"Students debugging a ground station fault — the realization that ground station problems look just like satellite problems.",
  },
  { phase:4, session:27, title:"Report to Mission Control", subtitle:"Dress Rehearsal + Portfolio Complete",
    envelope:"MISSION CLOSE: All operations data collected. Prepare your mission report for public presentation. Parents will attend the next session.",
    sprints:[
      {label:"Final Logbook Entry",desc:"Every student writes their final logbook entry: 'What I built, what I learned, what broke, what I'd change on the next mission.' Mission Commander writes the satellite's official final status. Logbooks are now complete engineering portfolios."},
      {label:"Presentation Practice",desc:"Each team runs through their 5-minute demo day presentation: agency name and logo, satellite name and mission, one technical concept explained simply for parents, a live telemetry reading explained in plain language. Instructor times them. Other teams give feedback."},
      {label:"Technical Demo Dry Run",desc:"Full demo day technical sequence: satellite powers on, NeoPixel confirms power, ground station confirms link, OLED shows live data, dashboard updates — all verified one final time. Any issues found and fixed now, not on demo day."},
      {label:"Agency Showcase Prep",desc:"Print or display agency logos. Set up team areas with satellite, ground station, and logbook displayed. Each team decides which logbook page to leave open for parents to see (most impressive graph? first contact log? first sensor data?)."},
    ],
    components:["All systems for full dry run","Printed agency logos","Logbooks completed","Demo-ready checklist"],
    logbook:"Final logbook entry signed. Demo day speaking notes (bullet points only). Open to the most impressive page for parent display.",
    wow:"Running the full demo sequence flawlessly — the confidence that comes from preparation.",
  },
  { phase:5, session:28, title:"Mission Patches & Portfolios", subtitle:"Final Prep — Everything Ready",
    envelope:"T-MINUS 48 HOURS: Mission patches printed. Certificates prepared. Parents invited. Make it worthy of the work you've done.",
    sprints:[
      {label:"Mission Patch Distribution",desc:"Printed, laminated mission patches distributed to all students. Each student gets 2: one for their logbook, one to keep. Students sign each other's patches on the back — like a crew signing mission memorabilia."},
      {label:"Certificate Preparation",desc:"Mission Specialist Certificates shown to students. Signed by MCC Director and PSUT faculty member. Each certificate tied to specific competencies: escape room completion (diagnostic), range test completion (RF engineering), demo day presentation (systems communication). Not attendance — demonstrated skill."},
      {label:"Wall of Celebrated Failures — Final Review",desc:"Class reviews every card on the Wall. 30 sessions of failures and lessons. Vote: which lesson will you remember in 5 years? The winner gets a special frame. The wall is photographed. Every student gets a printed summary of all lessons to take home."},
      {label:"Final Agency Photo",desc:"Each team poses with their satellite, mission patch, and logbook. Agency logo in the background. Certificates in hand. These photos go on MCC's record of the program. Students are the first graduates of the Junior Space Agency Program."},
    ],
    components:["Laminated mission patches","Certificates (MCC + PSUT signed)","Camera for team photos","Printed Wall of Failures summary"],
    logbook:"Paste mission patch. Write: 'The one lesson from this camp I will never forget is...' Record all 5 team members' names in the final page.",
    wow:"Holding a certificate signed by a university faculty member — in Jordan's culture, this moment is significant.",
  },
  { phase:5, session:29, title:"Final System Check", subtitle:"T-Minus One — Everything Verified",
    envelope:"LAUNCH MINUS 24 HOURS: Tomorrow is demo day. Satellites must be flawless. Every system verified.",
    sprints:[
      {label:"Full System Final Verification",desc:"Run complete final check: power on → NeoPixel green → OLED showing all sensors → LoRa transmitting → ground station receiving → SD card logging → GPS fix achieved → dashboard updating. Every item checked, signed off in logbook by Mission Commander."},
      {label:"Presentation Final Rehearsal",desc:"Last run-through of the 5-minute presentation. This time the audience asks parent-style questions: 'What does LoRa mean?' 'How does your satellite know the temperature?' 'What does that blue light mean?' Teams practice answering in plain language without jargon."},
      {label:"Ground Station Setup",desc:"Set up the ground station and live dashboard in demo day configuration. Confirm all team satellites appear on the dashboard simultaneously. Set appropriate transmission intervals so dashboard updates smoothly."},
      {label:"Open Practice — Students Lead",desc:"Last 30 minutes: no instructor direction. Teams do whatever they feel they need. Some debug, some rehearse, some explain to each other, some just run their satellite and watch the dashboard. This autonomy is deliberate — by now, they own the mission."},
    ],
    components:["All systems in demo configuration","Live dashboard configured","Final checklist signed"],
    logbook:"Final system check: every item ticked and signed. Write a message to your future self: 'In 10 years, when you're a real engineer, remember...'",
    wow:"The quiet confidence of a team that has prepared everything they can prepare.",
  },
  { phase:5, session:30, title:"PUBLIC DEMO DAY", subtitle:"Mission Report — The World Is Watching 🏆",
    envelope:"T-ZERO: Parents are seated. Satellites are transmitting. The dashboard is live. This is what 10 weeks of work looked like from the outside. Now show them what it looked like from the inside.",
    sprints:[
      {label:"Mission Control Opens",desc:"Parents and guests seated. Live dashboard visible on projector — all satellites transmitting simultaneously, all data updating. Instructor introduces the program: what students were challenged to do, what they actually did. Then hands the room to the students."},
      {label:"Team Presentations",desc:"Each team presents 5 minutes: 'We are [Agency Name]. Our satellite is [name]. Its mission is to measure [sensors]. Here is live data from our satellite right now. The most important thing we learned to build was [technical concept]. The most important failure we had was [from logbook] — and here is what we learned.'"},
      {label:"Wall of Celebrated Failures Reveal",desc:"The instructor walks parents through the Wall: 30 sessions of mistakes and lessons. 'Every card on this board was a student who failed and kept going. That is the most important skill in engineering.' Parents see the evidence that their children practiced resilience, not just electronics."},
      {label:"Certificate Ceremony + Closure",desc:"Certificates handed to each student individually in front of parents — name called, certificate presented, handshake. Mission patches taken home. Logbooks returned as engineering portfolios. Final words from instructor: 'You built a satellite. You are engineers.' Satellite transmits during the entire ceremony."},
    ],
    components:["All satellite systems transmitting live","Live dashboard on projector","Certificates","Mission patches","Logbooks returned","Parent chairs and event setup"],
    logbook:"Log the last packet received during the ceremony: timestamp, values, RSSI. This is the final entry.",
    wow:"A certificate handed to a student in front of their parents, by a university faculty member, for work they built with their hands.",
  },
];

const componentsList = [
  { cat: "Core Electronics", items: ["ESP32 development boards","BME280 temperature/humidity/pressure sensors","Ra-02 LoRa modules 868 MHz (×2 per team)","LiPo batteries 3.7V","Solar panels 5V/100mA","Breadboards + jumper wire kit","Resistor kit","LEDs (red, green, blue, yellow)"] },
  { cat: "Added Components ⭐", items: ["SSD1306 0.96\" OLED displays","NeoPixel WS2812B RGB LEDs","Neo-6M GPS modules + external antennas","MPU-6050 gyroscope/accelerometer","SD card modules + micro SD cards (FAT32)","Push buttons","SMA connectors for antenna","Wire for antenna cutting (22 AWG)"] },
  { cat: "Tools", items: ["Multimeters (1 per team)","Soldering irons + solder + helping hands","Wire strippers + cutters","Tape measure / measuring wheel","Rulers (for antenna measurement)","Rubber mallet (vibration test)"] },
  { cat: "Structure & Materials", items: ["Aluminum 1U CubeSat frame kits","Stomp rocket kits (Session 1)","Aluminum foil + foam insulation (thermal challenge)","Cable ties","Colored electrical tape","Card stock for nameplates + certificates","Sticker paper for mission patches","Laminator"] },
  { cat: "Classroom", items: ["Graph-paper mission logbooks (1 per student)","Laminated role cards (5 per team)","Sealed mission briefing envelopes (1 per session per team)","Corkboard + pushpins for Wall of Failures","Index cards for failure wall","Power budget game cards (printed)","Projector + internet access","Live dashboard laptop"] },
];

function App() {
  const [activePhase, setActivePhase] = useState(1);
  const [expandedSession, setExpandedSession] = useState(null);
  const [view, setView] = useState("sessions");
  const currentPhase = phases.find(p => p.id === activePhase);
  const phaseSessions = allSessions.filter(s => s.phase === activePhase);

  return (
    <div style={{minHeight:"100vh",background:"#080c14",fontFamily:"'Courier New',monospace",color:"#c8d8e8"}}>
      <div style={{position:"fixed",inset:0,zIndex:0,backgroundImage:"radial-gradient(1px 1px at 20% 30%,rgba(200,220,255,0.4) 0%,transparent 100%),radial-gradient(1px 1px at 70% 15%,rgba(200,220,255,0.3) 0%,transparent 100%),radial-gradient(1px 1px at 40% 70%,rgba(200,220,255,0.35) 0%,transparent 100%)",pointerEvents:"none"}}/>
      <div style={{position:"relative",zIndex:1,maxWidth:900,margin:"0 auto",padding:"0 16px 60px"}}>
        
        {/* Header */}
        <div style={{textAlign:"center",padding:"40px 0 24px"}}>
          <div style={{fontSize:11,letterSpacing:6,color:"#00D4FF",marginBottom:12,opacity:0.8}}>MCC JUNIOR SPACE AGENCY PROGRAM</div>
          <h1 style={{fontSize:"clamp(20px,5vw,36px)",fontWeight:700,color:"#fff",margin:0,letterSpacing:2,textShadow:"0 0 40px rgba(0,212,255,0.4)"}}>🛰️ CUBESAT SUMMER CAMP</h1>
          <div style={{fontSize:12,color:"#7a9ab8",marginTop:10,letterSpacing:1}}>30 SESSIONS · 10 WEEKS · 3 DAYS/WEEK · 2 HOURS/SESSION · 60 HOURS TOTAL</div>
          <div style={{display:"flex",justifyContent:"center",gap:8,flexWrap:"wrap",marginTop:20}}>
            {[["5","PHASES"],["30","SESSIONS"],["5","ROLES"],["7+","NEW COMPONENTS"],["1","DEMO DAY"]].map(([n,l])=>(
              <div key={l} style={{background:"rgba(0,212,255,0.06)",border:"1px solid rgba(0,212,255,0.15)",borderRadius:6,padding:"8px 16px",textAlign:"center"}}>
                <div style={{fontSize:20,fontWeight:700,color:"#00D4FF"}}>{n}</div>
                <div style={{fontSize:9,letterSpacing:2,color:"#5a7a98"}}>{l}</div>
              </div>
            ))}
          </div>
        </div>

        {/* View toggle */}
        <div style={{display:"flex",gap:8,justifyContent:"center",marginBottom:24}}>
          {["sessions","components"].map(v=>(
            <button key={v} onClick={()=>setView(v)} style={{background:view===v?"rgba(0,212,255,0.15)":"transparent",border:`1px solid ${view===v?"#00D4FF":"rgba(255,255,255,0.1)"}`,color:view===v?"#00D4FF":"#5a7a98",padding:"8px 24px",borderRadius:6,cursor:"pointer",fontSize:11,letterSpacing:2,fontFamily:"inherit",textTransform:"uppercase"}}>
              {v==="sessions"?"📋 SESSION OUTLINE":"🔩 COMPONENTS LIST"}
            </button>
          ))}
        </div>

        {view==="components"?(
          <div>
            <div style={{fontSize:11,letterSpacing:4,color:"#FFB800",marginBottom:16,textAlign:"center"}}>FULL EQUIPMENT LIST — ALL 30 SESSIONS</div>
            {componentsList.map(cat=>(
              <div key={cat.cat} style={{background:"rgba(255,255,255,0.03)",border:"1px solid rgba(255,255,255,0.08)",borderRadius:10,padding:20,marginBottom:12}}>
                <div style={{fontSize:11,letterSpacing:3,color:"#FFB800",marginBottom:12}}>{cat.cat.toUpperCase()}</div>
                <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(260px,1fr))",gap:8}}>
                  {cat.items.map(item=>(
                    <div key={item} style={{display:"flex",alignItems:"flex-start",gap:8,fontSize:12,color:"#a0b8d0",lineHeight:1.4}}>
                      <span style={{color:"#00D4FF",flexShrink:0}}>›</span>{item}
                    </div>
                  ))}
                </div>
              </div>
            ))}
          </div>
        ):(
          <>
            {/* Phase tabs */}
            <div style={{display:"flex",gap:6,marginBottom:20,flexWrap:"wrap",justifyContent:"center"}}>
              {phases.map(phase=>(
                <button key={phase.id} onClick={()=>{setActivePhase(phase.id);setExpandedSession(null);}} style={{background:activePhase===phase.id?`${phase.color}18`:"rgba(255,255,255,0.03)",border:`1px solid ${activePhase===phase.id?phase.color:"rgba(255,255,255,0.08)"}`,borderRadius:8,padding:"10px 14px",cursor:"pointer",fontFamily:"inherit",color:activePhase===phase.id?phase.color:"#5a7a98",textAlign:"center",minWidth:110}}>
                  <div style={{fontSize:16}}>{phase.icon}</div>
                  <div style={{fontSize:9,letterSpacing:2,fontWeight:700,marginTop:2}}>PHASE {phase.id}</div>
                  <div style={{fontSize:9,letterSpacing:1,opacity:0.8}}>{phase.name}</div>
                  <div style={{fontSize:9,color:activePhase===phase.id?phase.color:"#3a5a78",marginTop:2}}>{phase.weeks} · S{phase.sessions}</div>
                </button>
              ))}
            </div>

            {/* Phase header */}
            <div style={{background:`${currentPhase.color}10`,border:`1px solid ${currentPhase.color}30`,borderRadius:10,padding:"16px 20px",marginBottom:16,display:"flex",justifyContent:"space-between",alignItems:"center",flexWrap:"wrap",gap:8}}>
              <div>
                <div style={{fontSize:9,letterSpacing:3,color:currentPhase.color,marginBottom:4}}>PHASE {currentPhase.id} — {currentPhase.name}</div>
                <div style={{fontSize:13,color:"#8aa8c0"}}>{currentPhase.weeks} · Sessions {currentPhase.sessions} · {phaseSessions.length} sessions × 2 hours = {phaseSessions.length*2} hours</div>
              </div>
              <div style={{fontSize:32}}>{currentPhase.icon}</div>
            </div>

            {/* Sessions */}
            <div style={{display:"flex",flexDirection:"column",gap:10}}>
              {phaseSessions.map(session=>{
                const isOpen=expandedSession===session.session;
                return(
                  <div key={session.session} style={{background:isOpen?`${currentPhase.color}08`:"rgba(255,255,255,0.02)",border:`1px solid ${isOpen?currentPhase.color+"40":"rgba(255,255,255,0.07)"}`,borderRadius:10,overflow:"hidden"}}>
                    <button onClick={()=>setExpandedSession(isOpen?null:session.session)} style={{width:"100%",background:"transparent",border:"none",padding:"16px 20px",cursor:"pointer",fontFamily:"inherit",display:"flex",alignItems:"flex-start",gap:16,textAlign:"left"}}>
                      <div style={{background:isOpen?currentPhase.color:"rgba(255,255,255,0.06)",color:isOpen?"#000":"#5a7a98",borderRadius:6,width:36,height:36,flexShrink:0,display:"flex",alignItems:"center",justifyContent:"center",fontSize:12,fontWeight:700}}>
                        {session.session}
                      </div>
                      <div style={{flex:1}}>
                        <div style={{fontSize:14,fontWeight:700,color:isOpen?currentPhase.color:"#c8d8e8"}}>{session.title}</div>
                        <div style={{fontSize:11,color:"#5a7a98",marginTop:2,letterSpacing:1}}>{session.subtitle}</div>
                      </div>
                      <div style={{color:currentPhase.color,fontSize:16,marginTop:4}}>{isOpen?"▲":"▼"}</div>
                    </button>
                    {isOpen&&(
                      <div style={{padding:"0 20px 20px"}}>
                        <div style={{background:"rgba(255,184,0,0.06)",border:"1px solid rgba(255,184,0,0.2)",borderRadius:8,padding:"12px 16px",marginBottom:16,fontSize:11,color:"#c8a040",lineHeight:1.6}}>
                          <span style={{letterSpacing:2,fontSize:9,display:"block",marginBottom:6,opacity:0.7}}>📨 MISSION BRIEFING ENVELOPE</span>
                          {session.envelope}
                        </div>
                        <div style={{marginBottom:16}}>
                          <div style={{fontSize:9,letterSpacing:3,color:currentPhase.color,marginBottom:10}}>SPRINT BREAKDOWN — 4 × 30 MINUTES</div>
                          <div style={{display:"flex",flexDirection:"column",gap:8}}>
                            {session.sprints.map((sprint,i)=>(
                              <div key={i} style={{display:"flex",gap:12,alignItems:"flex-start"}}>
                                <div style={{background:`${currentPhase.color}20`,color:currentPhase.color,borderRadius:4,padding:"2px 8px",fontSize:9,letterSpacing:1,flexShrink:0,marginTop:2,whiteSpace:"nowrap"}}>S{i+1}</div>
                                <div>
                                  <div style={{fontSize:12,fontWeight:700,color:"#e0e8f0",marginBottom:4}}>{sprint.label}</div>
                                  <div style={{fontSize:11,color:"#7a9ab8",lineHeight:1.7,whiteSpace:"pre-line"}}>{sprint.desc}</div>
                                </div>
                              </div>
                            ))}
                          </div>
                        </div>
                        <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:10}}>
                          <div style={{background:"rgba(255,255,255,0.03)",borderRadius:8,padding:12,border:"1px solid rgba(255,255,255,0.07)"}}>
                            <div style={{fontSize:9,letterSpacing:2,color:"#7a9ab8",marginBottom:6}}>📓 LOGBOOK ENTRY</div>
                            <div style={{fontSize:11,color:"#8aa8c0",lineHeight:1.6}}>{session.logbook}</div>
                          </div>
                          <div style={{background:`${currentPhase.color}08`,borderRadius:8,padding:12,border:`1px solid ${currentPhase.color}25`}}>
                            <div style={{fontSize:9,letterSpacing:2,color:currentPhase.color,marginBottom:6}}>⚡ WOW MOMENT</div>
                            <div style={{fontSize:11,color:"#8aa8c0",lineHeight:1.6}}>{session.wow}</div>
                          </div>
                        </div>
                        <div style={{marginTop:10}}>
                          <div style={{fontSize:9,letterSpacing:2,color:"#5a7a98",marginBottom:6}}>🔩 SESSION COMPONENTS</div>
                          <div style={{display:"flex",flexWrap:"wrap",gap:6}}>
                            {session.components.map(c=>(
                              <span key={c} style={{background:"rgba(255,255,255,0.04)",border:"1px solid rgba(255,255,255,0.08)",borderRadius:4,padding:"3px 8px",fontSize:10,color:"#6a8aaa"}}>{c}</span>
                            ))}
                          </div>
                        </div>
                      </div>
                    )}
                  </div>
                );
              })}
            </div>
          </>
        )}

        <div style={{textAlign:"center",marginTop:40,paddingTop:24,borderTop:"1px solid rgba(255,255,255,0.06)",fontSize:10,color:"#3a5a78",letterSpacing:2}}>
          MCC MICROCONTROLLERS CORPORATION · JUNIOR SPACE AGENCY PROGRAM · ZARQA, JORDAN
        </div>
      </div>
    </div>
  );
}

ReactDOM.createRoot(document.getElementById('root')).render(<App/>);
</script>
</body>
</html>
