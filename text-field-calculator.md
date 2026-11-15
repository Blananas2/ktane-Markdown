The Text-Field Calculator, often abbreviated as "TFC", is an online calculator that allows inputting through text over multiple lines. Created by community member samfundev, it is widely used during solves as a simple yet powerful tool for calculations.

It is accessible through https://samfundev.github.io/TextFieldCalculator/Calculator.html.

It has a V2 version at https://samfundev.github.io/TextFieldCalculator/Calculator-v2.html which allows for multi-line editing by middle-click dragging and has answers on the same line as the input instead of below it, for example.

Some of its features include the ability to:
- Create, assign and use variables through a standard `name = 3` syntax.
- Create and use functions through a standard `customName(x) = (x + 3)*2` syntax.
- "Mute" a line by making it not show up in the results by using a semicolon at the end of the line useful for declarations.
- Do math in different bases:
  - Binary numbers represented as `0b1011`, and converted from decimal using `bin(x)`.
  - Hexadecimal numbers represented as `0x1c6cf0`, and converted from decimal using `hex(x)`.
- Use many different constants, from `pi` and `phi` to `avogadro` and `planckTemperature`.
- Generate a random float using `random()`.
- Do operation on matrixes, including applying a function to it using `map(function, matrix)`.

For a more in-depth look, refer to [TFC's documentation](<https://samfundev.github.io/TextFieldCalculator/documentation>).

There exist a Locked version accessible by appending `?locked` at the end of the url (`.html?locked`) which was designed to make it closer to a standard calculator, disabling custom functions, variable assignment, many pre-existing functions, arrays and matrices, boolean operators and base conversion. It still retains

Locked TFC still allows:
- Addition `+`, Subtraction `-`, Multiplication `*`, Division `/`
- Parentheses `()`
- Modulo `%`
- Power `^` (but not `sqrt()`, use `^0.5` instead)
- Cos, Sin and Tan (but not acos, asin, atan nor atan2)