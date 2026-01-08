# LMP_Toolkit

A Python tool that takes a PowerWorld output file and allows users to modify bus-level demand to compute locational marginal prices (LMPs) using optimal power flow (OPF) analysis, enabling fast and computationally efficient evaluation of how prices change across scenarios.

## Features

### Single OPF Analysis

Run optimal power flow calculations for specific load scenarios to determine locational marginal prices (LMPs) at each bus in the system. This feature allows you to:

- **Modify loads at specific buses**: Set exact load values (in MW) at one or more buses to analyze how different demand levels affect pricing
- **Absolute or delta-based changes**: Specify either absolute load values or incremental changes from the current base case
- **Comprehensive bus-level results**: Extract LMPs ($/MWh), voltage magnitudes (per unit), voltage angles (degrees), and active/reactive power flows for every bus
- **Generator dispatch information**: View optimal generator outputs and costs for the solved scenario
- **Automatic result saving**: Results are automatically saved to text files with descriptive names that encode the load configuration

This is ideal for analyzing specific "what-if" scenarios, such as understanding how LMPs change when a particular load center increases or decreases its demand, or evaluating the impact of new load connections at specific locations.

### Multiple Load Scenarios

Automatically iterate through ranges of load values to analyze LMP sensitivity and create comprehensive pricing curves. This feature enables:

- **Parametric load sweeps**: Define start values, end values, and step sizes for multiple buses simultaneously
- **Independent load ranges**: Each bus can have its own unique load range and step size, allowing for flexible sensitivity analysis
- **Batch processing**: Automatically runs OPF calculations for all combinations of load values across the specified ranges
- **Sensitivity analysis**: Identify how LMPs respond to load changes, helping to understand congestion patterns and marginal cost variations
- **Consolidated output**: All scenarios are saved in a single output file, making it easy to compare LMPs across different load levels

This is perfect for creating LMP vs. load curves, identifying critical load levels where pricing changes significantly, analyzing transmission congestion patterns, or performing economic studies that require evaluating multiple demand scenarios efficiently.


### Setup

1. Clone this repository:
```bash
git clone https://github.com/wcolglazier/LMP_Toolkit.git
cd LMP_Toolkit
pip install -r requirements.txt
```



## Usage

### Single OPF Analysis

Run a single OPF calculation with specific load values:

```python
from opf.helper import run_opf_single, print_current_loads

# Print current loads in the system
print_current_loads(file_path="data.m")

# Run OPF with specific load values
bus_numbers = [5, 3]
new_loads = [60.0, 55.0]
run_opf_single(bus_numbers_abs=bus_numbers, new_loads_abs=new_loads, file_path="data.m")
```

Or modify the `single.py` file directly:

```python
from opf.helper import get_current_load, run_opf_single, print_current_loads

file_name = "data.m"

# Print current loads
print_current_loads(file_path=file_name)

# Configuration
bus_numbers_abs = [5, 3]
new_loads_abs = [60.0, 55.0]

# Run OPF
run_opf_single(file_path=file_name)
```

### Multiple Load Scenarios

Run OPF calculations across a range of load values:

```python
from opf.helper import run_opf_loop, print_current_loads

data_file = "data.m"

# Print current loads
print_current_loads(file_path=data_file)

# Configuration
bus_numbers = [5, 3]
load_starts = [40.0, 20.0]      # Starting load values (MW)
load_ends = [50.0, 30.0]        # Ending load values (MW)
load_step_sizes = [1.0, 1.0]    # Step size for each bus (MW)

# Run loop
run_opf_loop(bus_numbers, load_starts, load_ends, load_step_sizes, file_path=data_file)
```

Or modify the `multiple.py` file directly with your desired configuration.

### Output

Results are automatically saved to text files:
- Single runs: `opf_single_b{bus}_{load}MW.txt` or `opf_single_basecase.txt`
- Multiple runs: `opf_loop_b{bus}_{start}to{end}MW.txt`

Each output file contains:
- Load configuration for the scenario
- LMP values ($/MWh) for each bus in the system





## Input File Format

The tool expects Matpower case files (`.m` format) containing:
- Bus data (bus numbers, types, loads, voltages)
- Generator data (generation limits, costs)
- Branch data (transmission line parameters)
- Generator cost data



