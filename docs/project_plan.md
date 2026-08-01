AeroVision Ground Control Software
Version 1 — Aerospace Telemetry Logging and Visualization Platform
1. Project Overview
Purpose

AeroVision is a containerized aerospace telemetry monitoring and logging platform designed to simulate an aircraft environment, collect real-time flight data, process telemetry, store flight information, and provide a web-based Ground Control Software (GCS) interface.

The system will interface with a simulated aircraft running through PX4 SITL and Gazebo. The aircraft telemetry will be transmitted through MAVLink to a custom C++ telemetry processing service. The processed information will be stored, analyzed, and displayed through a React-based ground control interface.

The project demonstrates aerospace software development practices including:

Modern C++
Object-oriented design
Memory management
Networking
Database systems
Web application development
Containerization
CI/CD automation
Software engineering workflows
2. Version 1 Objective

The goal of Version 1 is:

Build a functional aerospace telemetry logging system capable of receiving simulated aircraft telemetry, storing flight data, and displaying real-time aircraft information through a Ground Control Software interface.

Version 1 will focus on:

Telemetry collection
Data processing
Data storage
Real-time visualization
Flight logging

Future versions may include:

Mission planning
Autonomous commands
Fault detection
Flight replay
Hardware integration
3. Overall System Architecture
                    Developer
                        |
                   Git Repository
                        |
        --------------------------------
        |                              |
     Jenkins                    Azure DevOps
        |                              |
        |                              |
        -------- CI/CD Pipeline --------
                        |
                        |
                Docker / Podman
                        |
=================================================

                Gazebo Simulation

                       |
                       |
                 PX4 SITL Aircraft

                       |
                       |
                    MAVLink

                       |
                       |

              C++ Telemetry Service

        |             |              |
        |             |              |

   PostgreSQL     REST API     WebSocket

                                     |
                                     |

                         React Ground Control UI


                       |
                       |

                  Python Analytics

                       |

                  Streamlit Dashboard

=================================================
4. Component Breakdown
Component 1 — PX4 + Gazebo Simulation
Purpose

Provide a simulated aerospace environment that generates realistic aircraft telemetry.

Responsibilities

The simulation environment will:

Simulate aircraft movement
Generate sensor information
Provide flight dynamics
Publish MAVLink telemetry messages
Technologies
PX4 Autopilot
Gazebo Simulator
MAVLink Protocol
Initial Features

The simulated aircraft should provide:

Position Data
Latitude
Longitude
Altitude
Velocity Data
Ground speed
Airspeed
Vertical speed
Orientation
Roll
Pitch
Yaw
System Data
Battery level
GPS status
Flight mode
Connection status
Development Tasks
Install PX4 development environment
Install Gazebo simulator
Launch PX4 SITL
Spawn aircraft model
Verify MAVLink telemetry output
Connect using QGroundControl temporarily for validation
Completion Criteria

The aircraft successfully flies in simulation and publishes telemetry.

Component 2 — C++ Telemetry Service
Purpose

Create the main backend software responsible for receiving, processing, and distributing aircraft telemetry.

This component represents the aerospace software layer between the aircraft and the operator interface.

Technology
C++20
CMake
MAVLink/MAVSDK
Multithreading
Internal Architecture
Telemetry Service

        |
        |
Telemetry Manager

        |
--------------------------------

MAVLink Handler

State Manager

Logger

Database Manager

API Server

WebSocket Server

--------------------------------
Required Classes
TelemetryManager

Responsible for:

Receiving MAVLink packets
Decoding messages
Updating aircraft state

Example data:

Altitude
Speed
Position
Battery
Orientation
AircraftState

Stores current aircraft condition.

Example:

AircraftState

position

velocity

orientation

battery

gpsStatus

flightMode
Logger

Responsible for:

Recording system events
Tracking connections
Recording warnings

Examples:

Aircraft Connected

Telemetry Started

Battery Low

Connection Lost
FlightRecorder

Responsible for:

Saving telemetry samples
Timestamping data
Creating flight logs
DatabaseManager

Responsible for:

Connecting to PostgreSQL
Saving telemetry
Retrieving flight history
API Server

Responsible for:

Providing telemetry to external applications.

Examples:

GET /aircraft/status

GET /telemetry/latest

GET /flights
WebSocket Server

Responsible for:

Real-time telemetry streaming.

Example:

Aircraft Update

↓

WebSocket

↓

React Dashboard
C++ Learning Objectives

The implementation should demonstrate:

Object-Oriented Programming

Use:

