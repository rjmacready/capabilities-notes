# Capability-based security; how does it work?

from http://lambda-the-ultimate.org/node/3930

Capabilities, as I understand them, are essentially keys. 
In the simplest model, a program presents a cap to the OS with a request to "do something", and the OS checks the cap for validity. 
If it's valid, (has been issued to the requestor, hasn't been revoked or expired), the OS carries out the request.
> this doesnt sound very correct
> does it work like certificates do?

Now, these things are divisible and delegatable. 
If a program (like a command shell interpreter) has a cap that represents the authorities that the OS has issued to a particular user, 
it can create a cap that represents the user's authority over a particular file or directory or device, 
and pass that on to another program (like a compiler) when the user invokes a program supplying that filename, directory name, 
or device name (like the file to compile or the directory to create a file and write the compiled code in) as an argument. 
The idea is that each program should run with only the priveleges it actually needs.

Implication: programs no longer parse their own command lines, or at least not the parts of them referring to capabilities. 
Something to which the user's caps have been delegated has to do that, so as to provide the programs directly with the caps they need.

They are also replicable. That is, a program can communicate a cap to another program without itself giving up that cap. 
Thus, you can delegate the ability to run a camera or something to a remote program across the network, 
without giving up the ability of local programs to use that camera.

In fact, the "ambient authority" of machine code running directly on hardware is what the OS divides and replicates to provide the 
set of capabilities that represent the user's authority - or a particular program's authority, or etc.

The OS must know who (or what) owns a capability; when something presents a capability that was issued to something else, 
the request should be refused. Otherwise, the system could be breached by a program that "guessed" the binary pattern of a 
capability issued to something else. This means that replicating, delegating, or subdividing a capability can only be 
done via calls to the OS, so it can keep a current picture of who owns what.

Okay, so far so good. That's the theory as I understand it. But the theory doesn't in general match up with the security 
claims made for the technology, or at least those claims imply the existence of additional infrastructure, so I feel like 
I must be missing something.

In the first place, presenting a capability to the OS must also give the OS identity information about what's presenting the cap, 
so that it can check its ownership and make sure that cap actually belongs to the presenter. But every time a program starts, 
it has a new process ID. If the OS keeps track of the file from which the program code is loaded and assigns the compiled-in caps 
to the resulting process ID, then it can be defeated by putting different code into the file in order to hijack the caps.

So this seems to imply a requirement I've never seen discussed; that files with compiled-in capabilities have to be restricted 
such that no user whose own capabilities are not a superset of those compiled into the file can write it. IOW, if a program has 
the capability to write /etc/passwd then no user who does not have a capability to write /etc/passwd can be allowed to write to 
the program file.

A user must have all the capabilities required by any software he or she is installing or modifying. In principle each user 
priveleged in a certain domain can install software that has his or her priveleges in that domain. But it could easily happen 
that even where all the capabilities exist, no particular user would have the capabilities to install something.

So in practice, it seems like you'd always need an omnipriveleged root account to do some kinds of software installation and maintenance. 
Therefore it seems strange to me that the omnipriveleged root account is claimed not to be needed.

(...)
