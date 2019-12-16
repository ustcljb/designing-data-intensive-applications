# The trouble with distributed systems

##  Faults and partial failures
An individual computer with good software is usually either fully functional or entirely broken, but not something
in between. However, in distributed systems, we are no longer operating in an idealized system model.

-  Cloud computing versus supercomputing
   -  At one end of the scale is the field of high-performance computing (HPC). Supercomputers with thousands of CPUs are typically used for computationally intensive scientific computing tasks, such as weather forecasting or molecular dynamics (simulating the movement of atoms and molecules).
   - In a supercomputer, a job typically checkpoints the state of its computation to durable storage from time to time.
   -  At the other extreme is cloud computing, which is not very well defined but is often associated with multi-tenant datacenters, commodity computers connected with an IP network (often Ethernet), elastic/on-demand resource allocation, and
metered billing. (This type is our emphysis in this chapter).

It would be unwise to assume that faults are rare and simply hope for the best. It is important to consider a wide range of possible faults—even fairly unlikely ones—and to artificially create such situations in your testing environment to see what happens.

## Unreliable networks
The internet and most internal networks in datacenters (often Ethernet) are *asynchronous packet networks*.

In this kind of network, one node can send a message (a packet) to another node, but the network gives no guarantees as to when it will arrive, or whether it will arrive at all.

The usual way of handling this issue is a *timeout*. However, it is really hard to determine what is a suitable timeout.

### Synchronous versus asynchronous networks
- Can we not simply make network delays predictable?
   - Ethernet and IP are packet-switched protocols, which suffer from queueing and thus unbounded delays in the network. These protocols do not have the concept of a circuit.
   - Why do datacenter networks and the internet use packet switching? The answer is that they are optimized for *bursty traffic*.
   - Requesting a web page, sending an email, or transferring a file doesn’t have any particular bandwidth requirement—we just want it to complete as quickly as possible.
   - Variable delays in networks are not a law of nature, but simply the result of a cost/benefit trade-off.
   
## Unreliable clocks
Modern computers have at least two different kinds of clocks: a *time-of-day clock* and a *monotonic clock*. Although they both measure time, it is important to distinguish the two, since they serve different purposes.

-  A time-of-day clock does what you intuitively expect of a clock: it returns the current date and time according to some calendar.
-  A monotonic clock is suitable for measuring a duration (time interval), such as a timeout or a service’s response time.
-  It is possible to achieve very good clock accuracy if you care about it sufficiently to invest significant resources. Such accuracy can be achieved using GPS receivers, the Precision Time Protocol (PTP), and careful deployment and monitoring.

## Byzantine faults
Distributed systems problems become much harder if there is a risk that nodes may “lie” (send arbitrary faulty or corrupted responses)—for example, if a node may claim to have received a particular message when in fact it didn’t. Such behavior is known as a *Byzantine fault*, and the problem of reaching consensus in this untrusting environment is known as the *Byzantine Generals Problem*.

- The *Byzantine fault-tolerant* systems are usually used in the following applications:
   -  Flight control systems since the data in a computer’s memory or CPU register could become corrupted by radiation in aerospace environment.
   -  Peer-to-peer networks like Bitcoin and other blockchains.
