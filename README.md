<div align="center">

# 🎮 NX809J-CPU-Cluster-Lock

**Smart 2-Stage Thermal & Frequency Clamp for Nubia RedMagic 11 Pro (NX809J)**

![Device](https://img.shields.io/badge/Device-RedMagic%2011%20Pro%20(NX809J)-red)
![Android](https://img.shields.io/badge/Android-16-brightgreen)
![Root](https://img.shields.io/badge/Root-Magisk%20%7C%20KernelSU-blue)
![Version](https://img.shields.io/badge/Version-v3.5-orange)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

</div>

---

## Overview

`NX809J-CPU-Cluster-Lock` prevents aggressive vendor thermal throttling and sawtooth ("V-dip") clock drops under heavy gaming or benchmark loads on the **Nubia RedMagic 11 Pro (NX809J)**.

Instead of releasing cooling devices completely to vendor control (which causes sudden 1.44 GHz drops), it applies a **2-stage smooth clamp** across both small (`policy0`) and big (`policy6`) CPU clusters right below the hardware trip limit.

## Key Features

- **Dynamic Cluster Discovery:** Automatically detects all `cpu-cluster*` cooling nodes (`cooling_device3/4/5/13/14/31/32`, etc.) across reboots.
- **Static Trip Filtering:** Ignores static hardware trip zones (`*trip*`, `*limit*`, `*max*`) so only live SoC/CPU temperatures drive state transitions.
- **2-Stage Smooth Thermal Clamp (`v3.5`):**
  - **`<= 100°C` (`LOCKED`):** Locks `cur_state=0` and holds `policy0` at **3.19 GHz** (`3187200`) & `policy6` at **3.65 GHz** (`3648000`).
  - **`101–102°C` (`SOFT1_CLAMP`):** Applies `cur_state=2` and steps down `policy0` / `policy6` slightly to slow heat buildup.
  - **`103–105°C` (`SOFT2_CLAMP`):** Applies `cur_state=4` and steps `policy0` down 3 frequency levels to stabilize temperature without hitting hardware trip.
  - **`>= 106°C` (`FULL_RELEASE`):** Temporarily unlocks cooling devices as a final safety fallback; re-locks automatically at **`<= 98°C`**.
- **Live Configuration (`config.conf`):** Adjust temperature thresholds and target frequencies without rebooting.

## Configuration (`config.conf`)

```ini
P0_VAL=3187200
P6_VAL=3648000
P0_SOFT1=auto
P0_SOFT2=auto
SOFT1_TEMP=101
SOFT1_STATE=2
SOFT2_TEMP=103
SOFT2_STATE=4
FULL_RELEASE_TEMP=106
RELOCK_TEMP=98
