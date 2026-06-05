---
aliases:
  - Proportional, Integral, Derivative Controller
tags:
  - Growing
references:
  - Controls Engineering in FRC
---
[[Control Theory]], [[Robotics]]
2026-05-30 23:13

It is impossible to account for every possible influence on a given state. Feedback control uses measured error relative to our desired control setpoint to adjust inputs.
$$
\begin{gather}
\text{Setpoint: } r(t)\\
\text{Input: } u(t)\\
\text{Output: } y(t)\\
\text{Error: } e(t) = r(t) - y(t)
\end{gather}
$$
PID controllers are a widely used feedback control technique that uses three gains. 

The proportional term drives the position to zero similar to a spring.
$$u(t) = K_pe(t)$$
Recall Hooke's Law, $F = kx$. Where $x$ is essentially the error from the spring's equilibrium point. 

The derivative term drives velocity to zero, dampening inputs to smoothly adjust inputs to align with the setpoint. Below is a PD controller.
$$u(t) = K_pe(t) + K_d\frac{de}{dt}$$
Finally the integral term accumulates the error over time to adjust the input more aggressively, accounting for when error isn't decreasing over time (particularly to deal with drift). The full PID controller is as follows.
$$u(t) = K_pe(t) + K_i\int_0^t{e(\tau)}d\tau + K_d\frac{de}{dt}$$
We must use $\tau$ as the integration variable to distinguish it from the current time $t$. 

A system driven by a PID controller is generally underdamped, oscillating around the setpoint before settling; overdamped, which slowly approaches the setpoint; or critically damped, having the shortest rise time without overshooting or undershooting the reference.

> [!NOTE]
> Most velocity control systems don't need a D term.
>
> In addition, integrators should only be used to account for unmodelable dynamics.
>
> When a model is available, feedforward controllers can use information about the system's dynamics to quickly track a reference, letting a feedback controller compensate for unmodeled factors.