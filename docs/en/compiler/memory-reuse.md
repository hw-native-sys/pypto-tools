# Memory Reuse Analysis

`memory_map` renders a pass dump as an interactive HTML map of on-chip memory. It complements [IR Pass Trace](ir-pass-trace.md) when you need to understand how compiler output uses and reuses memory.

## Read the memory map

- The horizontal axis represents memory addresses.
- The vertical axis represents lifetimes, increasing downward.
- Each tile is a rectangle: its horizontal span covers the bytes occupied by the tile's MemRef, and its vertical span covers the statement interval during which the MemRef is live.

The address and lifetime axes make reuse decisions visible at a glance, without switching between separate tables.
