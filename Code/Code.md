# Code

## 1. Different commmands for angle control
<dl>
    <dt>Setting the context</dt>
    <dd>Instructions are divided into 4 different classes: "seq" which is the unstruction's unique identifier, "adress" which is the instruction's adress code, "funcode" which is instruction's function code, and **data** which is the instruction's data content.  
    All data is transmitted using strings in Json format. The "seq" part of the instruction is automatically generated.</dd>
</dl>

### Instruction 1: Angle control of a single joint
We only control the motor angle of a single joint of the robot arm.
- `seq`: Automatically generated.
- `adress`: 1.
- `funcode`: 1.
- `data`: Id, Angle, Delay (ms).
**Example of such an instruction**:
```json
{"seq":4,"address":1,"funcode":1",data":{"id":joint number,"angle":joint target angle,"delay_ms":execution time}}
```
> `id` is the corresponding joint number, `angle` is the joint's target angle, `delay_ms` is the execution time (generally set to 0).

### Instruction 2: Angle control of all joints
We control the motor angle of all the joints of the robot arm.
- `seq`: Automatically generated.
- `adress`: 1.
- `funcode`: 2.
- `data`: Mode, Angles 0~6.
**Example of such an instruction**:
```json
{"seq":4,"address":1,"funcode":2,"data":{"mode":control mode,"angle0":joint0 angle value,"angle1":joint1 angle value,"angle2":joint2 angle value,"angle3":joint3 angle value,"angle4":joint4 angle value,"angle5":joint5 angle value,"angle6":joint6 angle value}}
```
> `mode` and `angle0~6` are respectively the comtrol mode and the joint (0~6) angle value. mode `0` is the small smothing of 10Hz data, mode `1` is the large smoothing of trajectory use.

### Instruction 3: Locked/Free state of a single joint
- `seq`: Automatically generated.
- `adress`: 1.
- `funcode`: 1.
- `data`: Id, Angle, Delay (ms).
**Example of such an instruction**:
```json
{"seq":4,"address":1,"funcode":1",data":{"id": joint number,"angle": joint target angle,"delay_ms": execution time}}
```
> `id` is the corresponding joint number, `angle` is the joint's target angle, `delay_ms` is the execution time (generally set to 0).