| Document             | Status |
| -------------------- |:------:|
| Overall architecture | DRAFT  |

# Overall architecture

### Archived version
The first ever discussion of this is [archived on HackMD](https://hackmd.io/sIrqn4m3QymdaMWwk447KA)

## Engine requirements
We think an arbitrary requirement engineering is required in order to make a high quality game engine.
Before we start with technical fuss, here is a list of points that are deemed _"essential"_ in order for a game engine to ship.

- Users should be able to easily install one or multiple versions of the game (engine) at a time
- The installation should be possible in user space (no admin privilege) and it should be self-updating
  - Ideally a one-click solution for all platforms is preferred
- Different components should be exchangeable on runtime, ideally with multiple versions support
- Developers should be able to build & run multiple versions of the game engine at the same time
- Developers should be able to debug, pause, resume, forward (..) the game engine at any time

## Architecture requirements for yaggne
Every component, as well as it's composition of components should fullfill the requirements defined.
The current document specifies a _"definition of requirements"_ as defined below:

- every part of the engine should be an individual component
- thus, every part of the engine should be testable in isolation
- thus, every part of the engine should be plug and play as long as it meets the specification

## High level architecture for yaggne
Yaggne is structured in two main ecosystems:

- Manager
- Executor

This distinction is made in order to split the system into computation-heavy tasks as well as tasks that are purely business-logic.
The system is illustrated below, and explained further in detail in the spec.
![yangge architecture](../blob/master/specs/yangge.png)
We will introduce a few formal definitions in order to work with them in the specification.

### Component
We previously used the term _"component"_, which is defined as a single unit of code, executing a specific purpose. In concrete this can mean:
- a function, class, object or type definition
- a composition of functions, classes, objects or type definitions
- a module that structures those compositions

### Manager
The manager orchestrates task groups. In order to do this, the manager uses it's own set of plugins (manager language) to determine what is necessary to execute a task group (e.g load a player model). Once determined, it will instruct the executor to load the corresponding plugins.

### Executor
The executer schedules plugins, similliar to a main-loop in a traditional game engine

- schedules plugins -> components
- takes care of task execution based on configured context (see plugin config)
    - notifies execution unit to execute task
- execution unit = queue-compatible collectable which will run tasks
    - e.g commonly this would be a thread pool (or multiple)

#### Plugins
A plugin
- contains a PluginMeta (or PluginConfig) about execution order (...)
- defines one or multiple tasks

#### Tasks
Describes the actual unit being executed (executed means, delayed for thread pool execution of a function, object ....)

### Task group
A task group defines a set of tasks that are actually a product for the user (e.g game client, server)
