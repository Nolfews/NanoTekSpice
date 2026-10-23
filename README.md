# NanoTekSpice

NanoTekSpice is a digital circuit simulator. It reads a circuit description from a `.nts` file, builds the circuit from the declared components and links, and lets you simulate and inspect its logic states through an interactive shell.

## Features

- Three-state logic: `0`, `1`, and undefined `U`
- Input and clock components
- Output, constant, and elementary logic components
- Supported gate and integrated circuit components implemented in `src/components/`
- Circuit simulation with tick tracking
- Interactive display and continuous simulation mode
- Unit tests and functional tests

## Requirements

- A C++ compiler with C++20 support, such as `g++`
- `make`
- Python 3 for the functional tests
- Criterion, if you want to run the unit tests
- `gcovr`, if you want to generate coverage reports

## Build

From the project root, run:

```sh
make
```

This creates the `nanotekspice` executable.

To remove build files and the executable:

```sh
make clean
make fclean
```

## Usage

Start the simulator with a circuit file:

```sh
./nanotekspice tests/files/valid.nts
```

The simulator then opens an interactive shell. Available commands are:

| Command | Description |
| --- | --- |
| `display` | Display the current simulation tick, inputs, and outputs |
| `simulate` | Simulate one tick |
| `loop` | Repeatedly simulate and display the circuit; run it again to stop |
| `name=value` | Set an `input` or `clock` component to `0`, `1`, or `U` |
| `exit` | Exit the simulator |

Example session:

```text
> in_1=1
> in_2=0
> simulate
> display
tick: 1
input(s):
  in_1: 1
  in_2: 0
output(s):
  out: 0
> exit
```

## Circuit file format

A circuit file contains a `.chipsets:` section followed by a `.links:` section. Each component receives a unique name. Links connect component pins using the format `component:pin`.

Example:

```text
.chipsets:
input in_1
input in_2
output out
and gate

.links:
in_1:1 gate:1
in_2:1 gate:2
out:1 gate:3
```

Lines beginning with `#` are comments. The repository contains additional examples in `tests/files/` and `tests/files/nts_single/`.

## Tests

Build and run the unit tests with Criterion:

```sh
make tests_run
```

Run the functional test suite:

```sh
make functionnal_tests
```

Generate a coverage report:

```sh
make coverage
```

## Project structure

- `main.cpp`: command-line entry point
- `src/core/`: parsing, circuit management, and simulation
- `src/components/`: elementary gates, integrated circuits, and special components
- `src/Factory.*`: component creation
- `tests/`: unit tests, functional tests, and sample circuit files
