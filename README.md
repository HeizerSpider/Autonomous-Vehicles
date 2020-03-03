# Autonomous Vehicles


#### Taxonomy of driving:

Driving Task  
    - Perceiving the environment  
    - Motion Planning (From point A to point B)  
    - Vehicle Control: 
        - Acceleration/velocity, Brake - Longitudinal Control  
        - Steering - Lateral Control  
        - Object and Event Detection Response (OEDR) - Detection, Reaction  
        - Planning (Long/Short term)  
        - Miscellaneos (Signals etc)  

Automated Capabilities:
    - Automated Lateral/longitudinal control  
    - Degree of OEDR  
    - Automatic Emergency Response (System alone or Driver supervision required)  
    - System perform in all scenarios? Or is it limited?  

Operational Design Domain (ODD)  
    - Operating conditions (Night/Day, Road conditions etc.)  

Classification of Driving System Automation   
    - Factors: Driver attention/ Driver action (intervention)/ Driving Task


#### SAE Standard:
    - Level 0: No driving Automation (100% Driver)    
    - Level 1: Assisted Longitudinal OR Lateral Control (Adaptive Cruise Control: Longitudinal Control/ Lane Keeping Assistance: Lateral Control)    
    - Level 2: Partial Driving Automation (BOTH Lateral and Longitudinal Control) - Specific Scenarios (GM Super Cruise, Nissan Pro Pilot Assist - Driver monitoring of system is required)    
    - Level 3: Conditional Driving Automation = Level 2 + OEDR - Requires Driver monitoring. System can alert driver to take over in time to intervene  
    (Audi A8 luxury sedan)     
    - Level 4: High Driving Automation: System capable of handling minimum risk condition, Limited ODD (Can handle emergencies on its own, will still ask drivers to take over in higher risk conditons- does not require full driver monitoring)  
    (Waymo)  
    - Level 5: High Driving Automation - Unlimited ODD  


#### Perception: 
- Perceive driving condtions --> React
- What is perception? Make snese of environment and ourselves
- Identification and understanding object's motion (Prediction of trajectory to react accordingly)

1) Static: Road and Lane markings, Curbs, Road Signs, Traffic Lights, Road Construction(On/Off-Road)
2) Dynamic: Other cars, motorcycles, Bicycles(On-Road), Pedestrian movements (more erratic) (Off-Road)
3) Ego Localization (Estimate where we are and how we are moving at any point in time) -position to make informed and safe driving decisions
(What is used? GPS, IMU, Odometry)

- Challenges:
Detection/Segmentation: Performance of ML to achieve human level perception (can it ever reach such a standard?)  
Not immune to sensor uncertainty (Noise, Weather)  
Occlusion/ Reflection/ Lens Flare for camera sensor (Same, sensor data) - use radar to overcome such problems

#### Decision Making
