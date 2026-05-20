# STM32-FreeRTOS-Queue-Based-InterTask-Communication
FreeRTOS-based STM32 project demonstrating inter-task communication using message queues with producer-consumer architecture, sensor data transfer, and SWV ITM debugging using CMSIS-RTOS V2.

---

## Overview

This project demonstrates how FreeRTOS queues can be used for safe and efficient communication between multiple tasks in a real-time embedded system.

The application consists of:

- A Producer Task (`Sensor_Read`)
- A Consumer Task (`Motion_Control`)
- A FreeRTOS Message Queue

The producer task periodically generates sensor data and sends it to the queue, while the consumer task receives and processes the data.

The project uses:

- FreeRTOS Middleware
- CMSIS-RTOS V2 API
- SWV ITM Trace Debugging

---

## Features

- FreeRTOS queue-based communication
- Producer-consumer task architecture
- Inter-task data transfer
- Thread-safe message passing
- SWV ITM console debugging
- CMSIS-RTOS V2 implementation
- Blocking queue operations

---

## Hardware Requirements

- STM32 Nucleo-F446RE Development Board
- Ultrasonic Sensor
- USB Type-A to Mini-B Cable

---

## Software Requirements

- STM32CubeIDE
- STM32CubeMX
- FreeRTOS Middleware

---

## Peripheral Configuration

| Peripheral | Configuration |
|------------|----------------|
| SYS Debug | Trace Asynchronous SW |
| Timebase Source | TIM6 |
| RTOS Interface | CMSIS_V2 |
| Queue Type | Message Queue |
| Queue Size | 16 |
| Data Type | Unsigned Integer |

---

## RTOS Tasks

| Task Name | Function |
|------------|-----------|
| Sensor_Read | Produces sensor data |
| Motion_Control | Consumes sensor data |

---

## Queue Configuration

The queue is configured to store unsigned integer values.

```c
myQueue01Handle =
    osMessageQueueNew(
        16,
        sizeof(unsigned int),
        &myQueue01_attributes);
```

- Queue Capacity: 16 items
- Data Type: `unsigned int`
- Communication Type: FIFO

---

## Working Principle

### Producer Task

The `Sensor_Read` task:

1. Generates sensor data
2. Sends data to the queue
3. Delays for 1000 ms
4. Repeats continuously

### Consumer Task

The `Motion_Control` task:

1. Waits for queue data
2. Receives sensor value
3. Prints received value
4. Processes incoming data

The consumer task remains in the Blocked state until new data becomes available.

---

## Source Code

### Producer Task

```c
void Sensor_Read(void *argument)
{
    unsigned int dist = 0;

    for(;;)
    {
        printf("Inside Data Producer Task\n");

        dist = dist + 1;

        osMessageQueuePut(
            myQueue01Handle,
            &dist,
            0,
            osWaitForever);

        osDelay(1000);
    }
}
```

---

### Consumer Task

```c
void Motion_Control(void *argument)
{
    unsigned int distance;

    for(;;)
    {
        printf("Inside Data Consumer Task\n");

        osMessageQueueGet(
            myQueue01Handle,
            &distance,
            NULL,
            osWaitForever);

        printf("Distance is %u\n", distance);
    }
}
```

---

## SWV ITM Trace Debugging

The SWV ITM Data Console is used for:

- Monitoring task execution
- Viewing producer-consumer activity
- Verifying queue communication
- Real-time embedded debugging

---

## Build and Run

1. Open the project in STM32CubeIDE
2. Configure FreeRTOS middleware
3. Create producer and consumer tasks
4. Add message queue
5. Build the project
6. Connect STM32 board via USB
7. Start debugging mode
8. Enable SWV ITM Data Console
9. Start Trace and run the application

---

## Expected Output

The SWV ITM console displays:

```text
Inside Data Producer Task
Inside Data Consumer Task
Distance is 1
Distance is 2
Distance is 3
```

The producer task continuously sends data while the consumer task receives every value successfully.

---

## Learning Outcomes

- Understanding RTOS queues
- Inter-task communication
- Producer-consumer architecture
- Thread-safe data transfer
- Blocking task behavior
- SWV ITM debugging
- CMSIS-RTOS V2 queue APIs

---

## Future Improvements

- Add real ultrasonic sensor interfacing
- Implement queue overflow handling
- Add multiple producer tasks
- Add queue timeout handling
- Integrate LCD display for sensor values

---

## Author

**Ayush Jangra**  
ECE Student | Chitkara University
