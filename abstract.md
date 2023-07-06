We consider the parameterized verification of arbitrarily large networks of agents which communicate by broadcasting and receiving messages. In our model, the broadcast topology is reconfigurable so  that a sent message can be received by any set of agents. In addition, agents have local registers which are initially distinct and may therefore be thought of as identifiers. When an agent broadcasts a message, it appends to the message the value stored in one of its registers. Upon reception, an agent can store the received value or test this value for equality with one of its own registers.
We consider the coverability problem, where one asks whether a given state of the system may be reached by at least one agent. We establish that this problem is decidable; however, it is as hard as coverability in lossy channel systems, which is non-primitive recursive. This model lies at the frontier of decidability as other classical problems on this model are undecidable; this is in particular true for the target problem where all processes must synchronize on a given state. By contrast, we show that the coverability problem is NP-complete when each agent has only one register.