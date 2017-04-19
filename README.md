# Graph Theory Project 
# By Andrew Carroll


<h1> Introduction </h1>
<p> Final Year Student studying Software Development in GMIT, the following README is the documentation for the module <b> Graph Theory </b> taught in the final year of the level 7 programme of Computing & Software Development by lecturer Ian Mcloughlin. Graph Theory is used through out computation and the following project was assigned to demonstrate this.</p>

<h2> Project Brief </h2>
<p> You are required to design and prototype a Neo4j database for use
in a timetabling system for a third level institute like GMIT. The database
should store information about student groups, classrooms, lecturers, and
work hours – just like the currently used timetabling system at GMIT </p>

<h2> What is a Graph Database? </h2> 
<p> This is a database that stores data using graph structures and logical queries.  Graph databases are based on graph theory and use nodes & edges (relationships). The relationships allow data in the store to be linked together directly. When trying to retrieve data from a graph database it needs a query language an example of this would be <b> Cypher </b>.   </p>

<h2> Cypher </h2> 
<p> A very simple and powerful graph query language that was developed by Neo Technology for its graph database <b> Neo4j </b> it is an SQL-Inspired language for describing patterns in graphs visually using an ascii-art syntax. </p>

<h2> Neo4j </h2> 
<p> A software program developed by Neo Technology to deal with the management system of graph databases. according to db-engines.com Neo4j it is described as the most popular graph database system.The Neo4j Software is  achieved in Java and accessible from software written in other languages using the Cypher Query Language. To start using the  Neo4j software visit https://neo4j.com/download/ and select the download for the community edition for Individuals. After the download and installation process you can choose your password and begin working with Neo4j to build databases.  </p>

<h2> What is a Node? </h2> 
<p> In graph theory a <b> node </b> or <b> vertex </b>  is the fundamental unit of which graphs are formed </p>

<h2> What is a relationship? </h2> 
<p> In Neo4j relationships between two nodes are directional meaning that they have a connection. An example of this in my project would be a <b> Lecturer </b> node and a <b> Room </b> node, E.g (LecturerNode: "John Sample")-[TEACHES_IN_ROOM]->(RoomNode:"G0994"), the relationship between the room number and the lecturer is defined by TEACHES_IN_ROOM.   </p>

<h1> Design </h1>

<h3> Building The Database</h3>
<p> Before diving into create the database straight away, I took out a piece of paper and began doing rough ideas for how I was going to approach the set-up for the database. I decided to base the timetable on Semester 2 of 3rd Year Software Development in GMIT taking from http://timetable.gmit.ie/sws1617/(S(v0rxst45trrrbo55zvzqwgia))/showtimetable.aspx From here I began writing what nodes I needed. So I chose the following to work with:
                                     
                                     
                                      1.Rooms
                                      2.Lecturers
                                      3.Groups
                                      4.Subjects
                                      5.TimeSessions
                                      
  After selecting the nodes for the database, I started to fetch information from the http://timetable.gmit.ie website from the category Rooms here I opened the view page source and copy and pasted the information I needed for all the rooms I would be using. I copied the information into Notepad++ and from here I began removing any unnecessary tags and rooms that were not  needed for my database. After all tags were removed and I was left with the rooms I needed I opened up Neo4j and began to create the <b> Rooms </b> node by preforming the following cypher command for each room: 
  
                               E.G   CREATE (Room:Rooms{RoomNo:"G0436 CR5", Capacity: "20"})

To which I gave them two properties  RoomNo & Capacity. This was done for each of the room nodes I created. </p>

<h4> Lecturer Node </h4>
<p> When Creating the nodes for Lecturers it wasnt overally difficult as I knew all the lecturers for semester 2 in 3rd Year So i just began creating a cypher command for each of them as it only 5 lines long. The only property I gave them was their Name Value as it was all I needed, I wrote the cypher command in the following way : </p>
                          
                              E.G  CREATE (IanMcLoughlin:Lecturers{Name:"Ian McLoughlin"})