Classes
Encapsulation
Interfaces
Inheritance where appropriate
Memory Management

Demonstrate:

Stack allocation
References
unique_ptr
shared_ptr
RAII
Multithreading

Separate:

Thread 1:
MAVLink reception

Thread 2:
Database logging

Thread 3:
WebSocket updates

Component 3 — PostgreSQL Database
Purpose

Store historical flight telemetry.

Database Tables
Aircraft

Stores aircraft information.

Example:

aircraft_id

name

type
Flights

Stores flight sessions.

Example:

flight_id

aircraft_id

start_time

end_time

status
Telemetry

Stores flight samples.

Example:

timestamp

latitude

longitude

altitude

speed

heading

battery
Events

Stores important events.

Example:

timestamp

event_type

description

severity
Component 4 — React Ground Control Software
Purpose

Create the operator interface.

Technology
React
TypeScript
WebSockets
REST APIs
Features
Dashboard View

Display:

Aircraft status
Connection state
Flight mode
Battery
GPS status
Live Telemetry Panel

Display:

Altitude
Speed
Position
Orientation
Battery
Map View

Display:

Current aircraft location
Flight path
Graph View

Display:

Real-time graphs:

Altitude
Speed
Battery
Event Log

Display:

10:20:15

Aircraft Connected

10:21:00

Takeoff

10:30:22

Landing
Flight History

Display:

Previous flights.

Allow:

Viewing telemetry
Reviewing statistics
Component 5 — Python Analytics
Purpose

Analyze stored flight data.

Features

Generate:

Flight statistics
Maximum altitude
Maximum speed
Battery usage
Flight duration

Technologies:

Python
Pandas
NumPy
Component 6 — Streamlit Engineering Dashboard
Purpose

Provide engineering analysis tools.

Features:

Upload flight logs.

Display:

Charts
Statistics
Trends
Flight comparison
Component 7 — Containerization
Purpose

Package the entire system.

Containers:

cpp-telemetry-service

postgres-database

react-ground-control

python-analytics

streamlit-dashboard
Docker Compose

One command should start the entire system:

docker compose up
Component 8 — Jenkins CI/CD
Purpose

Automate software validation.

Pipeline:

Developer Push

↓

Checkout Code

↓

Build C++

↓

Run Unit Tests

↓

Static Analysis

↓

Build React

↓

Build Docker Images

↓

Integration Testing

↓

Package Release
Component 9 — Azure DevOps
Purpose

Manage the software lifecycle.

Use:

Azure Boards

Track:

Features
Tasks
Bugs
Development progress

Example:

Epic:
Ground Control System

Feature:
Telemetry Service

Tasks:
Create MAVLink Parser

Create Database Logger

Create API
Azure Pipelines

Create automated builds similar to Jenkins.

Development Roadmap
Phase 1 — Repository Setup

Tasks:

Create Git repository
Create documentation
Setup branching strategy
Create Azure project

Deliverable:

Professional project structure.

Phase 2 — Simulation Environment

Tasks:

Install PX4
Install Gazebo
Launch aircraft
Verify MAVLink

Deliverable:

Aircraft simulation working.

Phase 3 — C++ Telemetry Service

Tasks:

Create C++ project
Setup CMake
Connect MAVLink
Parse telemetry
Create aircraft state model

Deliverable:

Telemetry appears in console.

Phase 4 — Database Integration

Tasks:

Setup PostgreSQL
Create schema
Store telemetry

Deliverable:

Flight data saved.

Phase 5 — API Development

Tasks:

Create REST API
Create WebSocket server

Deliverable:

External applications can access telemetry.

Phase 6 — React Ground Control Software

Tasks:

Create React project
Connect APIs
Create dashboard
Add visualization

Deliverable:

Live aircraft monitoring interface.

Phase 7 — Analytics

Tasks:

Build Python analysis tools
Build Streamlit dashboard

Deliverable:

Flight analysis capability.

Phase 8 — Containerization

Tasks:

Create Dockerfiles
Create Docker Compose
Test full deployment

Deliverable:

Entire system runs with containers.

Phase 9 — CI/CD

Tasks:

Create Jenkins pipeline
Create Azure pipeline
Automate testing
Automate builds

Deliverable:

Professional software delivery workflow.

Final Version 1 Result

AeroVision will be able to:

✅ Simulate an aircraft
✅ Receive aerospace telemetry
✅ Process telemetry using C++
✅ Store flight data
✅ Display live information
✅ Record flights
✅ Analyze flight history
✅ Run fully inside containers
✅ Automatically build and test through CI/CD pipelines
