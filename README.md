# Neo4j Graph Database Design Document
###### By Ervin Mamutov

### The Spec
>You are required to design and prototype a Neo4j database for use
in a timetabling system for a third level institute like GMIT. The database
should store information about student groups, classrooms, lecturers, and
work hours – just like the currently used timetabling system at GMIT.

### Graph Databases
#### What are Graph Databases?
**A graph** [1] is composed of two elements, a node and a relationship. A node 
represents an entity and a relationship represents how these entities are associated.

**A database** [2] is a collection of information that is organized so that it can be 
easily accessed, managed and updated.

**A graph database** [2] is a type of NoSQL database that uses graph theory to store, map 
and query relationships. Graph databases are basically collections of nodes and edges, 
where each node represents an entity, and each edge represents a connection between nodes.

Relationships take first priority in Graph Databases. 

#### Why would a Graph Database be useful for a timetabling system?
To understand why a graph database would be useful for a timetabling system, we must
first understand what a timetabling system is.

A timetabling system is a method of organizing student groups and lecturers to provide
an organized schedule for modules, rooms and work times. A timetabling system cares more 
about the relationship between pieces of data rather than the data it's self.

As mentioned above, relationships take first priority in graph databases. This is why
a graph database would really compliment a timetabling system. Using a graph database
to create an effective graph modeled network of nodes and thier relationships, the database
can be efficiently queried for very specific information.

Timetabling schedules are subject to change quite frequently during the start of a 
semester, so the flexibility of graph databases can accomidate to these changes. Data teams 
can add to or update the existing graph structure without endangering current functionality [1].

#### What is Neo4j?
Neo4j is the world's leading graph database.

Neo4j is an open-source NoSQL graph database implemented in Java and Scala. Development started 
in 2003 and has been publicly available since 2007. Since Neo4j is open-source, the source code 
is available on GitHub. Neo4j is used by a large amount of companies and organizations in almost
all industries. Different uses for Neo4j include matchmaking, network management, software analytics, 
scientific research, routing, organizational management and more. [3]

#### How does Neo4j work?
...

## Implementation

#### Nodes
...

#### Relationships
...

#### Diagram
...

#### How the data was stripped
...

## CRUD Queries

#### Create
...

#### Retrieve
...

#### Update
...

#### Delete
...

## Conclusion
...

## References
[1] Neo4j - [why graph databases?](https://neo4j.com/why-graph-databases/)
[2] TechTarget/SearchSQLServer - [databases](http://searchsqlserver.techtarget.com/definition/database)
[3] Neo4j - [graph databases](https://neo4j.com/developer/graph-database/)
