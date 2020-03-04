# Autonomous Vehicles


## Taxonomy of driving:

- Driving Task  
    - Perceiving the environment  
    - Motion Planning (From point A to point B)  
    - Vehicle Control: 
        - Acceleration/velocity, Brake - Longitudinal Control  
        - Steering - Lateral Control  
        - Object and Event Detection Response (OEDR) - Detection, Reaction  
        - Planning (Long/Short term)  
        - Miscellaneos (Signals etc)  

- Automated Capabilities:  
    - Automated Lateral/longitudinal control  
    - Degree of OEDR  
    - Automatic Emergency Response (System alone or Driver supervision required)  
    - System perform in all scenarios? Or is it limited?  

- Operational Design Domain (ODD)  
    - Operating conditions (Night/Day, Road conditions etc.)  

- Classification of Driving System Automation   
    - Factors: Driver attention/ Driver action (intervention)/ Driving Task


### SAE Standard:
    - Level 0: No driving Automation (100% Driver)    
    - Level 1: Assisted Longitudinal OR Lateral Control (Adaptive Cruise Control: Longitudinal Control/ Lane Keeping Assistance: Lateral Control)    
    - Level 2: Partial Driving Automation (BOTH Lateral and Longitudinal Control) - Specific Scenarios (GM Super Cruise, Nissan Pro Pilot Assist - Driver monitoring of system is required)    
    - Level 3: Conditional Driving Automation = Level 2 + OEDR - Requires Driver monitoring. System can alert driver to take over in time to intervene  
    (Audi A8 luxury sedan)     
    - Level 4: High Driving Automation: System capable of handling minimum risk condition, Limited ODD (Can handle emergencies on its own, will still ask drivers to take over in higher risk conditons- does not require full driver monitoring)  
    (Waymo)  
    - Level 5: High Driving Automation - Unlimited ODD  


### Perception: 
- Perceive driving condtions --> React
- What is perception? Make sense of environment and ourselves
- Identification and understanding object's motion (Prediction of trajectory to react accordingly)

1) Static: Road and Lane markings, Curbs, Road Signs, Traffic Lights, Road Construction(On/Off-Road)
2) Dynamic: Other cars, motorcycles, Bicycles(On-Road), Pedestrian movements (more erratic) (Off-Road)
3) Ego Localization (Estimate where we are and how we are moving at any point in time) -position to make informed and safe driving decisions
(What is used? GPS, IMU, Odometry)

- Challenges:
Detection/Segmentation: Performance of ML to achieve human level perception (can it ever reach such a standard?)  
Not immune to sensor uncertainty (Noise, Weather)  
Occlusion/ Reflection/ Lens Flare for camera sensor (Same, sensor data) - use radar to overcome such problems

### Decision Making in Self Driving Car Systems
- Planning
    - Long term planning 
        - Navigation for the entire driving task
        - eg. How to get from Home to work
    - Short term planning 
        -Changing lanes, passing an intersection followed by joining another road
    - Immediate 
        - Following a lane
        - Acceleration/ braking

- Structure to represent Decision making in software (And types of Planning):
    - Reactive planning
        - Design a set of rules that take into account the current state of ego and other objects and give decisions (not future predictions) eg. If there is a pedestrian on road, stop OR if speed limit changes, slow down

    - Predictive planning (Predominant method, expandable)
        - Make predicitons about other vehicles and how they are moving, using predictions to inform our decisions (eg. That car has been stopped for the last 10 seconds, it will continue to remain stationary, move past OR pedestrian is jaywalking, will be in my lane in the next few seconds, slow down)
        - A more human reaction

--------------------------------------------------------------------------------
## Hardware and Software

### Sensors and Computing Hardware

