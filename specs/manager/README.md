| Document             | Status |
| -------------------- |:------:|
| Manager              | DRAFT  |

# Manager

## High level design overview
We learned some lessons from building [flex](https://github.com/inexorgame/inexor-flex).
These are mainly due to the nature of the tools we used, but also some architectural issues hit us during the construction.
We've come to a conclusion that `yangge` will need to satisfy the below requirements.

### Single source of truth.
Configuration files suck. Distributing and managing them is evil. We want a single, consistent, platform-independant source of truth for `yangge`
This is best done with a database, or database-alike structure.

### Modularity at runtime
Simply put: _make everything a plugin_. If you start linking your manager components before startup it will inevitably result in a monolithic design.
This is bad for a variety of reasons, among them:

- testability
- platform-independance (it is a pain in the ass to write integration tests for native Node.js addons)
- package-ability (making a single package from `10k` lines of code can be incredibly difficult)
- scalability
- (...)

The core of `yangge` manager should include four essential things:
- a mechanism for registering components
- a core component, which will take care of downloading and installing other components (and updating them)
- a routine to save and install the database provider (see above)
- a way to communicate configuration between itself and the database provider (synchronisation)

Everything else should be able to be hooked into the service at runtime. User requests should not be issued to the manager, but instead the single source of truth should be subject to instruct the manager for specific purposes (configuration change).
This design consequently makes every plugin a configuration object, and reduces maintenance burden. No more API to write or maintain, because issuing commands is as simple as adding configuration.

#### Interaction with the executor
The manager should introduce a mechanism similliar to a domain specific language. A configuration change is translated into a component call. A component call orchestrates a executor plugin. The executor plugin orchestrates it's tasks. The end result is a reduction from config change to actually calling the low-level functionality. Beside this mechanism, there are performance critical operations that will directly interact with the executor. These kind of interaction is discussed in the executor spec.
