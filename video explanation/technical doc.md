# Technical Design Documentation  
## Race-to-8 Digital Blackjack (Discrete Logic System)

---

# 1. System-Level Architecture

The system is divided into functional modules:

1. Clock Generator  
2. Counter Module (Card Generator)  
3. Memory Module (JK Flip-Flops)  
4. Arithmetic Module (4-bit Adder)  
5. Comparator Module (Winner Logic)  
6. Reset Module  
7. Display Module  

Signal Flow:

Clock → Counter → Hold → Flip-Flops → Adder → Comparator → Output LEDs

---

# 2. Finite State Flow (Game Logic)

The game operates in sequential phases:

| State | Description |
|-------|------------|
| S0 | Idle / Reset State |
| S1 | Player 1 Generating Card 1 |
| S2 | Player 1 Hold |
| S3 | Player 1 Generating Card 2 |
| S4 | Player 2 Generating Card 1 |
| S5 | Player 2 Hold |
| S6 | Player 2 Generating Card 2 |
| S7 | Compare Totals |
| S8 | Display Winner |
| S9 | Reset |

State transitions occur via push-button triggers and clock pulses.

---

# 3. Counter Module (Card Generator)

The counter is a synchronous binary counter:

- 4-bit output
- Counts from 0001 (1) to 1000 (8)
- Automatically resets after 8

Truth Table (Simplified)

| Current State | Next State |
|--------------|------------|
| 0001 | 0010 |
| 0010 | 0011 |
| 0011 | 0100 |
| 0100 | 0101 |
| 0101 | 0110 |
| 0110 | 0111 |
| 0111 | 1000 |
| 1000 | 0000 (Reset) |

The player manually stops the counter by pressing HOLD.

---

# 4. Memory Module – JK Flip-Flop Storage

Each player card uses 4 JK flip-flops.

## JK Flip-Flop Characteristic Table

| J | K | Q(next) |
|---|---|----------|
| 0 | 0 | Q (No Change) |
| 0 | 1 | 0 |
| 1 | 0 | 1 |
| 1 | 1 | Toggle |

In this design:

- Counter output feeds J/K inputs.
- HOLD pulse acts as clock trigger.
- After latching, flip-flops maintain state independently.
- This creates stable 4-bit memory.

Memory Equation:

Q(next) = JQ' + K'Q

This demonstrates sequential logic behavior.

---

# 5. Arithmetic Module – 4-bit Binary Adder (SN7483)

The adder computes:

Total = Card1 + Card2

For each bit:

Sum = A ⊕ B ⊕ Cin  
Carry = AB + Cin(A ⊕ B)

Full 4-bit addition:

S3 S2 S1 S0 = A3A2A1A0 + B3B2B1B0

If Carry-out = 1 and Sum > 8 → Bust Condition

---

# 6. Comparator Module – SN7485

The comparator evaluates:

Player1_Total vs Player2_Total

Outputs:

| A > B | A = B | A < B |
|-------|-------|-------|
| 1 | 0 | 0 |
| 0 | 1 | 0 |
| 0 | 0 | 1 |

Additional logic checks:

- If total = 8 → Direct win
- If total > 8 → Loss (bust)

Final winner logic:

If (P1 ≤ 8 AND P2 ≤ 8)
    Winner = Greater(P1, P2)
Else if (P1 > 8 AND P2 ≤ 8)
    Winner = P2
Else if (P2 > 8 AND P1 ≤ 8)
    Winner = P1
Else
    Tie

---

# 7. Timing Considerations

The system relies on synchronous clocking.

Important aspects:

- Flip-flops trigger on clock edge.
- Hold button must be debounced.
- Counter propagation delay affects display stability.
- Comparator output stabilizes after adder delay.

Total propagation delay:

t_total ≈ t_counter + t_flipflop + t_adder + t_comparator

---

# 8. Reset Logic

Reset clears:

- Counter registers
- Flip-flop memory
- Carry bits
- LED outputs

Reset condition forces:

All Q = 0  
System returns to State S0.

---

# 9. Engineering Analysis

This system combines:

Sequential Logic:
- JK flip-flops
- Clock synchronization
- State retention

Combinational Logic:
- Binary addition
- Magnitude comparison
- Win condition logic

It demonstrates full digital system integration including:

- State machine behavior
- Arithmetic computation
- Real hardware timing
- Physical implementation constraints

---

# 10. Design Challenges

- Clock bounce from mechanical push buttons
- Signal noise on breadboard wiring
- Propagation delay across IC chain
- Ensuring stable reset behavior
- Avoiding metastability during state capture

---

# Conclusion

This project represents a complete hardware-based digital system integrating memory, arithmetic logic, state transitions, and real-time comparison logic into an interactive two-player game.

The design reflects applied knowledge of digital electronics, logic design, sequential circuits, and hardware debugging.
