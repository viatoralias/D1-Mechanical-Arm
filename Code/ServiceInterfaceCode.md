# D1-550 Service Interface Code

> This commands are to be used when using the Unitree's D1-550 robotic arm ROS interface.

<dl>
    <dt>Setting the context</dt>
    <dd>Instructions are divided into 4 different classes: "seq" which is the instruction's unique identifier, "adress" which is the instruction's adress code, "funcode" which is instruction's function code, and "data" which is the instruction's data content.  
    All data is transmitted using strings in Json format. The "seq" part of the instruction is automatically generated.</dd>
</dl>

## Instruction 1: Angle control of a single joint
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

## Instruction 2: Angle control of all joints
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

## Instruction 3: Locked/Free state of a single joint
- `seq`: Automatically generated.
- `adress`: 1.
- `funcode`: 4.
- `data`: Mode.  
**Example of such an instruction**:
```json
{"seq":4,"address":1,"funcode":4,"data":{"id": joint id,"mode": enable mode}}
```
> `id` indicates the joint ID, `mode 0` is to lock the joint, while `mode 1` is to enable the joint.  

## Instruction 4: Locked/Free state of all the joints
- `seq`: Automatically generated.
- `adress`: 1.
- `funcode`: 5.
- `data`: Mode.  
**Example of such an instruction**:
```json
{"seq":4,"address":1,"funcode":5,"data":{"mode": enable mode}}
```
> `id` indicates the joint ID, `mode:0` is to lock the joint, while `mode:1` is to enable the joint.  

## Instruction 5: Power supply On/Off
- `seq`: Automatically generated.
- `adress`: 1.
- `funcode`: 6.
- `data`: Mode.  
**Example of such an instruction**:
```json
{"seq":4,"address":1,"funcode":6,"data":{"power": enable mode}}
```
> `power:0` is to power the power supply off, while `power:1` is to power it on.  

## Instruction 6: All joints reset to Zero-Position
- `seq`: Automatically generated.
- `adress`: 1.
- `funcode`: 7.
- `data`: no data.  
**Example of such an instruction**:
```json
{"seq":4,"address":1,"funcode":7}
```  

## Instruction 7: Joints' angle feedback
- `seq`: 10.
- `adress`: 2.
- `funcode`: 1.
- `data`: Angles 0~6.  
**Example of such an instruction**:
```json
{"seq":10,"address":2,"funcode":1,"data":{"angle0": joint 0 Angle value,"angle1":joint 1 Angle value,"angle2":joint 2 Angle value,"angle3":joint 3 point of value,"angle4": joint 4 angles Value,"angle5": joint 5 angle value,"angle6": joint 6 angle value}}
```
> A real-time feedback of the manipulator's joints angles, uploaded with a frequency of 10Hz.  

## Instruction 8: Execution commands' feedback
- `seq`: Automatically generated.
- `adress`: 3.
- `funcode`: 2.
- `data`: exec_status.  
**Example of such an instruction**:
```json
{"seq":10,"address":3,"funcode":2,"data":{"exec_status": execution status}}
```
> If `exec_status:1`, the execution succeeds. If `exec_status:0`, the execution fails.