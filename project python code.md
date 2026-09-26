import cv2  
import mediapipe as mp  
import serial  
import time  
import math

\# \==========================================  
\# ARDUINO  
\# \==========================================

ARDUINO\_PORT \= "COM5"  
BAUD\_RATE \= 9600

try:  
    arduino \= serial.Serial(  
        ARDUINO\_PORT,  
        BAUD\_RATE,  
        timeout=1  
    )

    time.sleep(2)

    print("Arduino connected on COM5")

except Exception as e:

    print("Arduino connection failed:")  
    print(e)  
    exit()

\# \==========================================  
\# MEDIAPIPE  
\# \==========================================

mp\_hands \= mp.solutions.hands  
mp\_draw \= mp.solutions.drawing\_utils

hands \= mp\_hands.Hands(  
    static\_image\_mode=False,  
    max\_num\_hands=1,  
    min\_detection\_confidence=0.6,  
    min\_tracking\_confidence=0.6  
)

\# \==========================================  
\# CAMERA  
\# \==========================================

camera \= cv2.VideoCapture(0)

if not camera.isOpened():

    print("Camera ERROR")

    arduino.close()

    exit()

print("Camera started")  
print()  
print("5 \= POTHOLE")  
print("4 \= SPEED BREAKER")  
print("3 \= OBJECT")  
print("0-2 \= CLEAR")  
print()  
print("Press Q to quit")

\# \==========================================  
\# DISTANCE FUNCTION  
\# \==========================================

def distance(a, b):

    return math.sqrt(  
        (a.x \- b.x) \*\* 2 \+  
        (a.y \- b.y) \*\* 2  
    )

\# \==========================================  
\# FINGER COUNT  
\# \==========================================

def count\_fingers(hand):

    lm \= hand.landmark

    count \= 0

    \# \======================================  
    \# NORMALIZE USING PALM SIZE  
    \# \======================================

    palm\_size \= distance(  
        lm\[0\],  
        lm\[9\]  
    )

    if palm\_size \== 0:

        return 0

    \# \======================================  
    \# FOUR MAIN FINGERS  
    \# \======================================

    \# Each finger is considered extended  
    \# when its tip is sufficiently far from  
    \# the palm.

    finger\_pairs \= \[  
        (8, 5),  
        (12, 9),  
        (16, 13),  
        (20, 17\)  
    \]

    for tip, base in finger\_pairs:

        tip\_distance \= distance(  
            lm\[tip\],  
            lm\[0\]  
        )

        base\_distance \= distance(  
            lm\[base\],  
            lm\[0\]  
        )

        if tip\_distance \> base\_distance \* 1.15:

            count \+= 1

    \# \======================================  
    \# THUMB  
    \# \======================================

    thumb\_tip\_distance \= distance(  
        lm\[4\],  
        lm\[0\]  
    )

    thumb\_base\_distance \= distance(  
        lm\[2\],  
        lm\[0\]  
    )

    if thumb\_tip\_distance \> thumb\_base\_distance \* 1.35:

        count \+= 1

    \# \======================================  
    \# LIMIT  
    \# \======================================

    if count \< 0:  
        count \= 0

    if count \> 5:  
        count \= 5

    return count

\# \==========================================  
\# VARIABLES  
\# \==========================================

previous\_command \= ""

current\_count \= 0

stable\_count \= 0

candidate \= \-1

REQUIRED\_FRAMES \= 6

\# \==========================================  
\# MAIN LOOP  
\# \==========================================

while True:

    success, frame \= camera.read()

    if not success:

        print("Camera error")

        break

    \# Mirror  
    frame \= cv2.flip(  
        frame,  
        1  
    )

    \# RGB  
    rgb \= cv2.cvtColor(  
        frame,  
        cv2.COLOR\_BGR2RGB  
    )

    \# MediaPipe  
    results \= hands.process(rgb)

    detected \= 0

    \# \======================================  
    \# HAND FOUND  
    \# \======================================

    if results.multi\_hand\_landmarks:

        hand \= results.multi\_hand\_landmarks\[0\]

        \# Draw landmarks  
        mp\_draw.draw\_landmarks(  
            frame,  
            hand,  
            mp\_hands.HAND\_CONNECTIONS  
        )

        detected \= count\_fingers(hand)

        \# \==================================  
        \# STABILITY  
        \# \==================================

        if detected \== candidate:

            stable\_count \+= 1

        else:

            candidate \= detected

            stable\_count \= 0

        if stable\_count \>= REQUIRED\_FRAMES:

            current\_count \= detected

    else:

        \# No hand \= CLEAR

        candidate \= 0

        stable\_count \+= 1

        if stable\_count \>= REQUIRED\_FRAMES:

            current\_count \= 0

    \# \======================================  
    \# GESTURE → COMMAND  
    \# \======================================

    if current\_count \== 5:

        command \= "P"

        status \= "POTHOLE"

    elif current\_count \== 4:

        command \= "B"

        status \= "SPEED BREAKER"

    elif current\_count \== 3:

        command \= "O"

        status \= "OBJECT"

    else:

        command \= "N"

        status \= "CLEAR"

    \# \======================================  
    \# SEND COMMAND  
    \# \======================================

    if command \!= previous\_command:

        try:

            arduino.write(  
                command.encode()  
            )

            print(  
                "COUNT:",  
                current\_count,  
                " COMMAND:",  
                command  
            )

            previous\_command \= command

        except Exception as e:

            print("Arduino communication error:")  
            print(e)

            break

    \# \======================================  
    \# SCREEN  
    \# \======================================

    cv2.putText(  
        frame,  
        "COUNT: " \+ str(current\_count),  
        (20, 55),  
        cv2.FONT\_HERSHEY\_SIMPLEX,  
        1.3,  
        (255, 255, 255),  
        3  
    )

    cv2.putText(  
        frame,  
        status,  
        (20, 105),  
        cv2.FONT\_HERSHEY\_SIMPLEX,  
        0.8,  
        (0, 255, 0),  
        2  
    )

    cv2.putText(  
        frame,  
        "Stable: " \+ str(stable\_count),  
        (20, 140),  
        cv2.FONT\_HERSHEY\_SIMPLEX,  
        0.6,  
        (255, 255, 0),  
        2  
    )

    cv2.imshow(  
        "Vehicle Hazard Detection",  
        frame  
    )

    \# \======================================  
    \# QUIT  
    \# \======================================

    if cv2.waitKey(1) & 0xFF \== ord("q"):

        break

\# \==========================================  
\# CLEANUP  
\# \==========================================

try:

    arduino.write(b"N")

except:

    pass

camera.release()

arduino.close()

hands.close()

cv2.destroyAllWindows()

print("System stopped.")

