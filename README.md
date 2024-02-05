# LimeLight.gist

This is how to add the default Network Tables to your code for the LimeLight 3 for FRC 2024.

<details>
<summary>How to setup the Networking/Comms</summary>

1. Set Team Number
    - Power-up your robot, and connect your laptop to your robot's network.
    -After your Limelight flashes its LED array, open the Limelight Finder Tool and search for your Limelight or navigate to (http://limelight.local:5801). This is the configuration panel.
    - Navigate to the "Settings" tab on the left side of the interface.
    - Enter you team number and press the "Update Team Number" button.

2. Setting the IP Address
    - Change your “IP Assignment” to “Static”.
    - Set your Limelight’s IP address to “10.TE.AM.11”.
    - NOTE: Teams with zeros need to pay special attention:
    - Team 916 uses 10.9.16.xx,
    - Team 9106 uses 10.91.6.xx
    - Team 9016 uses 10.90.16.xx
    - Set the Netmask to “255.255.255.0”.
    - Set the Gateway to “10.TE.AM.1”.
    - Click the “Update” button.
    - Give your roboRIO the following static IP address: “10.TE.AM.2”
    - Power-cycle your robot.
    - You will now be access your config panel at 10.TE.AM.11:5801, and your camera stream at 10.TE.AM.11:5800 

3. Before Connecting to the Field
    - Give your laptop a static IP configuration.
        - IP: 10.TE.AM.5
        - Subnet Mask: 255.0.0.0
        - Gateway: 10.TE.AM.1
    - Give your RIO a static IP configuration.
        - IP: "10.TE.AM.2"
        - Subnet Mask: 255.255.255.0 <- NOTE THE DIFFERENCE HERE
        - Gateway: 10.TE.AM.1
    - Give your Limelights unique hostnames (if using multiple).
    - Give your Limelights unique static IP configurations.
        - Always start with ".11" addresses and go upward. (10.9.87.11, etc.)
        - The use of other addresses may cause your units to malfunction when connected to the FMS.
        - IP: "10.TE.AM.11"
        - Subnet mask: 255.255.255.0
        - Gateway: "10.TE.AM.1"

***Note: Teams with zeros need to pay special attention:***
```
- Team 916 uses 10.9.16.xx
- Team 9106 uses 10.91.6.xx
- Team 9016 uses 10.96.16.xx
```

</details>

<details>
<summary>Programming Quick Start</summary>

# Programing Quick Start

For FRC Teams, the recommended protocol is NetworkTables. Limelight posts all targeting data, including a full JSON dump, to NetworkTables at 100hz. Teams can also set controls such as the ledMode, crop window, and more via NetworkTables. FRC teams may use the Limelight Lib Java and C++ libraries to get started with Limelight in seconds. Limelight Lib is the easiest way to get started

## LimelightLib:
[Java](https://github.com/LimelightVision/limelightlib-wpijava), [CPP](https://github.com/LimelightVision/limelightlib-wpicpp)

<details>
<summary>Java</summary>

```java
double tx = LimelightHelpers.getTX("");
```
</details>

<details>
<summary>C++</summary>

```C++
#include "LimelightHelpers.h"
```

```C++
double tx = LimelightHelpers::getTX("");
double ty = LimelightHelper::getTY("");
```

</details>

## If you want to skip LimelightLib and jump right into programming with NetworkTables:

```java
import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;
import edu.wpi.first.netwroktables.NetworkTable;
import edu.wpi.first.networktables.NetworkTableEntry;
import edu.wpi.first.networktables.NetworkTableInstance;
```

```java
NetworkTable table = NetworkTableInstance.getDefault().getTable("limelight");
NetworkTableEntry tx = table.getEntry("tx");
NetworkTableEntry ty = table.getEntry("ty");
NetworkTableEntry ta = table.getEntry("ta");

//read values periodically
double x = tx.getDouble(0.0);
double y = ty.getDouble(0.0);
double area = ta.getDouble(0.0);

//post to smart dashboard periodically
SmartDashboard.putNumber("LimelightX", x);
SmartDashboard.putNumber("LimelightY", y);
SmartDashboard.putNumber("LimelightArea", area);
```

</details>

