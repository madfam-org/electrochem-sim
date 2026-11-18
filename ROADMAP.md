# Galvana Roadmap

> Last Updated: 2026-11-17

This file says where the simulator stands and what would be worth building next. Nothing below is
scheduled. The repository has been dormant since 2025-11, and reviving it is an owner decision that
has not been made; this is the list that decision would start from.

## Where it stands

Read from the repository on 2026-11-17, not run:

- **One solver**: `workers/sim-fenicsx/simple_solver.py`, a 1D Nernst–Planck model on finite
  differences with Butler–Volmer kinetics at one electrode. Despite its directory name it does not
  use FEniCSx.
- **One scenario**: `examples/scenarios/ni_plating_mvp.yaml`, nickel plating.
- A hardware layer for potentiostats with mock drivers, an API gateway and a web app.
- The README's own status section lists what it advertises and does not contain: the SDKs, a second
  solver, an orchestrator and part of the infrastructure.
- No battery, impedance or corrosion model was found in `services/` or `workers/` (searched by
  name; 31 Python files), although the README's feature list names all three.

## Proposed scenarios

Four questions a product team brought to this simulator in 2026-11. Each names a real need, what
answering it takes, and how far the simulator is from it. They are ordered by how directly the
answer reaches a customer.

| Scenario | The question | What it needs | Distance today |
|---|---|---|---|
| **Cell life of a small wireless device** | How long does a coin or small lithium cell last under a duty cycle of sleep, short radio bursts and a button press, across temperature? | A cell model (an equivalent-circuit model fitted to a datasheet is enough to start; a single-particle model later), a load-profile input, and capacity fade | No cell model exists |
| **Galvanic corrosion at a mixed-metal joint** | Where a steel fastener meets an aluminium frame and the electrolyte is sweat or saline, how fast does the aluminium go, and what coating or isolation stops it? | Mixed-potential theory over two electrodes, polarisation curves per metal and coating, a thin-film electrolyte | One electrode only; no mixed potential |
| **Finish thickness on a shaped part** | For anodising or plating a part with corners and recesses, how even is the layer? | Current distribution over 2D or 3D geometry (secondary distribution at least) | Closest to what exists, but the solver is 1D |
| **Storing braking energy instead of dissipating it** | For a drive that regenerates in short bursts, does a capacitor or small cell bank beat a resistor? | A storage model with charge acceptance and thermal limits; mostly a system question before it is an electrochemical one | No storage model |

## What "in good shape" would mean

Before any scenario is trusted:

1. **Validation against a published case** for each model, with the reference cited and the error
   stated. The repository has five test files today.
2. **A scenario format that states its assumptions**: every parameter with its source, so a result
   can be traced to a datasheet or a paper.
3. **A result record another tool can consume**: for example a cell with its capacity and
   envelope, a joint with its coating and isolation, a finish with its thickness. One record per
   result, with the scenario's digest.
4. **The README brought in line with the code**, so that nothing is advertised that is absent.

## Order, if the lane opens

1. Validation harness and scenario format (items 1 and 2 above), on the solver that exists.
2. The cell-life scenario, starting from an equivalent-circuit model.
3. Mixed potential for the galvanic couple.
4. Geometry beyond 1D, which is what the finish scenario and most real plating questions need.
