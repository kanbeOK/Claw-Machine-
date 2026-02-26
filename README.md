# Claw-Machine-


---

# 1. Sơ đồ kết nối 


| Linh kiện | Chân trên Arduino | Ghi chú |
| --- | --- | --- |
| **Joystick X** | **A0** | Điều khiển xoay trái/phải |
| **Joystick Y** | **A1** | Điều khiển nâng/hạ cánh tay |
| **Joystick SW** (Nút nhấn) | **2** | Nhấn để đóng/mở kẹp |
| **Servo 1 (Xoay)** | **9** | Đặt ở đế |
| **Servo 2 (Nâng)** | **10** | Điều khiển độ cao |
| **Servo 3 (Kẹp)** | **11** | Điều khiển mỏ gắp |

---

# 2. Code 



```cpp
#include <Servo.h>

Servo servoBase, servoArm, servoClaw;
int joyX = A0, joyY = A1, joyBtn = 2;
bool clawOpen = true;

void setup() {
  servoBase.attach(9);
  servoArm.attach(10);
  servoClaw.attach(11);
  pinMode(joyBtn, INPUT_PULLUP);
}

void loop() {
  // 1. Đọc Joystick và điều khiển góc xoay/nâng
  int xVal = analogRead(joyX);
  int yVal = analogRead(joyY);
  
  int posBase = map(xVal, 0, 1023, 0, 180);
  int posArm = map(yVal, 0, 1023, 45, 135); // Giới hạn góc để tránh va đập cơ khí
  
  servoBase.write(posBase);
  servoArm.write(posArm);

  // 2. Nhấn nút để Đóng/Mở kẹp
  if (digitalRead(joyBtn) == LOW) {
    delay(200); // Chống dội nút (Debounce)
    clawOpen = !clawOpen;
    if (clawOpen) servoClaw.write(10);  // Góc mở
    else servoClaw.write(90);          // Góc đóng (tùy chỉnh cho khít)
  }
  delay(20);
}

```

---

