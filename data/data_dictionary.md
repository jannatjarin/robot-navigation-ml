# Data Dictionary

The project uses the 24-sensor version of the UCI Wall-Following Robot Navigation dataset.

All `US1`–`US24` variables are numerical ultrasonic sensor readings. The UCI repository does not specify a measurement unit for these variables.

| Variable | Type | Description |
|---|---|---|
| US1 | Numerical | Ultrasonic sensor reading at 180° (front of robot) |
| US2 | Numerical | Ultrasonic sensor reading at -165° |
| US3 | Numerical | Ultrasonic sensor reading at -150° |
| US4 | Numerical | Ultrasonic sensor reading at -135° |
| US5 | Numerical | Ultrasonic sensor reading at -120° |
| US6 | Numerical | Ultrasonic sensor reading at -105° |
| US7 | Numerical | Ultrasonic sensor reading at -90° |
| US8 | Numerical | Ultrasonic sensor reading at -75° |
| US9 | Numerical | Ultrasonic sensor reading at -60° |
| US10 | Numerical | Ultrasonic sensor reading at -45° |
| US11 | Numerical | Ultrasonic sensor reading at -30° |
| US12 | Numerical | Ultrasonic sensor reading at -15° |
| US13 | Numerical | Ultrasonic sensor reading at 0° (back of robot) |
| US14 | Numerical | Ultrasonic sensor reading at 15° |
| US15 | Numerical | Ultrasonic sensor reading at 30° |
| US16 | Numerical | Ultrasonic sensor reading at 45° |
| US17 | Numerical | Ultrasonic sensor reading at 60° |
| US18 | Numerical | Ultrasonic sensor reading at 75° |
| US19 | Numerical | Ultrasonic sensor reading at 90° |
| US20 | Numerical | Ultrasonic sensor reading at 105° |
| US21 | Numerical | Ultrasonic sensor reading at 120° |
| US22 | Numerical | Ultrasonic sensor reading at 135° |
| US23 | Numerical | Ultrasonic sensor reading at 150° |
| US24 | Numerical | Ultrasonic sensor reading at 165° |
| Class | Categorical | Robot navigation action |

## Target Classes

The `Class` variable contains four possible navigation actions:

- Move-Forward
- Slight-Right-Turn
- Sharp-Right-Turn
- Slight-Left-Turn