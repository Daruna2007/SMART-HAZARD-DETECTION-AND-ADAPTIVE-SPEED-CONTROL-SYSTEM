# **SMART HAZARD DETECTION AND ADAPTIVE SPEED CONTROL SYSTEM**

## **1\. Aim**

To develop a miniature smart vehicle prototype that detects different road hazard conditions using camera-based gesture recognition and automatically adjusts the vehicle's speed and warning indicators according to the detected condition.

## **2\. Problem Statement**

Road hazards such as potholes, speed breakers, and obstacles require quick reactions from vehicle riders. Delayed responses or sudden reactions can affect vehicle stability and safety.

The proposed system demonstrates an automated hazard-response mechanism in which different road conditions are simulated using hand gestures. The system identifies the simulated hazard and automatically controls the vehicle's motor speed, LED, and buzzer.

The prototype uses a miniature motorized bike to demonstrate how hazard detection can be connected to automatic vehicle control.

## **3\. Objectives**

* To develop a miniature motorized bike prototype.  
* To detect simulated road hazards using camera-based gesture recognition.  
* To classify different hazard conditions based on detected gesture counts.  
* To automatically control vehicle motor speed according to the detected hazard.  
* To provide an audible warning for pothole detection.  
* To provide a visual warning for obstacle detection.  
* To return the vehicle to normal speed when no hazard is detected.  
* To integrate Python, OpenCV, MediaPipe, Arduino, and motor control into a single prototype.  
* To demonstrate a scalable architecture for future real-world road hazard detection.

**4\. Proposed Solution**

The system consists of a camera, Python-based gesture recognition system, Arduino Uno, L298N motor driver, DC motor, LED, and buzzer.

A hand gesture is used to simulate different road conditions during the prototype demonstration.

The camera captures the user's hand, while Python and MediaPipe process the image and determine the number of detected fingers.

The detected count is converted into a corresponding command and transmitted to the Arduino through serial communication.

The Arduino then controls the motor, LED, and buzzer according to the detected condition.

The prototype uses the following hazard mapping:

* **5 fingers → Pothole**  
* **4 fingers → Speed Breaker**  
* **3 fingers → Object / Obstacle**  
* **0–2 fingers → Clear Road**


## **5\. Components Required**

* Arduino Uno  
* L298N Motor Driver  
* DC Gear Motor  
* Miniature Bike Chassis  
* Rear Wheel  
* LED  
* 220 Ω Resistor  
* Buzzer  
* External Battery / Power Supply  
* Laptop Camera / Webcam  
* Jumper Wires  
* Breadboard  
* Connecting Wires  
* Laptop for Python Processing

### **Software**

* Python 3.9  
* OpenCV  
* MediaPipe  
* PySerial  
* Arduino IDE

## **6\. Hardware Setup**

The prototype was physically assembled using a miniature bike structure with a motor-driven rear wheel.

The DC motor was connected to the rear wheel through the L298N motor driver. The Arduino Uno was used as the main controller for motor speed and warning devices.

A camera connected to the laptop captures the hand gesture. Python processes the camera input and sends the corresponding command to the Arduino through serial communication.

The LED and buzzer were connected to the Arduino to provide visual and audible hazard indications.

The prototype provides a physical demonstration of automatic vehicle response to different simulated road conditions.

## **7\. Pin Configuration**

| Component | Arduino Pin |
| ----- | ----- |
| L298N ENA | D5 |
| L298N IN1 | D7 |
| L298N IN2 | D8 |
| LED | D10 |
| Buzzer | D11 |
| Motor | L298N OUT1 / OUT2 |
| L298N GND | Arduino GND |

The motor speed is controlled using PWM through the **D5 ENA** pin.

The **ENA jumper on the L298N is removed** when using Arduino PWM speed control.

## **8\. Working Principle**

The system is powered on.

The Arduino initializes the motor driver, LED, buzzer, and serial communication.

The laptop camera captures the user's hand gesture.

The Python program processes the camera frame using OpenCV and MediaPipe.

The system determines the detected finger count.

The detected count is mapped to a simulated road condition.

The corresponding command is transmitted to the Arduino through serial communication.

The Arduino processes the command and controls the motor, LED, and buzzer.

The vehicle then changes its behaviour according to the detected road condition.

## **9\. Hazard Response Control**

The system uses four different operating conditions.

### **Pothole — 5 Fingers**

* Motor → Slow  
* Buzzer → ON  
* LED → OFF

The vehicle reduces its speed and activates an audible warning.

### **Speed Breaker — 4 Fingers**

* Motor → Slow  
* Buzzer → OFF  
* LED → OFF

The vehicle reduces its speed while approaching the simulated speed breaker.

### **Object / Obstacle — 3 Fingers**

* Motor → Slow  
* Buzzer → OFF  
* LED → ON

The vehicle reduces its speed and activates a visual warning.

### **Clear Road — 0–2 Fingers**

* Motor → Normal Speed  
* Buzzer → OFF  
* LED → OFF

The vehicle operates normally when no hazard is detected.

---

