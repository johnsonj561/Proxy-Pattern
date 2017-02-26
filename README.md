----------------------------------------------------------------------------------------------------------------
Title: The Proxy Pattern

Sources:
Notes below regarding proxy pattern taken from "Design Patterns - Elements of Reusable Object-Oriented Software"
By Gamma, Helm, Johnson, Vlissides

Example Code provided by Derek Banas:
Tutorial: https://www.youtube.com/watch?v=cHg5bWW4nUI
Source Code: http://www.newthinktank.com/2012/10/proxy-design-pattern-tutorial/

Author: Justin J

Purpose: FAU Object Oriented Software Design Course, Sprint 2017

----------------------------------------------------------------------------------------------------------------

Intent
- provide a surrogate (substitute) or placeholder for another object to control access to it
- controlling access to object is key

Motivation
- defer the full cost of its creation and initialization until we actually need to use it
- Ex) graphics document - opening large raster image can be expensive. Avoid creating all of the expensive objects
  at once when document is opened. Solution would be to use an image proxy, acts as a stand in for the real image. Proxy
  will handle instantiating real image when required
- create expensive objects on demand

Applicability
- applicable whenever there is need for more versatile or sophisticated reference to an obect than a simple pointer
- Several common scenarios:
	- Remote proxy provides local representative for an object in a different address space
	- Virtual proxy creates expensive objects on demand. Image proxy in motivation section is example of virtual proxy
	- Protection proxy controls access to the original object, useful when objects should have different access rights
	- Smart reference is a replacement for a bare pointer that performs additional actions when an object is accessed
		- counting the number of references to real object so that it can be freed automatically when no more references
		- loading a persisten tobject into memory when it's first referenced
		- checking that real object is locked before it's acessed to ensure that no other object can change it
	
Structure
- see ProxyPatternDiagram.png

Participants
- Proxy
	- maintains a reference that lets proxy access real subject. May refer to a Subject if the RealSubject and Subject
	  interfaces are the same
	- provides an interface identical to Subject's so that a proxy can be substituted for real subject
	- controlls access to the real subject and may be responsibly for creating/deleting it
	- other responsibilities depend on the type of proxy:
		- remote proxies are responsible for encoding a request and its arguments, and for sending the encoded request to
		  the real Subject in a different address space
		- virtual proxies may cache additional information about the real subject so that they can postpone accessing it
		- protection proxies check that the caller has access permissions required to perform a request
- Subject
	- defines the common interface for RealSubject and Proxy so that a Proxy can be used anywhere RealSubject is expected
- RealSubject
	- defines the real object that the proxy represents

Collaborations
- Proxy forwards requests to RealSubject when appropriate, depending on the type of proxy

Consequences
- Proxy pattern introduces level of indirection when accessing an object. 
	- Remote Proxy can hide the fact that an object resides in a different address space
	- Virtual Proxy can perform optimizations such as creating an object on demand
	- Both protection proxies and smart references allow additional housekeeping tasks when an object is accessed
- "Copy-on-write" is another optimization that the proxy pattern can hide from client
	- copying large/complicated object can be expensive. This proxy postpones copying cost until confirmed that object has been modified
	- copy-on-write can reduce cost of copying heavyweight subjects significantly

Implementation
- See sample code

Related Patterns
- decorator
- adapter
