# DJI Terra API Exploration

## 1. API Overview

### Core Functionality
- Mission planning and execution
- 2D/3D mapping capabilities
- RTK integration
- Multispectral data processing
- Terrain following

### API Access Levels
- Basic License
- Pro License
- Enterprise License

## 2. Mission Planning API

### a) Waypoint Configuration
- Maximum waypoints: 65,535 per route
- Coordinate system support:
  - WGS84
  - Local coordinate systems
  - Custom coordinate systems
- Altitude reference options:
  - Relative to takeoff point
  - Above sea level (ASL)
  - Relative to ground (RTK required)

### b) Flight Parameters
- Speed control:
  - Global mission speed
  - Individual waypoint speeds
  - Corner turning speed
- Altitude constraints:
  - Minimum safe altitude
  - Maximum altitude limits
  - Terrain following offset

### c) Camera Control
- Trigger modes:
  - Time interval
  - Distance interval
  - Waypoint-based
  - Smart oblique capture
- Multispectral settings:
  - Band selection
  - Exposure settings per band
  - Calibration parameters

### c) CSV Waypoint Import
- Supported CSV format:
  - Latitude (decimal degrees)
  - Longitude (decimal degrees)
  - Altitude (meters)
  - Optional parameters:
    - Speed (m/s)
    - Gimbal pitch
    - Camera actions
    - Heading

- CSV Requirements:
  - UTF-8 encoding
  - Comma-separated values
  - Header row required
  - Decimal point for coordinates (not comma)
  - Example format:
    ```csv
    latitude,longitude,altitude,speed,gimbal_pitch
    37.123456,-122.123456,100,5,-90
    37.123457,-122.123457,100,5,-90
    ```

- Import Process:
  1. Create new mission in Terra
  2. Select "Import Waypoints"
  3. Choose CSV file
  4. Map CSV columns to waypoint parameters
  5. Validate waypoint data
  6. Adjust mission parameters if needed

- Post-Import Editing:
  - Waypoints can be modified after import
  - Additional waypoints can be added
  - Flight parameters can be adjusted
  - Camera settings can be configured

## 3. RTK Integration

### a) RTK Sources
- D-RTK 2 Mobile Station
- NTRIP network support
- Custom base station integration
- RTK positioning accuracy: ±2cm+1ppm (horizontal)

### b) RTK Data Management
- Real-time correction data
- Raw observation data logging
- Base station coordinates management
- RTK status monitoring

## 4. Data Processing API

### a) Real-time Processing
- Image quality assessment
- RTK position tagging
- Multispectral band alignment
- Progress monitoring

### b) Post-Processing
- Orthomosaic generation
- Digital surface models (DSM)
- Point cloud generation
- Multispectral index calculations

## 5. Export Capabilities

### a) Mission Data
- Flight paths (KML, SHP)
- Waypoint lists (CSV, JSON)
- Flight logs
- RTK observation data

### b) Processed Data
- Orthomosaics (TIFF, JPG)
- Digital elevation models
- Multispectral indices
- Point clouds (LAS/LAZ)

## 6. Integration Options

### a) File-based Integration
- Import formats:
  - KML/KMZ
  - Shapefiles
  - GeoJSON
  - Custom boundary files
- Export formats:
  - Standard GIS formats
  - CAD formats
  - Custom data formats

### b) API Integration
- REST API endpoints
- WebSocket connections
- Custom data pipelines
- Third-party software integration

## 7. Development Requirements

### a) System Requirements
- Windows 10/11 64-bit
- 16GB RAM minimum
- NVIDIA graphics card
- Internet connection for licensing

### b) Development Tools
- Terra SDK
- API documentation
- Sample code
- Development license

## 8. Best Practices

### a) Mission Planning
- Optimal waypoint spacing
- Battery life considerations
- RTK fix maintenance
- Failsafe configuration

### b) Data Collection
- Ground sampling distance (GSD)
- Image overlap recommendations
- Multispectral calibration
- Weather considerations

## 9. Known Limitations

### a) Technical Limitations
- Maximum project size
- Processing capabilities
- Real-time monitoring constraints
- Network bandwidth requirements

