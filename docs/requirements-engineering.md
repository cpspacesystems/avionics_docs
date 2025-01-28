# Flight Computer Requirements Engineering

## User Stories:
- As a project lead, I want to decide when the rocket launches ejection charges, so I can recover my rocket.
- As a project lead, I want to define the sequence of events and transition conditions for the current mission, so I can translate my planning into software.
- As a project lead, I want to customize which data I downlink, so the operator can know the status of the mission during flight.
- As a project lead, I want to connect mission specific hardware and software to the flight computer, so I can extend my rocket functionality.
- As a project lea, I want to declaratively write which hardware and 
- As a project lead, I want to test my components individually, so I know I can 

- As a mission operator, I want the realtime GPS location, so that I can find and recover the rocket.
- As a mission operator, I want the realtime current and maximum rocket speed and altitude, so I can report to competition judges and know the rocket's limits.
- As a mission operator, I want the exact location from the rocket's sensors at recovery time, so that I have a redundant measurement to report to judges.
- As a mission operator, I want to find my rocket and know my rocket's maximum height after landing without using the flight computer, so I can recover my rocket if the flight computer fails 

- As a developer, I want to construct my code in modular components, so I can reuse software and run my code on any flight computer processor I want.
- As a developer, I want to easily publish data from one component to another, so I can move data without worrying if it's running on the same computer or how I send it.
- As a developer, I want to safely store and access data within a component, so that I can communicate between threads without locks or race conditions.
- As a developer, I want access to previous flight mission data, so I can recreate previous flight trajectories and use past data for future missions.
- As a developer, I want to run mission specific code on an internal and external flight computer, so I can scale the hardware with the mission.

### Integrate into user stories:
- custom uplink/downlink commands/services (request location and max altitude at landing)
- Large file sending with custom bitrate and frequency
- Realtime video footage
- Testing EVERYTHING
    
## Functional Requirements (Ground Software):
- [Need] The system shall provide the current rocket location in plain text, with an easy method to grab and export the current location text.
- [Like] The system shall provide a live map with the rocket's location relative to the operator's location.
- [Need] The system shall provide the rocket altitude as plain text
- [Want] The system shall provide the rocket altitude as a line graph
- [Need] The system shall provide the rocket velocity as plain text
- The system shall provide the rocket velocity
	- Should this be a single vector combining directions, or should we only report vertical velocity?
	- How recent/up-to-date does the data new to be? 1s? 5s? 30s? (TODO move to Core Application)
	- Should we display time series data
- [Like] The system shall display the realtime* rocket orientation in the GUI

## Functional Requirements (Flight Software Framework):
- The system shall provide components
- describe what a component is
- The system shall provide communication between processors using a CAN bus
- The system shall provide communication within a processor using y is the Z-BUS
- The system shall provide thread safe data sharing within a component, by creating GET and SET wrappers using the Z-BUS with common data structures.

## Functional Requirements (Flight Software Core Application):
- [Like] The system shall read the mission sequence plan from a YAML file, and translate the plan to a state machine. 

## Definitions:
Project Lead -
Mission Operator - 
Downlink - 
Uplink - 
Component - A block of software that corresponds to, or 1 logical control unit (like a sensor fusion component, physical state estimation component, or airbrake controller component).
Wrapper Component -  a wrapper for 1 specific piece of hardware (like a specific IMU or a generic Servo)
Logical Component - 
Package - A Collection of components

## CPSS General Planning
Timeline:
Mid-February launch
Late February conference?

### What doesn't work:
- CAN (currently 0 voltage)
- LoRa (PCB has low impedance)
- GPS messages (impedance matching issue, 5.6nH inductor not used. We are using separate GPS connected to teensy for January launch)
- logging/flash memory (we have nothing)

### What does work
- 3 IMUs (? Should theoretically could be done, we have no code)
- 3 altimeters

### THINGS WE NEED TO START DOCUMENTING
- Everytime there's a new rocket, we need to fully document the mission plan, state diagram, timeline of events, and everything they expect avionics to do if we are functioning properly
- Monorepo for TOM board schematics, create a release for each printed board, document tickets for each board release to make sure they're improved in the next version.  Tickets can include shorts, design flaws (not enough i2c buses), tracing issues, manufacturing issues, etc.
- Monorepo for Flight OS, create a release for each version you can flash to a board and it "just works". Document tickets on new features, bugs, or anything else usually included in software tickets.
- Current project timelines, documented on a shared calendar.
- Have a document/folder for each current or future project (including TOM) to document literally anything from the project, including resources, problems developing, and anything else
- For each rocket project, have precise setup instructions for how to use

## PLAN FOR CPSS NEXT QUARTER
1.  (Before Quarter) Client study. Collect full mission plans from 1 stage rocket, 2 stage rocket, and airbrake rocket.
2. Separate common tasks from 3 missions, to create the final mission of CPSS Flight Computer.
3. FOR EACH MISSION:
	1. Write user stories. This helps us make sure our rocket meets what our clients (the aero people) need. "As a ground station operator, I want to know the rocket's GPS location, so I can find it when it lands."
	2. Write functional requirements. This helps us understand what the system needs to do to accomplish the user stories. "The system shall read from 3 IMU sensors, then write all 3 sensor values to flash storage.". Immediately turn functional requirements into GitHub issues
	3. Write use cases, create component diagram circling components that meet a use case. This will help us keep in mind what's actually the important part in what we're developing. (IMPORTANT. Displaying GPS data is a use case, supplying a voltage is not. Sorry.)
4. Have short lesson on software development cycle. If you want to work on something, either create a new issue or choose an existing issue. Branch from main with your issue. If you create a branch that's not linked to an issue, something went wrong. To merge, pull from main, create a PR, make sure it's reviewed by the product owner of that project, then merge and delete your branch.

## Other Project info
[Requirements.xlsx](https://cpslo.sharepoint.com/:x:/s/CalPolySpaceSystems/EYvOk4mcNM5IuLaI1rCtd-sBRekzS7ULWpzczZCWQ3wvFg?e=R9k6D9)
Ryan James Fukui - Ok here is the project requirements document that our systems lead, Tarzan has been working on. The most pertinent requirement for avionics is 5 as that has all the data and telemetry we are expecting. This doc is still a work-in progress but I can make it a priority for the systems team and will let y'all know when these updates are made. The main things missing are 1. the difference between the stacks and 2. the separation mechanism. 3. recovery requirements
1.  The upper stage stack includes everything listed in requirement 5 and the lower stage stack does not have video.  
2. The separation mechanism currently uses the same ematches as recovery and will have 4 (subject to change)  (two placed on each side for redundancy).  
3. recovery in both stages is the exact same as last year, documentation of what that entails will be added as a subreq under requirement 5