yangge - Yet another next generation game engine
------------------------------------------------

## Preamble
Yangge, pronounced `/yang ge/`, sets it's goal in superseding the [Inexor](https://github.com/inexorgame) architecture with a clean cut from scratch.

Designing Inexor for the past has been a thrilling experience. It took us years of experimenting to end up with the heck of high level architecture that we are at now. However, these years of experiments have also left some serious implementation flaws, most of them still connected to the Cube2 engine.
Yangge aims to solve these problems, yet to set the barrier for all game engines much higher.

## Inspiration
There has been rumours in the gaming industry that a decent game must use decent hardware. Game engines like [Unity](https://unity3d.com/) are [literally eating up all available resources](https://www.gamasutra.com/blogs/RichGeldreich/20150731/250071/Lessons_Learned_While_Fixing_Memory_Leaks_in_our_First_Unity_Title.php). We know this is not true. A consequent, modular and decoupled design leads to a game engine that can meet all the demands of modern gamers, while being resource-savy.

## Top level document
This document serves as a top level entry to the yangge specification. Specification of individual entities can be found in the [specs](../blob/master/specs) folder.

## Implementation
Beside writing the specification for the yangge architecture, we strive to implement the essential code alongside. Ideally, the core ecosystem comes in < 10k lines of code. This concrete implementation can be found in the [impl](../blob/master/impl) folder.