### b) Operational Limitations
- Weather restrictions
- RTK coverage requirements
- Battery life constraints
- Data storage limitations

## 10. Support and Resources

### a) Official Support
- DJI Enterprise Support
- Documentation portal
- API reference guides
- Tutorial resources

### b) Community Resources
- User forums
- Knowledge base
- Sample projects
- Best practice guides

## 11. Licensing and Costs

### a) License Types
- Basic: Limited functionality
- Pro: Full mapping capabilities
- Enterprise: Advanced features + API access

### b) Cost Structure
- Annual subscription fees
- Per-seat licensing
- Enterprise volume licensing
- Support package options

## Implementation Notes

### Python Integration with Terra API

#### 1. API Setup
- DJI Terra provides a REST API accessible via Python
- Authentication required:
  ```python
  import requests
  
  TERRA_API_KEY = "your_enterprise_api_key"
  TERRA_BASE_URL = "https://api.terra.dji.com/api/v1"
  
  headers = {
      "Authorization": f"Bearer {TERRA_API_KEY}",
      "Content-Type": "application/json"
  }
  ```

#### 2. Large Waypoint Mission Handling
- For 5000 points, recommended approach:
  ```python
  import pandas as pd
  import json
  
  def create_terra_mission(csv_path):
      # Read waypoints from CSV
      df = pd.read_csv(csv_path)
      
      # Create mission configuration
      mission_config = {
          "name": "Large Survey Mission",
          "drone_type": "MAVIC_3_ENTERPRISE",
          "waypoints": []
      }
      
      # Convert waypoints to Terra format
      for _, row in df.iterrows():
          waypoint = {
              "latitude": row['latitude'],
              "longitude": row['longitude'],
              "altitude": row['altitude'],
              "speed": row.get('speed', 5),  # default speed 5 m/s
              "gimbal_pitch": row.get('gimbal_pitch', -90)
          }
          mission_config['waypoints'].append(waypoint)
          
      return mission_config
  ```

#### 3. Mission Upload Process
```python
def upload_mission_to_terra(mission_config):
    # Create new mission
    response = requests.post(
        f"{TERRA_BASE_URL}/missions",
        headers=headers,
        json=mission_config
    )
    
    mission_id = response.json()['mission_id']
    return mission_id
```

#### 4. Export to RC Pro Controller
```python
def export_to_rc_pro(mission_id):
    # Generate RC Pro compatible format
    response = requests.post(
        f"{TERRA_BASE_URL}/missions/{mission_id}/export",
        headers=headers,
        json={"format": "rcpro"}
    )
    
    # Get download URL
    download_url = response.json()['download_url']
    
    # Download the mission file
    mission_data = requests.get(download_url)
    
    # Save to local file
    with open('mission.rcpro', 'wb') as f:
        f.write(mission_data.content)
```

#### 5. Complete Implementation Example
```python
def process_large_mission(csv_path):
    try:
        # Create mission from CSV
        mission_config = create_terra_mission(csv_path)
        
        # Upload to Terra
        mission_id = upload_mission_to_terra(mission_config)
        
        # Export for RC Pro
        export_to_rc_pro(mission_id)
        
        print("Mission successfully created and exported")
        
    except Exception as e:
        print(f"Error processing mission: {str(e)}")

# Usage
process_large_mission('waypoints.csv')
```

#### 6. Important Considerations
- Mission Chunking:
  - Terra automatically handles large missions
  - No need to manually split 5000 points
  - Verify battery requirements for mission length

- Data Validation:
  - Validate coordinate bounds
  - Check altitude restrictions
  - Verify speed parameters
  - Ensure RTK requirements are met

- RC Pro Transfer:
  - Mission file can be transferred via:
    - USB drive
    - DJI Terra mobile app
    - Direct API connection to RC Pro

#### 7. Error Handling
```python
def validate_waypoints(df):
    # Example validation checks
    if len(df) > 65535:
        raise ValueError("Exceeds maximum waypoint limit")
        
    if df['altitude'].max() > 500:  # meters
        raise ValueError("Altitude exceeds maximum limit")
        
    # Add more validation as needed
```

