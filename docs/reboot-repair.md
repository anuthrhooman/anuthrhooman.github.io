# Root Cause Analysis: System Instability & Spontaneous Reboots

## System Specifications

| Component | Detail |
| :--- | :--- |
| **CPU** | Intel Core i7 14700K (Raptor Lake) |
| **Motherboard** | ASUSTeK COMPUTER INC. Z790 GAMING WIFI7 (LGA1700) |
| **Graphics** | EVGA NVIDIA GeForce GTX 1080 Ti (11GB) |
| **Power Supply** | Seasonic Core GX-750 80+ Gold Fully Modular |
| **Operating System** | Windows 11 |

---

## Situation
Following a custom system build, the workstation experienced intermittent spontaneous reboots. The screen would briefly display a quick error message, but the system would cut to black and restart before the text could be read. Because these restarts were sporadic and occurred in varying situations (e.g., both when idle and in use), it was impossible to capture the specific error on screen for a direct diagnosis.

## Task
Isolate the cause of the reboots by systematically testing the hardware components and firmware settings to rule out a physical part failure versus a configuration conflict.

## Action

### 1. Log Analysis
**Event Viewer:** Identified recurring Kernel-Power (Event ID 41) errors. The logs confirmed "unclean shutdowns," meaning the system was losing power or resetting at a hardware level rather than as a result of a software crash.

### 2. Hardware Testing & Investigation
* **CPU Stress Test:** I ran the Intel Processor Diagnostic Tool. The processor passed the benchmark and the computer did not restart during the high-stress test. This suggested the problem was not a defective CPU or an overheating issue.
* **GPU Power Capping:** I hypothesized that the GTX 1080 Ti might be causing power spikes that the system couldn't handle. I used the `nvidia-smi` command-line utility to manually limit the GPU's power draw. This did not stop the reboots, indicating the GPU was likely not the culprit.
* **Power Supply Testing:** I conducted a stress test using OCCT to check for PSU failure. During the test, the motherboard triggered a continuous beep alarm. I terminated the test immediately to prevent potential hardware damage. However, because the system had sustained the heavy load without a reboot until the alarm, I concluded the PSU was likely functional and looked for other potential fixes.
* **BIOS Configuration:** Further research indicated the issue might be related to voltage instability, including low-voltage events. It was suggested that changing the Global C-States would potentially prevent low-voltage events and thus prevent the reboot error from occurring. I entered the BIOS and disabled Global C-States. This setting forces the system to maintain a more consistent power level.

## Result
* **Current Status:** Since implementing the BIOS change, the system has run continuously over a significant period without experiencing a spontaneous reboot.
* **Conclusion:** While the specific error message remained elusive, the systematic process of elimination allowed me to find a functional fix in the BIOS power management settings.