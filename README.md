# LMP Calculator

A Python tool for calculating Locational Marginal Prices (LMPs) using Optimal Power Flow (OPF) analysis. This project provides an easy-to-use interface for running OPF calculations on power system networks defined in Matpower format.

## Features

- **Single OPF Analysis**: Run optimal power flow calculations for specific load scenarios
- **Multiple Load Scenarios**: Automatically iterate through ranges of load values to analyze LMP sensitivity
- **Matpower File Support**: Parse and work with standard Matpower case files (`.m` format)
- **Comprehensive Results**: Extract LMPs, voltages, angles, and power flows for all buses
- **Flexible Configuration**: Modify loads at specific buses using absolute values or deltas
- **Automatic Output**: Results are saved to text files with descriptive names

## Installation

### Prerequisites

- Python 3.7 or higher
- pip (Python package manager)

### Setup

1. Clone this repository:
```bash
git clone https://github.com/yourusername/LMP_Calc.git
cd LMP_Calc
```

2. Install required dependencies:
```bash
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

## Project Structure

```
LMP_Calc/
├── data.m              # Matpower case file (input)
├── single.py           # Single OPF analysis script
├── multiple.py         # Multiple load scenario analysis script
├── opf/
│   ├── __init__.py
│   ├── helper.py       # High-level helper functions
│   ├── parser.py       # Matpower file parser
│   └── solver.py       # OPF solver using PyPower
├── requirements.txt    # Python dependencies
└── README.md          # This file
```

## Dependencies

- **numpy**: Numerical computations
- **pandas**: Data manipulation and analysis
- **pypower**: Optimal power flow solver

See `requirements.txt` for specific versions.

## Input File Format

The tool expects Matpower case files (`.m` format) containing:
- Bus data (bus numbers, types, loads, voltages)
- Generator data (generation limits, costs)
- Branch data (transmission line parameters)
- Generator cost data

Example structure:
```matlab
mpc.baseMVA = 100.00;
mpc.bus = [...];
mpc.gen = [...];
mpc.gencost = [...];
mpc.branch = [...];
```

## How It Works

1. **Parsing**: The `parser.py` module reads and parses Matpower case files
2. **Load Modification**: Loads at specified buses are modified according to your configuration
3. **OPF Solution**: PyPower's `runopf` function solves the optimal power flow problem
4. **Results Extraction**: LMPs and other bus quantities are extracted from the solution
5. **Output**: Results are formatted and saved to text files

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is open source and available for use and modification. Please check the license file for details.

## Acknowledgments

- Built using [PyPower](https://github.com/rwl/PYPOWER), a Python port of MATPOWER
- Inspired by power system economics and locational marginal pricing analysis

## Support

If you encounter any issues or have questions, please open an issue on the GitHub repository.


