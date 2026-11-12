# Intelligent CPU Cooling

**Towards Practical Intelligent CPU Cooling: Simulation-Driven Adaptive Fan Control with State-Machine Regularized Online RL**

*Train in simulation. Learn in shadow mode. Let the state machine decide when RL takes control.*

Tsinghua University · Alibaba Cloud · Beihang University  
**IEEE APCCAS 2026 · Poster**

| Real-server tracking MAE | Real-server mean fan power | Simulator temperature MAE |
| :---: | :---: | :---: |
| **0.90 °C** | **13.95 W** | **1.83 °C** |

## From server telemetry to fan control

- **Simulation.** A physics-informed Temporal Fusion Network predicts CPU temperature. A lightweight LSTM estimates fan power from PWM history, enabling policy iteration in simulation.
- **Bounded policy.** Prior-Regularized Policy Optimization (PRPO) uses a Beta-distributed policy for PWM commands and an asymmetric reward that penalizes temperature overshoot, high fan duty, and abrupt PWM changes.
- **Online adaptation.** In shadow mode, the policy learns from the rule-based controller while that controller drives the fans. During active control, a finite state machine keeps the rules in charge in steady state and routes transient workloads to the learned policy.

## Real-server benchmark

| Controller | Tracking MAE ↓ | Peak signed deviation ↓ | Mean fan power ↓ | Composite cost (3C) ↓ |
| --- | ---: | ---: | ---: | ---: |
| Naive-PID | 5.94 °C | −3.00 °C | 15.73 W | 0.67 |
| BP-PID | 1.74 °C | 1.00 °C | 14.56 W | 0.30 |
| DQN | 3.52 °C | 7.27 °C | 14.35 W | 0.58 |
| **Ours** | **0.90 °C** | 1.45 °C | **13.95 W** | **0.14** |

Compared with BP-PID on real servers, the proposed controller reduced mean absolute tracking error by **48.3%** and mean fan power by **4.2%**. The 3C score gives equal weight to the three min–max normalized metrics in the table. A negative peak signed deviation means the temperature stayed below the setpoint during the measured run.

At a **60 °C setpoint in simulation**, peak overshoot fell from **7.00 °C to 1.05 °C** versus I-PID, while mean fan power was **23.63 W versus 22.14 W**. The proposed controller achieved the lower composite cost at all five simulated setpoints (50–70 °C).