<h4> Groups </h4>
<p> Groups was another very simple command as there are only 3 groups in our year so it was very quick to create the nodes for this: </p>
                                 
                                  CREATE (GroupA:Groups{name:"Group A"})
                                  CREATE (GroupB:Groups{name:"Group B"})
                                  CREATE (GroupC:Groups{name:"Group C"})
 
 <h4> Subjects </h4> 
 <p> Next I looked at creating the Subjects/Modules that I do in semester 2 as these were one of the nodes that I needed so I began creating the cypher commands for them giving them the label name <b> Subject </b> and the Properties being the Module which I wrote in the following way: </p>
                                  
                                 
                                 
                                 E.G CREATE (GraphTheory:Subject{Name:"Graph Theory"})

<h4> TimeSessions </h4> 
<p> This was the last node I created in the database with the label name <b> TimeSessions </b> and each property created with a one hour interval between them e.g 9am-10am all the way up to 6pm: </p>

                                
                             E.G   CREATE (Space1:TimeSession{name:"9am-10Am"})

<h3> Relationships </h3>
<p> This process of the database took a while to figure out and alot of research  but once I got my head around it, it began to get a bit easier and I was able to work through it. The neo4j documentation site https://neo4j.com/docs/ is very helpful with commands and instruction on how to create nodes and relationships. The first relationship I decided to test out was the between the lecturers and the Subjects:                 

                              MATCH (a:Lecturers),(b:Subjects)
                              WHERE a.Name = 'Ian McLoughlin' AND b.Name = 'Graph Theory'
                              CREATE (a)-[r:Teaches]->(b)
                              RETURN a,b,r
    
    The command matches the label lecturers and subjects together and finds the properties (Ian Mcloughlin) & (Graph Theory) and links a  relationship between them by calling it [TEACHES] </p>
    
<p> I saw that the command worked perfectly, So I did this for the rest of the remaining nodes for lecturers and subjects, after which I began creating a relationship between the Subjects and the Groups which were wrote in the following manner : </p>

                              MATCH (a:Module),(b:Groups)
                              WHERE a.Name = 'Server Side Rad' AND b.name = 'Group A'
                              CREATE (a)-[r:TO]->(b)
                              RETURN a,b,r

<p> The next relationship I linked together was between the rooms and the Time sessions of one hour intervals </p>
                                 
                               MATCH (a:Rooms),(b:TimeSession)
                               WHERE a.RoomNo = 'PF18' AND b.name = '9am-10Am'
                               CREATE (a)-[r:AT]->(b)
                               RETURN a,b,r

<p> I finally tried to link the different labs and lectures together with the appropiate room and modules for each one, both commands can be seen below here : </p>

                              MATCH (a:Subject),(b:TimeSession)
                              WHERE a.Name = 'Server Side Rad' AND b.RoomNo = 'G0436 CR5'
                              CREATE (a)-[r:IN_LAB]->(b)
                              RETURN a,b,r
                              
                              
                              MATCH (a:Subject),(b:TimeSession)
                              WHERE a.Name = 'Server Side Rad' AND b.RoomNo = 'G0436'
                              CREATE (a)-[r:IN_LECTURE]->(b)
                              RETURN a,b,r

<h2> Conclusion </h2> 
<p> In conclusion with this project and after using neo4j I can see it is a very strong tool and application for when creating a graphical database, even though I only touched it on a very small basis, I can see the benefit of using it when creating a graph. The visual aspects of it I found were very helpful and it was easy to understand the relationships between each node once they were created and I could see the linkages between them for myself very cleary. </p>

<h2> References </h2>


<p>
                    https://neo4j.com/docs - Neo4j Documentation
                    https://en.wikipedia.org/wiki/Graph_theory - Wikipedia about Graph Theory
                    https://www.tutorialspoint.com/graph_theory/ - Notes On Graph Theory
                    http://timetable.gmit.ie/ - TImetable For GMIT
                    http://timetable.gmit.ie/sws1617/(S(hfe5u2ftbt5paznk12urhs55))/default.aspx - Room Selection For GMIT
                    http://www.tutorialspoint.com/neo4j/ - Notes on Neo4j
                    PDF Slides Provided by lecturer - Ian Mcloughlin from https://learnonline.gmit.ie/ -
                    https://notepad-plus-plus.org/download/v7.3.3.html - Used To Help Remove Tags 
</p>
