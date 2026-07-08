MPU6050 3D Cube Project Brief
Project Overview
A real-time 3D visualization system that uses an MPU6050 accelerometer to control a 3D cube on screen. The cube rotates in response to physical movements of the sensor.
 Hardware Setup
ESP32-C3 + MPU6050
 ESP32-C3 (Microcontroller)
 MPU6050 (Motion Sensor)
 Wiring:
 VCC → 3.3V
 GND → GND  
 SDA → GPIO7
 SCL → GPIO6
 USB → Computer (COM11)
 Software Architecture
1. Arduino/ESP32 Code (Data Collection)
// Reads raw accelerometer data
void loop() {
  // Reads: ax, ay, az (raw values: -17000 to +17000)
  Serial.print(ax); Serial.print(",");
  Serial.print(ay); Serial.print(","); 
  Serial.println(az); // Sends: "123,-456,16384"
}
2. Processing Code (3D Visualization)
java
// Receives data and creates 3D graphics
void serialEvent() {
  // Parses: "123,-456,16384" → ax=123, ay=-456, az=16384
}

void draw() {
  // Maps MPU data to 3D cube rotation
  rotateX = map(ay, -17000, 17000, -PI, PI);
  rotateY = map(ax, -17000, 17000, -PI, PI);
  // Applies rotation to 3D cube
}
Data Flow
text
MPU6050 (Physical Movement)
         ↓
     Raw Accelerometer Data
         ↓  
     ESP32-C3 (Serial)
         ↓
     "ax,ay,az" over COM11
         ↓
   Processing (3D Rendering)
         ↓
    Real-time Cube Rotation
 Control Scheme
• Tilt Left/Right → Cube rotates around Y-axis
• Tilt Forward/Back → Cube rotates around X-axis
• Stationary → Cube remains still
• Movement → Direct 1:1 rotation mapping
 Key Technical Features
MPU6050 Data Range
• X-axis: -17000 to +17000 (Left to Right tilt)
• Y-axis: -17000 to +17000 (Forward to Back tilt)
• Z-axis: ~16384 (Gravity when flat)
3D Rendering
• P3D Renderer: Hardware-accelerated 3D graphics
• Lighting: Directional + ambient lights
• Cube: 6 colored faces with wireframe
• Real-time: 60fps smooth rotation
Sensitivity Control
java
// Adjust these values:
float deadZone = 100;    // Ignore small movements
float sensitivity = 0.001; // Rotation speed
