
# Object Capabilities For Security

from https://www.youtube.com/watch?v=EGX2I31OhBE

## Intro

Authority: ability to affect the rest of the world

Ambient authority: authority available even if you dont ask for it
(anti principle of least privilege)

when copying files, cp runs with all of the user's authority
cp only needs to access 2 files, but has access to all user's files

however, if you pass 2 opened file descriptors, cp doesnt need this anymore

Capabilities bundle designation with authority
* parameters designate which files to access
* in cap systems, parameters also provide all needed authority - no extra effort needed
* eg, tell cat/cp which files to copy (by pass cap to those files) automatically 
provides it with the authority it needs to do its job

processes dont receive strings with file names, receive caps / file objects
(anti unix >:D )

## Principles

- objects (entities with state and code) (Java objects)
a protected domain with private state + code with well defined entry points

- capabilities (object wrapper) (Java references)
unforgeable reference to an object

Java static fields violates the cap principle (see the scoping remark below).
Java library violates the cap principle.

> can a method only operate on what it receives?!?

- caps are the only way to obtain authority
all rights are represented by capabilities

- by default, newly created objects have no authority

## Language Support

Type safe OO langs are perfect for building object capabilities systems

Capabilities are just references, the lang renders references unforgeable

Type safety renders objects tamper resistant

> issue with assignments?

Scoping rules ensures that code only receives the caps needed to its job

Subset of Java: www.joe-e.org

## Reasoning

Can reason about the flow of caps, by reasoning about the points-to graph

(A -> B) A has a reference to B

> A is dependent of B, B is dependee of A 

Naive modular reasoning: Understand a particular module, assume all others as malicious

Trust and vulnerability: a module is vulnerable to the modules in which it dependents, but trusts the dependees

Defensive consistency: 
if a client satisfies A's preconditions, A must provide correct service to that client.

Eg. if Z misbehaves (violates A's preconditions) A does not need to provide correct
service to Z, but sill must provide correct service to Y

Modular reasoning & defensive consistency
1) check that A respects dependees preconditions.
Corollary: dependees will provide correct service to A

2) check that if the dependents respects A's preconditions, then
A provides correct service to its dependents
(examine source code of A, spec of dependees and use conservative bounds
on behavior of dependents)

## Obj. cap. langs enable local reasoning

Client -> Logger -> log[]

to verify log entries are never deleted
1) check that Logger has the only ref to log[]
2) check that source code of Logger only appends (never deletes or overwrites)

eg, in a very big C (not memory safe, no caps) its impossible to reliably make this reasoning

## Advs of object caps

Caps make principle of least privilege easy
- bundling designation with authority means modules receive only
caps to objects relevant to their legitimate purpose
- POLA follows from good OO design

> no static fields? 

Caps enable modular reasoning
- its just reasoning about pointers, which programmers are already used to
- reachability analysis allows to bound the authority a module can obtain

## Summary and conclusions

- avoid ambient authority
- bundle designation with authority
- capability discipline
- reachability analysis
- use idioms that enable modular reasoning

## Further reading

Mark Miller, Robust Composition: Towards a Unified Approach to Access Control and Concurrency Control, PhD dissertation 2006

The E language www.erights.org
(see also, joe-e, oz-e, twisted python, emily, caperl, capdesk, polaris)

mailing lists: {e-lang, cap-talk}@mail.eros-os.org

Jonathan Rees, A Security Kernel Based on the Lambda Calculus, MIT, 1995

