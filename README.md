# Neo4j Graph Database Design Document
###### By Ervin Mamutov

### The Spec
>You are required to design and prototype a Neo4j database for use in a timetabling system for a third level institute like GMIT. The database should store information about student groups, classrooms, lecturers, and work hours just like the currently used timetabling system at GMIT.

## Graph Databases
### What are Graph Databases?
**A graph** [1] is composed of two elements, a node and a relationship. A node represents an entity and a relationship represents how these entities are associated.

**A database** [2] is a collection of information that is organized so that it can be easily accessed, managed and updated.

**A graph database** [2] is a type of NoSQL database that uses graph theory to store, map and query relationships. Graph databases are basically collections of nodes and edges, where each node represents an entity, and each edge represents a connection between nodes.

Relationships take first priority in Graph Databases. 

### Why would a Graph Database be useful for a timetabling system?
To understand why a graph database would be useful for a timetabling system, we must first understand what a timetabling system is.

A timetabling system is a method of organizing student groups and lecturers to provide an organized schedule for modules, rooms and work times. A timetabling system cares more about the relationship between pieces of data rather than the data it's self.

As mentioned above, relationships take first priority in graph databases. This is why a graph database would really compliment a timetabling system. Using a graph database to create an effective graph modeled network of nodes and their relationships, the database can be efficiently queried for very specific information.

Timetabling schedules are subject to change quite frequently during the start of a semester, so the flexibility of graph databases can accommodate to these changes. Data teams can add to or update the existing graph structure without endangering current functionality [1].

### What is Neo4j?
Neo4j is the world's leading graph database.

Neo4j is an open-source NoSQL graph database implemented in Java and Scala. Development started in 2003 and has been publicly available since 2007. Since Neo4j is open-source, the source code is available on GitHub. Neo4j is used by a large amount of companies and organizations in almost all industries. Different uses for Neo4j include matchmaking, network management, software analytics, scientific research, routing, organizational management and more. [3]

### How does Neo4j work?
Neo4j, upon installation, provides a server that runs on localhost:7474. This server uses JAXRS and Jersey, combined with a Jetty web server [6] to provide a REST based web-service that a user can use to interact with or get a visual representation of the graph database by using Neo4j's Cypher query language.

Cypher query language, developed by Neo Technology for Neo4j, is used to query the database to perform CRUD operations. Cypher is based on the property graph model, which in addition to the standard graph elements of nodes and edges (relationships) adds labels and properties as concepts [4].

Neo4j stores everything in the form of either an edge, a node or an attribute. Each node and edge can have any number of properties. Both the nodes and edges can be labeled. Labels can be used to narrow searches [5].

## Implementation

### Planning
During my planning phase I examined what the problem was and broke it into smaller problems. 

My first problem was to decide what kind of nodes and relationships I was going to use, so looking at my current timetable, I drew up some nodes and relationships to get an understanding of how it would all work. 

My next problem was to decide how much data was I going to add to this database. I didn't want to add too much data instead I wanted to create a simple prototype for my semester's timetable but in a way that could be re-used to implement GMIT's whole timetabling system. 

My final problem was how will I retrieve this data and how will I inject this data into Neo4j. I decided to use GMIT web timetables [7] to retrieve the data and used a .txt file with query commands [8] to populate my database.

After executing several model plans, this was the model that worked.
### Nodes
   * **Course**
    
     The course node represents a course in the college. This is the root node. This node has a property "name". My prototype contains 1 course node, but ideally there would be *n* number of course nodes, (n) depending on how many courses are available in the college.
   * **Semester**
      
     The semester node represents a semester in the course. This node has a property "name". My prototype contains 1 semester node, but ideally there would be *2(n)* number of nodes for each course, (n) depending on how many years that course has.
   * **Module**
      
     The module node represents a module in the course. This node has a property "name". My prototype contains 6 module nodes, one for every module studied during my semester. Ideally there would be *n* module nodes for every semester node, (n) depending on how many modules there are in a semester.
   * **Staff**
      
     The staff node represents the lecturer for a module. This node has a property "name". My prototype contains 6 staff nodes, one/two for every module. Ideally there would be *n* staff nodes, (n) depending on how many lecturers are employed in the college.
   * **Room**
      
      The room node represents a room in the college. This node has a property "name". My prototype 15 room nodes that are used by modules. Ideally there would be *n* room nodes, (n) depending on how many class rooms there are in the college.
      
    * **Day**
      
      The day node represents a day in a working week. This node has a property "name". My prototype has 5 day nodes for each working day of the week. Ideally there should only have 5 day nodes for the work days of the week.

These are the nodes I have implemented in my prototype but ideally if I were to implement the whole timetabling system, I would need nodes for Department and Campus.
### Relationships
   * **HAS_SEMESTER**
      
      The has_semester relationship connects a **Course** node to a **Semester** node. The direction of the relationship is: *(:Course)-[:HAS_SEMESTER]->(:Semester)*. This relationship has no properties.
   * **HAS_MODULE**
      
      The has_module relationship connects a **Semester** node to a **Module** node. The direction of the relationship is: *(:Semester)-[:HAS_MODULE]->(:Module)*. This relationship has no properties.
   * **TEACHES**
    
      The teaches relationship connects a **Staff** node to a **Module** node. The direction of the relationship is: *(:Staff)-[:TEACHES]->(:Module)*. This relationship has no properties.
   * **THOUGHT_IN**
    
      The thought_in relationship connects a **Module** node to a **Room** node. The direction of the relationship is: *(:Module)-[:THOUGHT_IN]->(:Room)*. This relationship has 4 properties: duration, type, day and group. Duration refers to how long the lesson is. Type refers to the type of lesson, practical or lecture. Day refers to the day that lesson is on. Group refers to which group the lesson is being thought to.
   * **THOUGHT_ON**
    
      The thought_on relationship connects a **Room** node to a **Day** node. The direction of the relationship is: *(:Room)-[:THOUGHT_ON]->(:Day)*. This relationship has 2 properties: time and group. Time refers to the time the room is being used on the day and group refers to which group is occupying that room.
    
