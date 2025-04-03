# Troubleshooting

## Freezed motors

**Issue**: When testing the arm manipulator for the first time, there was no response from the multiple motors at all. We were left with two choices:  
- Either it is a **_hardware problem_**, meaning the motors were defective, the power supply wasn't sufficient, or the wiring wasn't correctly done.  
- Or it was a **_software problem_**, meaning that, in case the motors were disabled, we needed a command that activated the motors and enable them.  
**Solution**: The motors' Control and communication services were disabled by default. We had to create a command bach to enable them.