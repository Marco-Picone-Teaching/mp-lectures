<!-- omit in toc -->
# Python Best Practices & Object Oriented Programming

<!-- omit in toc -->
## Lecture Information

| **Lecture Title**   | Python Best Practices & Object-Oriented Programming (OOP)                        |
|---------------------|----------------------------------------------------------------------------------|
| **Author**          | Prof. Marco Picone (marco.picone@unimore.it)                                     |
| **License**         | [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/) | 

This lecture is split into **two standalone files**, each covering
one macro-section of the lecture — **Section A** and **Section B** respectively —
meant to be read in order. All numbering inside each file (headings, table of
contents, cross-references) is nested under its own macro-section: every heading in
`python_best_practices.md` is numbered `A.x`, and every heading in `python_oop.md`
is numbered `B.x`.

### Section A — [`python_best_practices.md`](python_best_practices.md) — *Python Best Practices*

Practices that make *any* Python code more robust, readable, and maintainable,
regardless of whether it uses OOP. It covers:

- What `if __name__ == "__main__":` actually does, and why it matters
- Exception handling (`try` / `except` / `else` / `finally`, custom exceptions)
- Python data types and type hints — and what goes wrong without them
- Getting rid of hardcoded values: CLI arguments, environment variables, YAML/JSON config files
- Logging as a replacement for scattered `print()` calls
- Dependency management with `requirements.txt` and `pyproject.toml`
- Virtual environments with `venv`, and how they pair with dependency management
- Python project layout, modules, packages, and absolute vs. relative imports
- `uv` as a modern, faster all-in-one project/dependency/venv workflow

### Section B — [`python_oop.md`](python_oop.md) — *Python OOP & Use Case Modelling*

Object-Oriented Programming from first principles, applied to an IoT use case:

- OOP introduction: paradigms, procedural vs. OO, the four pillars
- OOP in Python: classes, `__init__`, `self`, instance/class/static members, dunder methods
- Encapsulation and access control, getters/setters, `@property`
- Inheritance, method overriding, multiple inheritance, MRO
- Polymorphism (duck typing, operator, class-based) and abstraction with `abc`
- Exception management in an OOP context
- **Smart Home example**: identifying entities, sensors & actuators, refactoring the model with inheritance (`Device` → `Sensor` / `Actuator` → `TemperatureSensor`, `HumiditySensor`, `SmartLight`)
- Smart Home + Data Manager, and implementing the `SmartHome` class and its behaviours
- Design patterns: Singleton, Factory, Observer, and the delegation principle