# Micromouse Maze Fire Escape Simulation

Micromouse maze simulation notebook refactored for a fire-escape robot applicability study.

## Main Notebook

- `micromouse_maze_알고리즘.ipynb`
- `micromouse_maze_알고리즘_20260531_2359.ipynb`

Both notebooks include:

- 7x7 local observation based position-candidate estimation
- virtual sensor CSV mode
- smoke, CO, temperature, obstacle based risk map generation
- risk-aware A* planning
- dynamic sensor-map-risk update loop with automatic replanning
- metrics: `path_length`, `computation_time`, `observed_risk_exposure`, `true_risk_exposure`, `replanning_count`, `blocked_event_count`, `success`
- Colab-compatible execution and optional Google Drive save cell

## Safety Scope

This project uses only safe simulated risk values. It does not perform real fire, toxic gas, or hardware sensor experiments.

## Colab

Open the notebook in Google Colab and run cells from top to bottom. The legacy animation is disabled by default with:

```python
RUN_LEGACY_ANIMATION = False
```

The fire-escape simulation runs by default with:

```python
RUN_FIRE_ESCAPE_EXPERIMENT = True
```

The external maze dataset/tool directory is intentionally not committed here because it is a separate Git repository. The notebook contains a Colab cell that clones:

```text
https://github.com/micromouseonline/micromouse_maze_tool.git
```
