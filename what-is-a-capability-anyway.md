# What is a Capability, Anyway?

from http://www.eros-os.org/essays/capintro.html

## What a Capability Is

Programming Semantics for Multiprogrammed Computations, Dennis and Van Horn 1966

Suppose we design a computer system so that in order to access an object, 
a program must have a special token. This token designates an object 
and gives the program the authority to perform a specific set of actions (such as reading or writing) on that object

> a token used when calling a method? and the OS must figure out if it's allowable or not?

capabilities do not care who uses them
capabilities can be handed as a car key is handed. the car doesnt know who's using the key, only knows about the key

two capabilities can designate the same object but authorize different sets of actions
One program might hold a read-only capability to a file while another holds a read-write capability to the same file.

As with keys, you can give me a capability to a box full of other capabilities.

* Capabilities can be delegated

* Capabilities can be copied

If it comes down to desperate measures, you can change the locks on the car, 
making all of the keys useless. This can be done with capabilities too; it is known as severing an object, 
which has the effect of rescinding all capabilities. A rescinded capability conveys no authority to do anything at all.

In order to be useful, capabilities must be unforgeable.

## Capability-Based Computer Systems

In a capability-based computer system, all access to objects is done through capabilities, 
and capabilities provide the only means of accessing objects. 
In such a system, every program holds a set of capabilities.

The only way to obtain capabilities is to have them granted to you as a result of some communication.

> by a parent process / OS / User ?

The goal is to make the set of capabilities held by each program as specific and as small as possible, 
because a program cannot abuse authority it does not have. This is known as the _principle of least privilege_.

Aside from the fact that UNIX file descriptors carry some associated information about the current file offset, 
a UNIX file descriptor is essentially a capability.

### Writing Things Down, Universal Persistence

The solution is to simply remember everything. Every five minutes, write down all of the things the computer is working on.

> in a VM-ish environment, save to image?

You might think this is an inefficient approach. In practice it's actually faster than file systems and requires less code.

> !?...
> dont we still have the "chicken and egg problem"?  Where does the first program of the first boot gets its capabilities from.

## You Can't Get There From Here

### Privileged Programs

The right solution is to explicitly give the program access to the password file, 
and not leave the password program lying around in a file system where it can be overwritten. 
In a capability system, you simply give the program a capability to the password database and let it run forever. 
You then give out a capability that lets people run the password program but does not let them read or modify that program.

> Isnt this a consequence of ACL's granularity? or assigning privileges to users instead of programs?

### Access Restriction

> This seems to be a issue that ACL doesnt handle (sort of "lack of scope")

Java solves this problem by ad-hoc restrictions.

> How so?

### Collaboration

Suppose I have a secret program that is very valuable.

You have some very secret data concerning the results of a new drug trial. 
You need to run my program, but you're not willing to show me the data.

In a capability system, we can set things up so that 
you can run the program without being able to see the code, but you are able to control what authority my program is given. 
Since you control the authorities, you can ensure that my program will not tell me your secrets. 
Since you cannot gain access to the binary code, you cannot steal the program

## Revoking Access Selectively

One problem with capabilities is that there is no way to say "Remove all of Fred's access to this object."

> why not? we can invalidate a certificate, cant we make something like this?
> maybe we can attach a validity to a capability (0: needs constant validation; x: validation in x <time-out>; +Inf: allways valid)

Revoking access to a particular object is exactly like the special compartment problem. 
Either you build a special compartment in advance, or there really isn't any good way to prevent the user from absconding with the data.

> remember, capability isnt aware of identities ... maybe it should? if it were, it probably wouldnt be correct to allow them to be copied / interchanged.

