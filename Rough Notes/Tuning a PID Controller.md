---
aliases:
  - PID Tuning
tags:
  - Seed
references:
  - Controls Engineering in FRC
---
[[Control Theory]], [[Robotics]]
2026-05-30 23:52

1. Set $K_p$, $K_i$, and $K_d$ to zero.
2. Increase $K_p$ until the output oscillates around the setpoint.
3. Increase $K_d$ as much as possible without introducing jittering.
4. 