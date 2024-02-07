# Limelight Guide

## Imports
<details>
<summary>Imports</summary>

You want to put these with all the other imports for your robot.

```java
import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;
import edu.wpi.first.networktables.NetworkTable;
import edu.wpi.first.networktables.NetworkTableEntry;
import edu.wpi.first.networktables.NetworkTableInstance;
```

</details>

## NetworkTable Instance
<details>
<summary>NetworkTable Instance</summary>

This is how you get the default info.

```java
NetworkTable table = new NetworkTableInstance.getDefault().getTable(“limelight”);
NetworkTableEntry tx = table.getEntry(“tx”);
NetworkTableEntry ty = table.getEntry(“ty”);
NetworkTableEntry ta = table.getEntry(“ta”);
```

</details>

## Read Periodic Values
<details>
<summary>Read Periodic Values</summary>

This will read the values periodically

```java
//read values periodically
double x = tx.getDouble(0.0);
double y = ty.getDouble(0.0);
double area = ta.getDouble(0.0);
```

</details>