#### 8. Mission Status Monitoring
```python
def check_mission_status(mission_id):
    response = requests.get(
        f"{TERRA_BASE_URL}/missions/{mission_id}/status",
        headers=headers
    )
    return response.json()['status']
```

Note: The exact API endpoints and parameters may vary based on your Terra Enterprise license and API version. Consult the latest DJI Terra API documentation for the most current information.

## Questions and Issues
_(To be updated during development)_ 

### c) DJI RC Pro and Terra Connectivity

#### Direct Connection
- The DJI RC Pro does NOT connect directly to Terra
- Terra is a desktop-based planning software
- Mission transfer workflow:
  1. Create mission in Terra (desktop)
  2. Export mission file
  3. Transfer to RC Pro via:
     - USB drive (recommended for large missions)
     - DJI Pilot 2 app sync
     - Manual import through RC Pro

#### DJI Pilot 2 Integration
- DJI Pilot 2 is the app used on RC Pro
- Workflow options:
  1. USB Transfer Method:
     ```plaintext
     Terra (Desktop) → Export → USB Drive → RC Pro → DJI Pilot 2
     ```
  2. Cloud Sync Method (requires DJI FlightHub Enterprise):
     ```plaintext
     Terra (Desktop) → DJI Cloud → DJI Pilot 2 on RC Pro
     ```

#### Important Notes
- No direct API connection between Terra and RC Pro
- Mission files must be manually transferred
- Large missions (5000+ waypoints) recommended to use USB transfer
- DJI Pilot 2 must be used to execute the mission on RC Pro
- Ensure Terra and DJI Pilot 2 versions are compatible

### d) DJI Cloud Integration

#### DJI FlightHub 2 Overview
- Enterprise cloud platform for DJI fleet management
- Enables real-time mission syncing
- Subscription-based service
- Required for cloud-based workflows

#### Key Features
- Mission Syncing:
  ```plaintext
  Terra → FlightHub 2 → DJI Pilot 2
  ```
- Real-time Data:
  - Live drone telemetry
  - RTK corrections distribution
  - Mission progress tracking
  - Image/data upload during flight

#### Cloud API Access
```python
# FlightHub 2 API Authentication
FLIGHTHUB_API_KEY = "your_flighthub_api_key"
FLIGHTHUB_URL = "https://api.flighthub.dji.com/api/v1"

headers = {
    "Authorization": f"Bearer {FLIGHTHUB_API_KEY}",
    "Content-Type": "application/json"
}

# Example: Sync mission to cloud
def sync_to_flighthub(mission_id):
    response = requests.post(
        f"{FLIGHTHUB_URL}/missions/sync",
        headers=headers,
        json={"terra_mission_id": mission_id}
    )
    return response.json()
```

#### Storage and Bandwidth
- Cloud Storage Tiers:
  - Basic: 10GB/month
  - Pro: 1TB/month
  - Enterprise: Custom storage limits
- Bandwidth Considerations:
  - Mission files (~1MB per 1000 waypoints)
  - Real-time image upload (varies by resolution)
  - Telemetry data (~100KB/minute)

#### Security Features
- End-to-end encryption
- Role-based access control
- Data residency options
- Audit logging
- Compliance certifications:
  - ISO 27001
  - SOC 2
  - GDPR compliance (EU regions)

#### Integration Requirements
- FlightHub 2 Enterprise subscription
- Compatible hardware:
  - DJI RC Pro
  - Mavic 3 Enterprise
  - 4G/5G dongle (recommended)
- Software versions:
  - Latest DJI Terra
  - DJI Pilot 2 v4.0+
  - FlightHub 2 v2.0+

#### Best Practices
- Network Requirements:
  - Minimum 5Mbps upload speed
  - Stable internet connection
  - VPN support if required
- Mission Sync:
  - Pre-sync large missions before field deployment
  - Use USB backup for critical missions
  - Verify sync status before flight
- Data Management:
  - Regular cloud storage cleanup
  - Local backup of critical missions
  - Monitor bandwidth usage

#### Limitations
- Internet connectivity required
- Additional subscription cost
- Data transfer speeds dependent on connection
- Some features require enterprise license
- Regional availability may vary