## **10\. Camera-Based Gesture Recognition**

The camera provides the input for the prototype.

Python processes the captured video using OpenCV and MediaPipe.

MediaPipe detects the hand landmarks, and the program determines the number of detected fingers.

The finger count is then converted into a hazard command.

The gesture itself is used only as a **simulation method** for the prototype.

In a real-world implementation, the same control system could receive hazard information from road-facing cameras, sensors, or other detection technologies.

## **11\. Arduino Motor Control**

The Arduino controls the DC motor through the L298N motor driver.

PWM is used to control the motor speed.

The system uses three basic motor states:

**Normal Speed**

PWM \= 200

**Slow Speed**

PWM \= 100

**Stop**

PWM \= 0

In the current prototype, pothole, speed breaker, and obstacle conditions use reduced motor speed, while the clear-road condition uses normal motor speed.

---

## **12\. Warning System**

The prototype uses two warning mechanisms.

### **Buzzer**

The buzzer is activated during pothole detection to provide an audible warning.

### **LED**

The LED is activated during obstacle detection to provide a visual warning.

This allows the system to provide different types of feedback depending on the detected road condition.

---

## **13\. Serial Communication**

Python communicates with the Arduino through serial communication.

The Arduino is connected through:

COM5

The commands transmitted by Python are:

| Command | Condition |
| ----- | ----- |
| `P` | Pothole |
| `B` | Speed Breaker |
| `O` | Object / Obstacle |
| `N` | Clear / Normal |

The Arduino receives these commands and activates the corresponding vehicle response.

## **14\. Testing and Verification**

During development, the following functions were tested:

* Camera input  
* Hand detection  
* Finger-count detection  
* Gesture classification  
* Python-to-Arduino serial communication  
* Arduino command processing  
* Motor speed control  
* L298N motor driver  
* LED warning system  
* Buzzer warning system  
* Miniature bike rear-wheel movement  
* Integration of the complete prototype

The motor was tested at different PWM values to verify normal and reduced-speed operation.

The LED and buzzer were also tested individually before integrating them into the complete system.

## **15\. Advantages**

* Provides automatic response to simulated road hazards.  
* Demonstrates adaptive motor-speed control.  
* Provides both visual and audible warnings.  
* Combines computer vision with embedded systems.  
* Uses relatively low-cost components.  
* Provides a physical miniature vehicle demonstration.  
* Can be extended with real road-hazard detection technologies.  
* Demonstrates real-time communication between Python and Arduino.

## **16\. Limitations**

* Hand gestures are currently used to simulate road hazards.  
* The prototype does not currently detect actual potholes or obstacles on the road.  
* The system depends on camera visibility and hand detection.  
* Motor control is demonstrated using a miniature vehicle.  
* The current prototype does not include actual vehicle sensors.  
* The system requires further testing under different lighting and environmental conditions.

## **17\. Future Scope**

Future improvements can include:

* Real-time pothole detection using computer vision.  
* Road obstacle detection using cameras.  
* Ultrasonic sensor integration.  
* LiDAR-based obstacle detection.  
* Machine-learning-based hazard classification.  
* Real-time road condition analysis.  
* GPS-based hazard mapping.  
* Automatic speed control for real vehicles.  
* Rider warning systems.  
* Mobile or cloud-based monitoring.  
* Integration with actual two-wheelers and automotive systems.

## **18\. Result**

A working miniature smart vehicle prototype was developed with a motor-driven rear wheel, Arduino-based control, L298N motor driver, LED, and buzzer.

Camera-based gesture recognition was implemented using Python, OpenCV, and MediaPipe.

The system successfully demonstrated different vehicle responses for simulated road conditions:

* **5 fingers → Pothole → Motor Slow \+ Buzzer**  
* **4 fingers → Speed Breaker → Motor Slow**  
* **3 fingers → Object → Motor Slow \+ LED**  
* **0–2 fingers → Clear Road → Normal Speed**

The prototype demonstrates the integration of computer vision, serial communication, embedded control, and adaptive vehicle response.

## **19\. Project Images**

Add your prototype photographs under this section.

### **Prototype – Miniature Smart Vehicle**

*Add image of the complete bike prototype.*

### **Rear Wheel Motor Assembly**

*Add image showing the motor connected to the rear wheel.*

### **Hardware Setup**

*Add image showing Arduino, L298N, LED, buzzer, and wiring.*

### **Gesture Detection Interface**

*Add screenshot of the Python/MediaPipe camera interface.*

### **Complete System**

*Add image showing the complete setup during operation.*

---

## **20\. Conclusion**

The Smart Hazard Detection and Adaptive Speed Control System demonstrates how computer vision and embedded control can be combined to create an intelligent vehicle-response prototype.

The miniature bike responds to different simulated road conditions by automatically adjusting its motor speed and activating appropriate warning indicators.

The project provides a foundation for developing a more advanced road-hazard detection system using real-world cameras, sensors, and machine-learning techniques.

**The prototype demonstrates the complete concept:**

**Detect → Classify → Decide → Respond**

> 

