# DJI Mavic 3 Enterprise Multispectral RTK Drone - Waypoint Flight Research Plan

## 1. Key Areas to Research

### a) Official DJI Documentation
- DJI Mavic 3 Enterprise Series User Manual
- DJI SDK Documentation (Mobile SDK and Payload SDK)
- DJI Developer Documentation for waypoint missions
- DJI Terra software capabilities

### b) Technical Specifications
- Flight capabilities
- RTK functionality
- Multispectral sensor specifications
- Flight planning limitations

### c) Development Resources
- DJI Mobile SDK (for iOS/Android)
- Available APIs for waypoint mission planning
- Third-party integration possibilities

## 2. Information Sources

### a) Primary Sources
- [DJI Developer Portal](https://developer.dji.com/)
- [DJI Enterprise Documentation](https://enterprise.dji.com/mavic-3-enterprise)
- [DJI Forum](https://forum.dji.com/)
- GitHub repositories with DJI SDK implementations

### b) Secondary Sources
- Developer blogs and tutorials
- YouTube tutorials on DJI waypoint missions
- Academic papers on drone survey methodologies
- Industry case studies

## 3. Implementation Considerations

### a) Flight Planning
- Understanding waypoint parameters
- Mission planning constraints
- RTK integration requirements
- Multispectral data collection requirements

### b) Safety and Regulations
- Flight safety parameters
- Regulatory requirements
- Failsafe mechanisms
- RTK base station setup

### c) Data Management
- Data collection formats
- Post-processing requirements
- Integration with existing systems

## 4. Next Steps
1. Start with official DJI documentation review
2. Explore existing SDK examples and implementations
3. Identify specific technical requirements for use case
4. Create a proof-of-concept implementation plan

## Research Progress

### Documentation Review
_(To be filled as research progresses)_

### Technical Findings

#### Waypoint Mission Limitations

1. DJI SDK Waypoint Limits:
   - Traditional DJI SDK: Maximum 99 waypoints per mission
   - DJI Terra: Supports up to 65,535 waypoints per route
   - Waypoint 2.0 (newer SDK): Maximum 65,535 waypoints per mission

2. Handling Large Missions (>99 waypoints):
   - Break into multiple smaller missions
   - Use DJI Terra for planning complex routes
   - Consider using Waypoint 2.0 API for newer enterprise drones
   - Mission upload time increases with waypoint count

3. Mission Storage:
   - Local Storage: Limited by device memory
   - Onboard Storage: Varies by drone model
   - Consider chunking large missions into manageable segments

4. Performance Considerations:
   - Battery life becomes critical with large waypoint missions
   - Memory usage on mobile devices
   - Upload time for large mission files
   - RTK fix maintenance across extended missions

#### Existing Implementations and Examples

1. Official DJI Sample Applications:
   - [Mobile SDK Waypoint Sample](https://github.com/dji-sdk/Mobile-SDK-Android/tree/master/Sample Code) - Demonstrates basic waypoint mission setup
   - [DJI GSDemo](https://github.com/dji-sdk/Mobile-SDK-iOS/tree/master/Sample Code) - Ground Station waypoint example

2. Key Open Source Projects:
   - [Litchi](https://flylitchi.com/) - Popular third-party mission planner supporting waypoints
   - [QGroundControl](http://qgroundcontrol.com/) - Open-source ground control station

3. Available APIs and Methods:
   - WaypointMissionOperator class for mission control
   - WaypointMission.Builder for mission configuration
   - Key mission parameters:
     - maxFlightSpeed
     - autoFlightSpeed
     - headingMode
     - flightPathMode
     - finishedAction
     - RTK positioning requirements

4. Implementation Approaches:
   - Mobile app-based control
   - Desktop planning with mission upload
   - Integration with mapping software
   - RTK base station coordination

#### DJI Terra RTK & Image Capture Capabilities

1. RTK Integration:
   - Supports RTK positioning for high-precision waypoint navigation
   - Can maintain centimeter-level positioning accuracy
   - Compatible with D-RTK 2 Mobile Station and Network RTK
   - Records RTK positioning data in image metadata

2. Image Capture Features:
   - Supports automated image capture at waypoints
   - Can trigger all cameras (including multispectral) at specified points
   - Image capture modes:
     - Time interval
     - Distance interval
     - Waypoint-triggered
     - Terrain-aware capture points

3. Multispectral Considerations:
   - Supports synchronized capture of all spectral bands
   - Records RTK coordinates for each image
   - Allows setting overlap ratio for mapping missions
   - Can adjust exposure settings for each band

4. Data Output:
   - Stores RTK positioning data in image EXIF
   - Generates detailed flight logs with RTK status
   - Exports waypoint data with RTK coordinates
   - Compatible with post-processing software

#### Waypoint 2.0 API RTK & Image Capture Capabilities

1. Mission Control Features:
   - Supports RTK positioning throughout mission
   - Allows precise waypoint coordinates with altitude control
   - Can define action types at each waypoint
   - Supports both global and local coordinate systems

2. Image Capture Actions:
   - Supports camera actions at waypoints via `WaypointV2Action`
   - Action types include:
     - `WaypointV2ActionType.CAMERA_SHOOT_PHOTO`
     - `WaypointV2ActionType.CAMERA_RECORD_VIDEO`
     - `WaypointV2ActionType.CAMERA_FOCUS`
     - `WaypointV2ActionType.CAMERA_ZOOM`
   - Can trigger at specific waypoint or between waypoints

3. RTK Integration:
   - Supports RTK positioning data in mission planning
   - Records RTK status in mission telemetry
   - Can maintain RTK fix during entire mission
   - Allows real-time position corrections

4. Limitations and Considerations:
   - Actions need to be programmatically defined
   - More complex setup compared to DJI Terra
   - Requires custom development for multispectral synchronization
   - Need to handle RTK status monitoring manually

5. Data Management:
   - Developer responsible for image metadata storage
   - Must implement own RTK data logging
   - Requires custom solution for data organization
   - Can access raw RTK positioning data

#### Waypoint 2.0 API Details

1. API Structure:
   - Part of DJI Mobile SDK (MSDK)
   - Core Classes:
     - `WaypointV2Mission`: Mission configuration
     - `WaypointV2`: Individual waypoint settings
     - `WaypointV2Action`: Action definitions
     - `WaypointV2MissionOperator`: Mission control

2. Key API Features:
   - Mission Planning:
     - Configure waypoint coordinates
     - Set flight speed and altitude
     - Define heading mode
     - Set gimbal actions
     - Configure camera actions
   
   - Mission Control:
     - Start/stop missions
     - Pause/resume
     - Real-time mission updates
     - Mission state monitoring

3. API Documentation:
   - Official Documentation: https://developer.dji.com/api-references/android-api/Components/Missions/DJIWaypointV2Mission.html
   - Sample Code Repository: https://github.com/dji-sdk/Mobile-SDK-Android
   - API Reference Guide
   - Code Examples
   - Integration Guides

4. Development Requirements:
   - DJI Developer Account
   - Enterprise SDK License
   - Mobile Development Environment:
     - Android Studio / Xcode
     - DJI Mobile SDK
     - Sample Application

5. API Access:
   - Register at developer.dji.com
   - Apply for Enterprise SDK license
   - Download SDK and documentation
   - Access to developer support

#### Third-Party Solution Limitations

1. Litchi Waypoint Capabilities:
   - Maximum 99 waypoints per mission
   - Limited by DJI's traditional SDK restrictions
   - Cannot exceed this limit even with paid version
   - Workarounds:
     - Breaking missions into multiple segments
     - Using "Resume Mission" feature between segments
     - Creating multiple mission files

2. Litchi Mission Features:
   - Supports curve/point-of-interest missions
   - Can trigger camera actions at waypoints
   - Supports basic RTK positioning data
   - Limited multispectral camera support

3. Comparison with DJI Solutions:
   - More limited than DJI Terra (65,535 waypoints)
   - More limited than Waypoint 2.0 API (65,535 waypoints)
   - Easier to use but less flexible
   - Better suited for simpler missions

#### Ground Control Station Solutions

1. QGroundControl Compatibility:
   - Primary support for MAVLink-based systems (ArduPilot, PX4)
   - No direct support for DJI Mavic 3 Enterprise
   - Cannot interface directly with DJI's closed ecosystem
   - Would require custom firmware/hardware modifications

2. QGroundControl Capabilities (for supported drones):
   - Can handle thousands of waypoints (limited by vehicle firmware)
   - Supports complex mission planning
   - Advanced RTK integration
   - Comprehensive flight data logging
   - Survey planning tools

3. DJI Integration Challenges:
   - DJI uses proprietary protocols
   - No official MAVLink support on Mavic series
   - Third-party adapters exist but:
     - May void warranty
     - Require hardware modifications
     - Risk of firmware incompatibility

4. Alternative Approach:
   - Stick with DJI's ecosystem (Terra or Waypoint 2.0 API)
   - Both support required waypoint count (65,535)
   - Maintain warranty and official support
   - Native multispectral camera integration

#### Open Source Options

1. Current Open Source Landscape:
   - Limited options due to DJI's closed ecosystem
   - No direct open-source ground control solutions
   - Some community projects exist but with limitations

2. Notable Open Source Projects:
   - DJI-Reverse-Engineering (GitHub):
     - Unofficial protocol documentation
     - Experimental implementations
     - Not production-ready
     - High risk of breaking with firmware updates

   - MAVROS-DJI-BRIDGE:
     - Attempts to bridge DJI and MAVLink
     - Early development stage
     - Limited device support
     - No multispectral support

3. Challenges with Open Source Solutions:
   - No official SDK support for open platforms
   - Risk of firmware incompatibility
   - Limited access to advanced features:
     - RTK functionality
     - Multispectral camera control
     - Mission safety features
   - Potential warranty implications

4. Community Development Status:
   - Active reverse engineering efforts
   - Focus mainly on consumer DJI drones
   - Enterprise features (RTK, multispectral) not prioritized
   - No stable open-source alternative available

5. Practical Considerations:
   - Open source options not viable for professional use
   - Reliability concerns for survey operations
   - Risk of data loss or mission failure
   - Recommended to stay within DJI ecosystem

### Implementation Notes
_(To be filled as research progresses)_

### Questions and Concerns
1. RTK Integration:
   - How to ensure stable RTK fix during mission?
   - Base station setup requirements?
2. Multispectral Considerations:
   - Optimal flight parameters for spectral data collection
   - Data quality verification during mission
3. Mission Planning:
   - Maximum waypoints per mission
   - Handling of large survey areas
   - Battery life constraints 

#### Current Product Status (As of 2024)

1. DJI Terra Status:
   - Active commercial product
   - Latest version: 3.7.2 (January 2024)
   - Annual license required ($2,999/year)
   - Confirmed support for Mavic 3M Enterprise
   - Regular updates and bug fixes
   - Available at: https://www.dji.com/dji-terra

2. Waypoint 2.0 API Status:
   - Part of DJI Mobile SDK 5.x
   - Latest SDK version: v5.8.0 (December 2023)
   - Actively maintained
   - Supported Platforms:
     - iOS (11.0 or later)
     - Android (6.0 or later)
   - Documentation: https://developer.dji.com/doc/mobile-sdk-tutorial/en/

3. DJI Mobile SDK Support:
   - Official support for Mavic 3 Enterprise series
   - Enterprise SDK license required
   - Active developer forum
   - Regular SDK updates
   - Enterprise Developer Support available

4. Development Considerations:
   - Terra: Ready-to-use commercial solution
   - Waypoint 2.0: Custom development required
   - Both actively supported by DJI
   - Both receive regular updates
   - Enterprise-level support available
  