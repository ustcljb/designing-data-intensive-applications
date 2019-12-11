# Data models and query languages

When talking about data representation, there are multi-level models:

-  As an application developer, you look at the real world and model it in terms of objects or data structures;
-  When you want to store those data structures, you express them in terms of a general-purpose data model, such as JSON or XML documents, tables in a relational database, or a graph model;
-  The engineers who built your database software decided on a way of representing that JSON/XML/relational/graph data in terms of bytes in memory, on disk, or on a network;
-  On yet lower levels, hardware engineers have figured out how to represent bytes in terms of electrical currents, pulses of light, magnetic fields, and more.

## Relational Versus Document Databases

- Document databases are sometimes called *schemaless*, or actually *schema-on-read*, whereas the relational databases are often called *schema-on-write*.
- Schema-on-read is similar to dynamic (runtime) type checking in programming languages, whereas schema-on-write is similar to static (compile-time) type checking.
