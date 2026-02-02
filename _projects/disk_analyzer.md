---
title: "Disk analyzer"
permalink: /projects/disk_analyzer
layout: single
---

![Disk](/assets/images/disk_analyzer.png)

## Overview

This is a Python project, which is designed to replicate the "Performance" page of the Task Manager app.
It displays statistics, which will be explained in the later section. The library "Rich" was used for the terminal UI.

## Statistics   

### CPU & CPU Info

- Library: psutil
- CPU Usage: Live update of CPU Usage 
- CPU Info: Specification of number of CPUs, CPU cores, Clock Speed & Uptime

### Network Speed

- Library: psutil
- Displays real-time Network Speed (Download/Upload) in KB/s

### Disk Space & System/Other Specifications

- Library: psutil, platform
- Displays disk space for the main drive (C:Drive)
- System specifications shows OS details and Python version
- Other specifications displays:
    - Battery, Last Boot Time, Memory Usage
    - Load average (number of processes running on the CPU)


