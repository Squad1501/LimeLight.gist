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

<details>
<summary>Tracking AprilTags</summary>

AprilTags are tracked using the "tx", "ty", and "ta" values in NetworkTables, just like standard retroreflective targets! No code changes are required to upgrade a retroreflective tracking robot to apriltags. "botpose" and "campose" may also be used for field-space and target-space 3D tracking.

For more advanced usage with multiple tags, the JSON results dump may be used.

Do not feel pressured to use the more advanced features in the "Advanced" pages unless you know you need them. Many of the very best teams in FRC use the simplest techniques available to maximize reliability and speed. If you frequent Discord, CD, and regionals with elite teams, you may get the impression that you need the most advanced software possible to win events, but this is simply not true.

Our message to many of the teams we help is "It's ok to do the simple thing."

## Quick Start for FRC AprilTags

    - Input Tab - Change "Pipeline Type" to "Fiducial Markers"

    - Standard Tab - Make sure "family" is set to "AprilTag Classic 36h11"

    - Input Tab - Set "Black Level" to zero

    - Input Tab - Set "Gain" to 15

    - Input Tab - Reduce exposure to reduce motion blur. Stop reducing once tracking reliability decreases.

    - Standard Tab - If would like to increase your framerate, increase the "Detector Downscale"

    - Input Tab - For increased range and/or accuracy, increase the capture resolution.

    - If you're seeing spurious tag detections, add the ID's you want to track to the "filter" control or increase the "Quality     Threshold" value.

    - Click the "Gear" Icon, and make sure your team number is set and that a static IP is configured.

    - Click "Change Team Number" and "Change IP Settings" if you changed their corresponding settings. Powercycle your robot.

    - You're done! Use "tx" and "ty" from networktables. Copy the code sample on the "getting started" page.

## Tips

For ideal tracking, consider the following:

    - Your tags should be as flat as possible.
     
    - Your Limelight should be mounted above or below tag height and angled up/down such that the target is centered. Your target should look as trapezoidal as possible from your camera's perspective. You don't want your camera to ever be completely "head-on" with a tag if you want to avoid tag flipping.

There is an interplay between the following variables for AprilTag Tracking:

    - Increasing capture resolution will always increase 3D accuracy and increase 3d stability. This will also reduce the rate of ambiguity flipping from most perspectives. It will usually increase range. This will reduce pipeline framerate.

    - Increasing detector downscale will always increase pipeline framerate. It will decrease effective range, but in some cases this may be negligible. It will not affect 3D accuracy, 3D stability, or decoding accuracy.

    - Reducing exposure will always improve motion-blur resilience. This is actually really easy to observe. This may reduce range.
    
    - Reducing the brightness and contrast of the image will generally improve pipeline framerate and reduce range.
    - Increasing Sensor gain allows you to increase brightness without increasing exposure. It may reduce 3D stability, and it may reduce tracking stability.

## Input Tab

The Input Tab hosts controls to change the raw camera image before it is passed through the processing pipeline. See the "Building a retroreflective/color pipeline" page for more details.

To trakc ApilTags:

    - Change "Pipeline Type" to "Fiducial Markers"

    - Set "Black Level" to zero

At this point, it is a matter of balancing sensor gain and exposure time. You want to be able to see the tags with the smallest exposure possible to minimize motion blur. This usually calls for a high sensor gain setting. For simple 2D tracking, it is often advisable to max-out your sensor gain, and then increase your exposure from zero until targets are sufficiently tracked. Make sure the correct family is selected in the "Standard" tab if tracking isn't working.

## Standard Tab

### Family

Selects the fiducial/AprilTag family type. For FRC, you should select "AprilTag Classic 36h11"

### Marker Size

Sets the expected size of the tags your robot will encounter in mm. For FRC, this should be set to 165.1 (152.4 for 2023 tags)

### Dectoctor Downscale

Increasing this number will result in significant performance boosts. This this will sometimes result in reduced range, but the cost is usually minimal.

### ID Filters

ID Filters allow you specify exactly which tags you care about. For most FRC teams, each pipeline should be configured to track exactly one tag ID. This is a comma-separated list of numbers (eg. "0,1"). This feature is important for eliminating the vast majority of false-positives.

### Cropping

Cropping removes content from the image for huge performance boosts. Use the NT "crop" key to crop drynamically during matches.

### Multi-Target Sorting and Grouping

This allows for the exact grouping functionality seen in standard retroreflective pipelines. In most games, the only feature to modify is the "Area" filter, which will allow you to filter-out small tags.

</details>

