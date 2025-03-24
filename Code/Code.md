# Code

## 1. Different commmands for angle control
<dl>
    <dt>Setting the context</dt>
    <dd>Instructions are divided into 4 different classes: **seq** which is the unstruction's unique identifier, **adress** which is the instruction's adress code, **funcode** which is instruction's function code, and **data** which is the instruction's data content.
    > All data is transmitted using strings in Json format. The **seq** part of the instruction is automatically generated.</dd>
</dl>

### Instruction 1: Angle control of a single joint
- **seq**: Automatically generated.
- **adress**: 1.
- **funcode**: 1.
- **data**: ID, Angle, Delay (ms).
**Example of such an instruction**:
```json
{"seq":4,"address":1,"funcode":1",data":{"id": joint number,"angle": joint target angle,"delay_ms": execution time}}
```