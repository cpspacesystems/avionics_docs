# CPSS OS Specification
## CPSS Core Messages
```yaml
messages:
	Gnss:
		stamp: Time
		latitude: f64
		longitude: f64
		altitude: f64
		
	Imu:
		stamp: Time
		orientation: Quaternion
		angular_velocity: Vector3
		linear_velocity: Vector3
		
	Altimeter:
		stamp: Time
		y: u32

	RadioPacket:
		data: list[u8]

	Quaternion:
		x: f64
		y: f64
		z: f64
		w: f64
		
	Vector3:
		x: f64
		y: f64
		z: f64
		
	Time:
		sec: i32
		nanosec: i32
	
```

## CPSS Component Library

```yaml
components:
	wrappers:
		gnss:
			valid-types: 
				- ublox
				- stm
			channels:
				output:
					data: Gnss
		imu:
			valid-types: 
				- ...
			channels:
				output:
					data: Imu
		altimeter:
			valid-types:
				- ...
			channels:
				output:
					data: Imu
		radio:
			valid-types:
				- lora
			channels:
				input: RadioPacket
		logger:
			valid-types:
				- flash?
				- microSD?
			channels:
				input: list[]
	logical:
		downlink:
			channels:
				input: list[pair[String, MsgType]]
				output: 
					payload: RadioPacket
		uplink:
			services:
				input:
				output:
				
```

## Using the CCL
```yaml
gnss:
	name: gnss0
	type: ublox

downlink:
	name: downlink0
	input:
		- /gnss0/data
		- /imu0/data
		- /imu1/data

input

radio:
	name: radio0
	type: lora
	input: /downlink0/payload
		
```

## Using Components in a Project
```yaml
Core Project Template:
	flight board: TOM-1.0.YAML
	processors: 
		- ESP32-1:
		  	packages: 
		  		- Telemetry:
					1+ Radio Wrapper Component:
					1+ Downlink (Publish) Logical Component:
					0+ Uplink (Service) Logical Component:
		- ESP32-2: 
			packages:
				- Perception:
					0+ IMU Wrapper Component:	
					1+ GNSS Wrapper Component:
					1+ Altimeter Wrapper Component:
		- STM32:
			packages: 
				- Mission Control:
					1 State Machine Logic Component:
						YAML file: ...
				- (Optional) Localization:
					0+ Sensor Fusion Logic Component:
					0+ State Estimation Logic Component:

				- (Optional) Mission Specific:
					1+ Mission Component:
```