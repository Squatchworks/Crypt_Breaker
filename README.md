# Crypt Breaker

![Language](https://img.shields.io/badge/language-C%2B%2B17-blue)
![Platform](https://img.shields.io/badge/platform-Windows%20x64-lightgrey)
![Renderer](https://img.shields.io/badge/renderer-Win32%20Console-darkgrey)
![Build](https://img.shields.io/badge/build-Visual%20Studio%202022-purple)

A console-based brick breaker built with **C++ and the Win32 Console API**. Five bricks, a ball, a paddle, multi-hit collision, and win/lose state — rendered entirely in a Windows terminal at a fixed frame rate.

> Part of the Squatchworks portfolio. Developed by [Jacob Blackburn](https://github.com/Squatchworks).

---

## Gameplay

- Move the paddle left/right with **Arrow Keys**
- Launch/pause the ball with **Space**
- Each brick takes **3 hits** to destroy — it darkens each hit as it takes damage
- Clear all five bricks to win
- If the ball drops below the paddle, you lose
- Press **R** to reset at any time
- Press **Escape** to quit
- **Up/Down Arrow** adjusts the frame rate live

---

## Features

- Five evenly spaced bricks managed in a `std::vector<Box>`
- Multi-hit collision: bricks track damage via `ConsoleColor` value — each hit decrements the color until the brick is removed from the vector
- Win condition: all bricks cleared, ball halts, victory text displayed
- Lose condition: ball passes paddle y-position, ball halts, reset prompt displayed
- Fixed frame rate loop using `std::chrono` and `std::this_thread::sleep_for`
- Custom `Console` utility class wrapping Win32 `HANDLE` calls for cursor positioning, color, box drawing, and screen locking

---

## Architecture

Crypt_Breaker/
├── Main.cpp # Entry point — game loop with chrono frame timing
├── stdafx.h # Precompiled header — Windows includes, constants (80×40, 15 FPS)
├── Game.cpp / Game.h # Core loop — Update, Render, CheckCollision, Reset
├── Ball.cpp / Ball.h # Ball movement and boundary reflection
├── Box.cpp / Box.h # Brick and paddle — DrawBox, Contains(x, y) hit test
├── BaseObject.cpp / .h # Base class — position, color, visage, Draw()
└── Console.cpp / Console.h # Win32 console abstraction — cursor, color, box drawing, locking

`BaseObject` is the shared base for `Ball` and `Box`. The `Console` class is a static Win32 wrapper that handles all terminal rendering — cursor positioning, foreground/background color via `SetConsoleTextAttribute`, and Unicode box-drawing characters. `Box::Contains()` drives all collision detection. `Game` owns the brick vector and manages the full game state.

---

## How to Build

### Prerequisites

- Visual Studio 2022 with the **Desktop development with C++** workload
- Windows 10/11 x64

### Build steps

1. Clone the repo:
git clone https://github.com/Squatchworks/Crypt_Breaker.git
2. Open `Bricks.sln` in Visual Studio 2022.
3. Set configuration to **Debug | x64** or **Release | x64**.
4. **Build → Build Solution** (`Ctrl+Shift+B`).
5. Run the output executable. The console window will resize itself to 80×40 automatically.

No external dependencies or package manager required — pure Win32 and standard library.

---

## Lessons Learned

- **`std::vector` erase-during-iteration** requires index management — erasing at `_bricks.begin() + index` inside a ranged loop would invalidate iterators, so an index-based for loop is used in `CheckCollision`.
- **Color as a damage counter** — using `ConsoleColor` enum value as a hit point proxy is a compact pattern for early-stage console games, though it couples visual state to game logic in a way that would need separation at scale.
- **Win32 console locking** — `Console::Lock(true/false)` wraps `LockWindowUpdate` to suppress flicker during full-screen clears. This is the console equivalent of double-buffering.
- **Frame timing with `std::chrono`** — subtracting elapsed milliseconds from the target frame budget and sleeping the remainder gives a consistent 15 FPS without a busy-wait loop.

---

## Roadmap

### Current build
- Five-brick single-row layout with multi-hit destruction
- Win/lose state with R-to-reset
- Live frame rate adjustment via Up/Down arrow keys

### Planned — "CryptBreaker: Paranormal Edition"
A full reskin targeting the Cryptid Forge Studios aesthetic:

- Rune stone bricks with paranormal-themed damage states
- Spirit orb ball with trail effect
- Atmospheric tomb environment and color palette
- Sound effects and ambient audio
- Multi-row brick layouts and level progression
- Branding and visual identity pass consistent with the Cryptid Forge Studios catalog

Currently in the branding and design backlog.

---

## Author

**Jacob Blackburn** — Gameplay programmer and C++ developer based in Russells Point, Ohio.
- GitHub: [@Squatchworks](https://github.com/Squatchworks)
- LinkedIn: [linkedin.com/in/squatchworks](https://linkedin.com/in/squatchworks)
