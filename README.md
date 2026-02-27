# Race-to-8 Digital Blackjack (Discrete Logic Implementation)

## Project Overview

This project implements a two-player, Blackjack-inspired “Race to 8” game entirely using discrete digital logic components. The system was first designed and simulated using TINA circuit simulation software, then physically implemented on breadboards using TTL logic ICs.

The objective was to design a sequential digital system incorporating counters, memory elements, arithmetic logic, and magnitude comparison to determine a winner based on proximity to a target value (8).

![WhatsApp Image 2026-02-27 at 10 37 10 PM](https://github.com/user-attachments/assets/d387673e-d39f-4b3a-b5e6-1bdfe21fc792)

---

## Game Concept

The game is a simplified multiplayer version of Blackjack:

- Two players compete.
- Each player generates up to two “cards”.
- Each card is a number from 1 to 8.
- The player whose total is closest to 8 (without exceeding it) wins.

---

## System Operation

### Phase 1 – Player 1 Turn

1. Player 1 presses a push button.
2. A clock-driven binary counter begins cycling from 1 to 8.
3. The output is displayed as a 4-bit binary value.
4. Player 1 presses a "Hold" button to store the card value.
5. The value is stored using JK flip-flops (memory).
6. Player 1 may repeat the process to generate a second card.
7. A 4-bit binary adder sums both card values.

---

### Phase 2 – Player 2 Turn

The same process is repeated for Player 2:

- Generate card using counter
- Store value using flip-flops
- Add two card values using binary adder

---

## Winner Determination

A 4-bit magnitude comparator compares:

- Player 1 total
- Player 2 total
- Target value (8)

The logic determines:

- Equal to 8 (automatic win)
- Closest to 8 without exceeding
- Over 8 (bust condition)

LED indicators display the winner.

---

## Reset Logic

A reset button clears:

- All counters
- All flip-flops
- All stored values
- Display outputs

This prepares the system for a new game round.

---

## Engineering Design Phases

### Phase 1 – Simulation (TINA)

- Designed complete digital logic circuit
- Verified counter cycling and reset behavior
- Tested arithmetic addition logic
- Confirmed comparator output states
- Debugged logic before hardware build

### Phase 2 – Hardware Implementation

- Implemented full circuit on breadboards
- Wired multiple TTL ICs manually
- Connected clock and control buttons
- Integrated 7-segment display drivers
- Debugged timing and signal stability
- Verified real-world operation

---

## Hardware Components Used

- Binary Counters
- JK Flip-Flops (Memory Storage)
- 4-bit Binary Adder (SN7483)
- 4-bit Magnitude Comparator (SN7485)
- 7-Segment Display Drivers
- Clock Generator
- Push Buttons (User Input)
- TTL Logic ICs (74xx Series)
- Breadboards and discrete wiring

---

## Digital Logic Concepts Demonstrated

- Sequential logic systems
- State retention using flip-flops
- Clock-driven state transitions
- Binary arithmetic operations
- Digital comparison logic
- Hardware debugging
- Simulation-to-hardware workflow

---

## Outcome

The final system successfully:

- Generates random-like card values via clock counter
- Stores player data using memory elements
- Computes binary sums
- Compares totals using magnitude logic
- Displays winner based on defined game rules
- Resets cleanly for repeated play

This project demonstrates practical implementation of core digital system principles, integrating combinational and sequential logic into a fully functional hardware system.