- Sensors for Perception 
    - Device that measures or detects a property of the environment (changes to a property)

    - Types: 
        - Exteroceptive: Record a property of the surrounding environment
            - a) Camera: Essential for correctly perceiving environment      
                - Resolution of camera (no. of pixels that create the image)
                - Field of View (horizontal and vertical angular extent visible to the camera- based on lens) - FOV and Resolution tradeoff (both must go up to maintain quality of image)
                - Dynamic range (Difference between the lightest and darkest tones of a camera - to counter the difference in lighting throughout the day)
            - b) Stereo camera (combination of 2 cameras with overlapping field of view and aligned image plane): enables depth estimation (using a disparity map)
            - c) Lidar (Light Detection and Ranging): Reflective return of light beams measured (amt of returned light and time of flight of beam gives intensity and range of object detected)
                - Comparison metrics: No. of beams, Points per second, Rotation Rate, Field of View
                - To look further into: Solid state Lidar
            - d) Radar (Radio detection and Ranging): Robustly detect large objects
                - Comparison metrics: Detection Range, Field of View, Position and Speed Measurement Accuracy
                - Wide angular FOV, short range OR Narrow FOV, longer range
            - e) Sonar (Sound navigation and Ranging): Short range, all-weather distance measurement (good for low-cost parking solutions)
                - Comnaprison metircs: Range, Field of View, Cost

        - Proprioceptive: Records a property of the Ego Vehicle
            - a) Global Navigation Satellite Systems (GNSS) & Inertial Measurement Unit (IMU): Measures Vehicle Position and Velocity
                - Acuracy depends on positioning methods and corrections used
                - IMU also calculates: Angular Rotation Rate, Acceleration (Combined measurements can be used to estimate the 3D orientation of the vehicle, Heading most impt for vehicle control)
            - b) Vehicle Odometry: Rates of rotation, Wheel Velocity, Orientation and calculate overall speed and orientation of the car (Also tracks mileage of car in normal situations)

            <img src="/images/car_overview.png" width="400">

- Computing platforms (Central Computer)
        - All sensors feeding information to the Central Computer: 
        - Takes in all sensor data and computes actions (Mostly proprietary to match specific softwares and algorithms) OR common examples: Nvidia Drive Px AND Intel Mobileye EyeQ
        - Needs Serial and Parallel compute modules for Lidar and Image Processing: Segmentation, Object Detection and Mapping   
        (Employ GPUs, Field Programmable Gate Arrays (FPGAs) and Application Specific Integrated Chip(ASICs)) 
        - eg. Drive Px contains multiple GPUs, EyeQ has FPGAs to accelerate parallelizable computes (image processsing or neural network inference)
        - Synchronization of modules is important, as well as to have a common clock (for reference) - time stamping

- Basics of Designing hardware configurations
    - Setting of assumptions: 
        - Aggresive Deceleration: >= 5m/s^2
        - Normal/Comfy Deceleration: ~2m/s^2
        - Stopping distance: d = v^2/(2a)  (can consider other factors such as computing speed and road conditions etc)
    -Sensor placement needs to support maneuvers within ODD

    - Sensor coverage for different scenarios: eg. Highway driving and urban driving

    ||Highway|Urban/Residential|
    |-|------|-----------------|
    |Traffic Speed|High|Low-Medium|
    |Traffic Volume|High|Medium-High|
    |# of lanes|SG 3-5|2-4|
    |Other features|Fewer curves, exits and merges|Many turns and intersections|

    - Highway Analysis:
        - Emergency stop: 
            - Blockage ahead, to stop in time (Longitudinal coverage- speed of 120km/h stopping distance of 110m --> Aggresive deceleration needed)- Sensing ranges of 150-200m 
        - Change lanes or cars merging into our lane: 
            - Sense adjacent lanes to avoid a hard stop (width of lane to be taken into consideration) 
            - Field of view is important as well to track adjacecnt lanes
        - Maintain speed: 
            - Sense vehicle in own lane (Relative position and speed of front vehicle are important to maintain safe following distance ~100m in front can measure their deceleration as well) 
            - Both vehicles moving hence no need to look as far as emergency stop scenario (Calculating reaction time/distance)
            <img src="/images/coverage_highways.png" width="400">

    - Urban Driving Analysis:
        - Similar 3 maneuvers as the Highway analysis but since car is at a slower speed, there is no need for the same extent of long range sensing
        - Overtaking:
            - Longitudinal Coverage: Need to sense parked car and look for oncoming traffic (Wide short range sensor for parked car, narrow long range for oncoming traffic)
            - Lateral Coverage (same as highway- lookout for merging vehicles)
        - Turning, crossing at intersections:
            - Near Omnidirectional Sensors for all kind of movements (approacing vehicles, nearby pedestrians, doing turns)
        - Roundabouts:
            - Lateral Coverage:  Slow vehicles, limited range required
            - Longitudinal Coverage: Wider field of view due to shape of roundabout
        <img src="/images/coverage_urban.png" width="400">
        Highway case almost entirely covered

    - Overall coverage and design loopholes/flaws (blind spots)
        - Choice of sensors should be decided based on the maneuvers that we want to execute
        - Include both long range for longitudinal dangers
        - Wide FOV for omnidirectional perception
        - Final choice of sensors also depends on requirements for operating conditions, sensor redundancy due to failures and budget (no one size fits all)

    <img src="/images/coverage_overall.png" width="500">


- Components of typical Autonomy Software Stack

- Ways of representing the environment for self-driving


