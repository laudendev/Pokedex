# Pokédex

A dataset query tool with a custom command language, built on a Pokémon
dataset. Instead of nested menus, queries are single commands:

    $ Type : Fire ; Attack : @descend $

The command parser turns each command into a dictionary, matches keys to
filter functions built as lambda closures, and applies them recursively
against the dataset. Sorting works on any attribute via lambda-keyed
`sorted()`, ascending by default with an `@descend` modifier. The design
generalizes: point it at any dataset that can be encapsulated in an
object, and the same command structure gives you filtering and sorting
over its attributes.

## Usage

Interactive mode:

    python3 pokedex.py

Type `$ Get : Help $` for the command reference.

Batch mode — run commands from one or more files:

    python3 pokedex.py commands1 [commands2 ...]

Output labels each command with its results, per file.

## Design

The assignment specified menus with filter and sort options. I thought that was inefficient — finding one Pokémon by name shouldn't require walking sub-menus — so I proposed a command language instead, and the professor approved it.

- **Parser** (`command_parser.py`): validates delimiters, splits on `;`,
  builds a command dictionary via dict comprehension. Unknown keys fail
  with an error rather than being ignored.
- **Filters**: each command key maps to a lambda closure; matched
  filters accumulate into a list applied recursively to the Pokédex.
  Functions as first-class objects do the composition work.
- **Sorting**: any attribute, using lambdas as sort keys.
- **Data**: the [Kaggle Pokémon dataset](https://www.kaggle.com/abcsds/pokemon),
  cleaned with pandas (see `modify_pokemon_data.ipynb`).

---

*CSC 402, Kutztown University, 2021. Archived — the design idea
(command-language query over arbitrary datasets) is one I still reach for.*