I added properties to the last 2 relationships in the list above to better query the database. I ran into a problem where when I wanted to see what modules were thought on a Monday, it would show me all modules connected to the rooms that are connected to Monday, i.e if Graph Theory was using room 0995 on a Friday, that relationship would show up on my Monday query because Software Testing was using room 0995 on a Monday. 
    
This issue also happened when I used Time as a node, so I decided to make Time a relationship property for THOUGHT_ON and day a relationship property for THOUGHT_IN.
    
Ideally if I were to add the department and campus nodes, I would need two more relationships to connect campus to department and department to course.
### Diagram
    
   * **Design Diagram**
      
      ![Design Diagram](https://github.com/ImErvin/Neo4jTimetable/blob/master/images/DesignDiagram.png?raw=true "Design Diagram")
   * **Design to Graph Diagram**
   
      ![Design to Graph Diagram](https://github.com/ImErvin/Neo4jTimetable/blob/master/images/DesignGraphDiagram.png?raw=true "Design to Graph Diagram")
   * **Graph Database Diagram**
   
      ![Graph Database Diagram](https://github.com/ImErvin/Neo4jTimetable/blob/master/images/DatabaseDiagram.png?raw=true "Graph Database Diagram")
### How the data was stripped
I simply extracted the data from GMIT's web timetables [7] by using their UI to return my current semesters timetable in a list format and used inspect element to retrieve the HTML code. Once extracted I used Sublime IDE to refactor and remove any HTML and was left with raw data. I used this raw data to create a txt file of CREATE queries [8] for nodes and relationships. Pasted the contents of the txt file into the console and tested by querying the database.

## CRUD Queries
A simple API on how to work with my prototype.
### Create
**General Queries**

To create a new node use:
```cypher
CREATE (a:NodeLabel {name:"Name Value"})
```
   * a refers to the name of the node.
   * NodeLabel refers to the Node label. Use node labels mentioned above (under nodes). E.g: Use :Course if creating a new course.
   * name refers to the single property every node has in my prototype.
   
To create a new relationship use:
```cypher
CREATE (a)-[:RELATIONSHIP_LABEL {propertyName1:"property value1", propertyName2:"property value2"}]->(b)
```
   * a and b refer to names of nodes.
   * RELATIONSHIP_LABEL refers to the relationship label. Use relationship labels mentioned above (under relationships). E.g: use :HAS_SEMESTER if connecting a course node to a semester node.
   * propertyName refers to any property a relationship may have. Use this if the relationship you're creating has properties. E.g: when using :THOUGHT_ON , You will need to specify the time and group properties.

**Interesting Queries**

Keep in mind you could create several relationships at once:
```cypher
CREATE (sdst)-[:THOUGHT_IN {day:mon.name, type:"P", duration:"2", group:"A"}]->(sd0481)-[:THOUGHT_ON {time:"16:00", group:"A"}]->(mon)
```
### Retrieve
**General Queries**

To retrieve all of the nodes and relationships use:
```cypher
MATCH (x)
RETURN x;
```
To retrieve all of the nodes of one type use:
```cypher
MATCH (x:NodeLabel)
RETURN x;
```
   * x refers to the type of node you want to retrieve. E.g: If you want all the Modules you'd use (x:Module).
To retrieve all of the relationships of one type use:
```cypher
MATCH (a)-[r:RELATIONSHIP_LABEL]->(b) 
RETURN a,b,r;
```
   * a and b refer to nodes. You can specify which nodes by adding a :NodeLabel after the a or b. E.g: (a:Module) (b:Room).
   * r:RELATIONSHIP_LABEL refers to the relationship label you want to retrieve. E.g: If you want all to see all the rooms being used by Modules you'd use [r:THOUGHT_IN].

**Interesting Queries**

To retrieve all of the rooms used and modules being thought on a specific day use (replace "MONDAY" with any other day):
```cypher
match (a)-[r:THOUGHT_IN]->(b)-[t:THOUGHT_ON]->(c) 
WHERE c.name="MONDAY" and r.day = c.name
RETURN a,b,c,r,t;
```

To retrieve all of the 1 hour lessons during the week use:

### Update
...

### Delete
...

## Conclusion
...

## References
[1] Neo4j - [why graph databases?](https://neo4j.com/why-graph-databases/)

[2] TechTarget/SearchSQLServer - [databases](http://searchsqlserver.techtarget.com/definition/database)

[3] Neo4j - [graph databases](https://neo4j.com/developer/graph-database/)

[4] Wikipedia - [cypher query language](https://en.wikipedia.org/wiki/Cypher_Query_Language)

[5] Wikipedia - [neo4j](https://en.wikipedia.org/wiki/Neo4j#Neo_technology)

[6] Google Groups - [frameworks used](https://groups.google.com/forum/#!topic/neo4j/dh1Y11OqNb0)

[7] GMIT - [web timetables](http://timetable.gmit.ie/)

[8] GitHub - [neo4jscript.txt](https://github.com/ImErvin/Neo4jTimetable/blob/master/NEO4JSCRIPT.txt)
