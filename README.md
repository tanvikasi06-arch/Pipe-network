# Pipe-network

A steady-state 1D model of a network of pipes, written in Python. Given fixed pressures at some nodes (such as a supply tank) and the flow drawn off at others, it calculates the pressure at every node and the flow in every pipe, including networks with loops.

## Physics

- **Pressure drop:** Darcy–Weisbach, ΔP = f · (L/D) · ρv²/2
- **Friction factor:** 64/Re for laminar flow; the Colebrook equation for turbulent flow, solved by root-finding (`scipy.optimize.brentq`)
- **Network equations:** one momentum equation per pipe, plus either a fixed pressure or a mass balance at each node, all solved together with `scipy.optimize.fsolve`

## How to run

Open `Pipemodel.ipynb` in Jupyter or Google Colab and select **Run all**. Each validation cell ends with `assert` checks, so if every cell runs without an error, every test has passed.

Requires `numpy` and `scipy`.

## Validation and tests

| Test | What it checks |
|---|---|
| Single pipe | Matches a hand calculation (water, 5 L/s, 100 m of 50 mm steel pipe: about 1.39 bar) |
| Parallel pipes | Two identical pipes split the flow 50/50 |
| Laminar flow | Matches the analytical Hagen–Poiseuille result |
| Mass balance and direction | Flow balances at an intermediate node, and a pipe defined against the flow gives a negative flow |
| Looped network | A tank feeding two outlets in a ring: total supply equals total demand, and flow balances at each outlet |

## Ideas for extending it

- Add a pump with a curve ΔP = a − bQ² and find its operating point
- Add elevation changes (ρgΔz) between nodes
- Validate against a published Hardy Cross textbook problem
- Add heat transfer along each pipe
