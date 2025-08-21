# Improv Casting Optimizer

An integer programming solution for optimizing improv show casting assignments based on performer preferences while ensuring fairness and meeting sketch requirements.

## Overview

This tool helps improv groups create optimal casting assignments for their shows by:
- Maximizing overall cast happiness based on individual preferences
- Ensuring fair distribution of roles across all performers
- Meeting exact casting requirements for each sketch
- Balancing between total group satisfaction and individual fairness

## Features

- **Preference-based optimization**: Performers rank sketches from most desired (1) to least desired (n)
- **Flexible casting constraints**: Each performer gets 1-2 sketches
- **Customizable sketch sizes**: Define exact number of performers needed per sketch
- **Fairness balancing**: Adjustable coefficient to prioritize group happiness vs. preventing individual dissatisfaction
- **Host restrictions**: Optional constraint to prevent the host from performing in other sketches

## Requirements

```bash
pip install pandas pulp
```

## Usage

### 1. Prepare Your Data

Create a CSV file named `cast_preferences.csv` with:
- First column: "Player" (performer names)
- Remaining columns: Sketch names
- Values: Rankings from 1 (most desired) to n (least desired)

Example:
```csv
Player,4 corners,New choice,Alphabet,Nightclub,3579,Host
Alice,1,3,2,5,4,6
Bob,2,1,4,3,6,5
Charlie,3,2,1,4,5,6
...
```

### 2. Configure Sketch Sizes

Edit the `skit_sizes` dictionary in the code to match your show's requirements:

```python
skit_sizes = {
    "4 corners": 4,
    "New choice": 3,
    "Alphabet": 3,
    "Nightclub": 7,
    "3579": 4,
    "Host": 1
}
```

### 3. Adjust Optimization Parameters

- **Fairness Coefficient** (`fairness_coefficient`):
  - `0`: Maximize total group happiness only
  - `1-5`: Balance group happiness with individual satisfaction
  - `1000+`: Prioritize fairness above total happiness

- **Host Participation** (`host_plays_games`):
  - `True`: Host can perform in other sketches
  - `False`: Host only hosts, doesn't perform

### 4. Run the Optimizer

```bash
python improv_casting.py
```

## Output

The optimizer provides:
1. **Casting Results**: Shows which performers are assigned to each sketch
2. **Individual Assignments**: Lists sketches assigned to each performer
3. **Optimization Metrics**: Total happiness score and minimum individual happiness

Example output:
```
Casting results:
4 corners (4 slots): Alice, Bob, Charlie, Dana (count: 4)
New choice (3 slots): Eve, Frank, Grace (count: 3)
...

Skits by person:
Alice: 4 corners, Alphabet
Bob: 4 corners, New choice
...
```

## How It Works

1. **Happiness Calculation**: Preferences are converted to happiness scores (higher rank = lower happiness)
2. **Optimization Model**: Uses integer programming to find the assignment that maximizes:
   - Total group happiness (sum of all individual happiness scores)
   - Plus a fairness bonus (minimum individual happiness × fairness coefficient)
3. **Constraints**:
   - Each performer appears in exactly 1 or 2 sketches
   - Each sketch has exactly the specified number of performers
   - Optional: Host cannot perform in other sketches

## Validation

The tool includes automatic validation to ensure:
- All preference rankings are valid (no duplicates or missing ranks)
- Sketch names in preferences match configured sketch sizes
- Solution meets all constraints

## Troubleshooting

- **"Infeasible" status**: The constraints cannot be satisfied. Check:
  - Total slots needed vs. available performers
  - Minimum/maximum appearances per performer
  - Host constraints if enabled
  
- **Unequal casting**: Verify the assignment extraction is capturing all assignments (performers can be in multiple sketches)

## License

[Add your license here]

## Contributing

[Add contribution guidelines if applicable]