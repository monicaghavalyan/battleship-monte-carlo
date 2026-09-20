# Battleship Monte Carlo

A Monte Carlo approach to target selection in Battleship. Instead of tracking board state analytically, the algorithm estimates where ships are likely to be by generating a large number of random valid ship placements and counting how often each cell is occupied. The highest-scoring unshot cell becomes the next target.

The purpose of the experiment is to observe how shot accuracy changes as the sample count grows. The same initial grid is replayed once per sample size, so the results are directly comparable.

University course project (Artificial Intelligence).

## Contents

| File | Description |
|---|---|
| `battleship_monte_carlo.py` | Grid setup, ship placement, the Monte Carlo estimator and the simulation loop |
| `report.pdf` | Write-up of the method and results |
| `slides.pdf` | Presentation covering the same material |

## How it works

1. An initial grid is created and ships are placed at random positions and orientations.
2. For a given sample count, many independent grids are generated with random valid placements. Each cell accumulates a count of how often it held a ship, with already-shot cells excluded.
3. The cell with the highest count is fired at, and the result is recorded as a hit (`X`) or miss (`O`).
4. Steps 2 and 3 repeat for the configured number of shots, then the whole run repeats for the next sample count.

Only the standard library is used — `random`, `argparse` and `copy`.

## Usage

```bash
python battleship_monte_carlo.py
```

Defaults match the configuration used in the report: an 8x8 grid, ships of length 3, 3 and 2, sample counts of 100, 500, 1000, 10000 and 30000, and 25 shots per run.

All parameters can be overridden:

```bash
python battleship_monte_carlo.py \
  --grid_size 10 \
  --ship_lengths 4 3 3 2 \
  --num_samples 100 1000 5000 \
  --num_of_shots 30
```

| Argument | Default | Description |
|---|---|---|
| `--grid_size` | `8` | Side length of the square grid |
| `--ship_lengths` | `3 3 2` | Length of each ship to place |
| `--num_samples` | `100 500 1000 10000 30000` | Sample counts to compare, one run each |
| `--num_of_shots` | `25` | Shots fired per run |

Output is the initial grid followed by the final grid for each sample count, where `*` is an unhit ship segment, `X` a hit and `O` a miss.

Runtime grows with the product of sample count and shot count, so the higher sample sizes take noticeably longer.
