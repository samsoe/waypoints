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