# Encoding and evolution

In order for the system to continue running smoothly, we need to maintain compatibility in both directions:
-  *Backward compatibility*: newer code can read data that was written by older code.
-  *Forward compatibility*: older code can read data that was written by newer code.
-  Backward compatibility is normally not hard to achieve: as author of the newer code, you know the format of data written 
by older code, and so you can explicitly handle it

In this chapter the author explains several formats for encoding data, including JSON, XML, Protocol Buffers, Thrift, and Avro.
And most importantly, how *schema evolution* is achieved.

## Modes of Dataflow

-  Compatibility is a relationship between one process that encodes the data, and another process that decodes it. Data can flow via different processes:
   -  via databases
   -  via service calls
   -  via asynchronous message passing
   

