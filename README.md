# Analysis of a criminal network
This repository contains code and analysis for the project **“Analysis of a Criminal Network”**, based on the `moreno_crime` dataset from the Koblenz Network Collection (KONECT). The project explores the structure and dynamics of a bipartite, undirected, and unweighted network connecting individuals to specific crimes.


## Dataset

- **Source:** KONECT – The Koblenz Network Collection. Jérôme Kunegis. In Proc. Int. Conf. on World Wide Web Companion, 2013.
  - **Person:** Each node represents an individual involved in at least one crime (as suspect, victim, or witness).
  - **Crime:** Each node represents a specific criminal case.
- **Edges:** Link a person to a crime in which they participated.

Additional attributes enrich the analysis:
- **Name** and **gender** of each person.
- **Role** in each crime: Suspect, Victim, Witness, or combinations.

## Project Goals

- Identify which people are involved in which crimes and with whom they have overlapped.
- Study recurring collaborations between suspects.
- Detect criminal groups or communities.
- Analyze the role of each individual to differentiate types of participation (suspect, victim, witness).

## Data Preparation

- Loaded and processed the `out.moreno_crime_crime` file (person_id and crime_id).
- Imported supplementary files for names, gender, and roles.
- Linked all attributes to person nodes for advanced analysis by gender, role, or individual identification.

## Network Construction & Visualization

To build the bipartite graph connecting people and the crimes in which they were involved, the initial bipartite graph (B) was created and each node was assigned its bipartite type (person or crime), as shown in the following code: 

```python
B = nx.Graph()
B.add_edges_from(edges)
for node in B.nodes():
    B.nodes[node]['bipartite'] = 'person' if node <= 829 else 'crime'
```
Then, the graph was enriched with data from the files (name, gender, and role) to enable further qualitative analysis.
Next, a projection of the bipartite network was applied to obtain a graph where the nodes are only people, and edges connect two people if they have been involved in at least one common crime (co-participation):

```python
persons = [n for n, d in B.nodes(data=True) if d['bipartite'] == 'person']
G = nx.bipartite.weighted_projected_graph(B, persons)
```
To achieve an informative and clear visualization, the following criteria were applied:

- Nodes were colored by gender (light blue for male, light red for female, and gray for unknown). This allows observation of segregation in certain areas of the network.
- The spring_layout from NetworkX (force-directed) was used to position the nodes according to the tension of the connections. This helps group highly connected nodes and separates peripheral ones.
- Attributes such as name and role were kept in the graph as metadata for further use with Gephi or other tools.

This was the result:
<img width="314" height="244" alt="image" src="https://github.com/user-attachments/assets/1346d876-1f67-4251-afe3-f54fbf747908" />
(Translation Catalan - English: "Xarxa de co-implicació en crims entre persones" means "Co-involvement network in crimes between individuals".)

We observe different groupings of people related to crimes. There is no clear segregation between genders.
On the other hand, by creating the same graph with Gephi and using the same colors with the “Force Atlas” layout, the result was as follows:

<img width="285" height="264" alt="image" src="https://github.com/user-attachments/assets/ce3e990f-f095-486f-a060-393fc47cc342" />

## Analysis

To identify relevant individuals within the network, several centrality metrics were applied:
- **Degree centrality:** Indicates how many connections each person has, i.e., with how many other people they have shared a crime.
- **Eigenvector centrality:** Measures not only the number of connections, but also the importance of the people they are connected to. This is useful for detecting influential actors within the network.
- **Betweenness centrality:** Identifies nodes that act as bridges between different parts of the network. These are crucial for the circulation of information or coordination.

## Results
<img width="189" height="211" alt="image" src="https://github.com/user-attachments/assets/5ae6edb5-a2e8-416b-8358-90f14be551b7" />
(Translation Catalan - English: "Grau" means "Degree", "Centralitat de vector propi" means "Eigenvector centrality" and "Centralitat de betweenness" means "Betweenness centrality".)

- Using degree centrality, Katz Luella (Suspect) with 51 connections is likely a key figure within the criminal organization, being involved in many cases with other suspects or victims.
- Abrams Chad (Victim) with 48 connections: It is unusual for a victim to have so many connections; this could indicate recurring victimization (collective harassment), a mixed role not correctly reflected, or systemic revictimization.
- With eigenvector centrality, Katz Luella stands out again, but Smith Michael Thomas, Steiner Catherine, and Binder Errol also appear. These individuals are well connected to other influential figures, possibly within the core of the criminal network, giving them the capacity to impact through their connections.
- With betweenness centrality, Willis Jenny (Witness) has the highest value, making their role as a witness highly relevant. This person may connect groups that would not otherwise interact or be a key witness in several dispersed cases. Slattery Maurice, Bendix Jerry Lee, and Steiner Catherine are also likely intermediaries between criminal groups or recurring collaborators. Abrams Chad appears here as well, possibly because, as a victim, they have connections with multiple groups.
- Thus, Katz Luella is not only the most connected but also influential and central, suggesting they may be a leader or central figure in the network.

The degree distribution was also analyzed to see the number of connections per person:
<img width="328" height="241" alt="image" src="https://github.com/user-attachments/assets/1785ac38-0ccb-41cb-9c8d-d4b591356b16" />
(Translation Catalan - English: "Distribució del grau (n connexions per persona)" means "Degree distribution (number of connexions per person)", "Nombre de persones" means "Number of persons" and "Grau" means "Degree").

- Most people have a degree between 0 and 5, meaning most individuals are only connected to a few crimes or a few other people, with many local, loosely connected clusters. Some people have a degree of 30, indicating highly connected hubs.

To analyze the network's morphology, connectivity and communities were studied:

<img width="216" height="55" alt="image" src="https://github.com/user-attachments/assets/ebadc5a7-032d-47a2-8454-f868e101e3a5" />


- The network is not fully interconnected; not every person can reach every other person. This makes sense, as many cases are isolated and do not share involved individuals with other crimes.
- There are 20 disjoint subsets of nodes, and the largest component contains 754 people, indicating strong interrelation within the main component.


<img width="365" height="113" alt="image" src="https://github.com/user-attachments/assets/6a740de1-6e47-4af2-b0ea-8d29140fd785" />


The Louvain community detection algorithm identified 47 densely interconnected groups. For example, community 1 has 79 members, which may indicate a gang, organized criminal group, or a set of related crimes.

Other interesting interactions include relationships where two people share more than three crimes, indicating repeated collaboration. This was calculated by filtering edge weights greater than 3.

<img width="277" height="101" alt="image" src="https://github.com/user-attachments/assets/d3ad20dd-8e6f-4a88-aeea-ff873fa0d57f" />

Additionally, analyzing more insights regarding the gender, we obtain the following:

<img width="227" height="313" alt="image" src="https://github.com/user-attachments/assets/38ea98a3-1a54-44c7-affb-55667c8ab598" />


- The analysis shows the network is dominated by men (558) compared to women (271), which may reflect male overrepresentation in roles associated with criminal cases.
- Regarding roles, most are suspects (373) and victims (317), with fewer witnesses (117) and a small group with both roles (22). This distribution indicates a complex structure where some individuals may be involved in multiple aspects of crimes.
- Several communities are composed exclusively of a single role, especially victims (communities 23, 37, 44) and suspects (communities 9, 19, 22, 27, 31, 39, 40), as well as a witness community (9). This segregation by role within communities suggests clear polarization, possibly reflecting groups with differentiated functions and implications in criminal dynamics. The marked separation between suspects and victims may indicate that the network is not just a heterogeneous set, but is structured according to social and legal roles, with few points of contact between communities of different roles.

<img width="301" height="251" alt="image" src="https://github.com/user-attachments/assets/e90bcbd2-08f8-46b6-8a9f-75705e6d931b" />

- The distribution of roles by gender was also analyzed: men predominate in all cases, but the proportion difference is greater among victims and suspects, indicating more female witnesses.


<img width="299" height="229" alt="image" src="https://github.com/user-attachments/assets/afd377d3-1d90-4fcb-8311-073b09a0145f" />

- The degree distribution by gender shows that men have a higher number of connections than women, with a higher degree in the third quartile and upper whisker.

The graph of the person with the highest centrality was also visualized, confirming previous conclusions that Katz Luella is the leader or central figure of the network: 

<img width="367" height="275" alt="image" src="https://github.com/user-attachments/assets/c6dc6147-99fa-4c77-ac7c-750d82c99b8f" />

A representation of the main component with names was also created, showing clear hubs that could be leaders of criminal groups:

<img width="325" height="294" alt="image" src="https://github.com/user-attachments/assets/063dc8bc-a104-4e7a-8981-f02aa8062963" />


Finally, communities among suspects were visualized to observe the different gang groups present in the network:
<img width="329" height="289" alt="image" src="https://github.com/user-attachments/assets/7c6cfbe2-aee5-488b-97c6-ef58039362b3" />

## References
Akhtar, M. S., & Saeed, A. (2021). Social Network Analysis: From Graph Theory to 
Applications with Python. arXiv preprint arXiv:2102.10014. 
https://arxiv.org/abs/2102.10014
Hagberg, A., Schult, D., & Swart, P. (2023). NetworkX documentation. NetworkX Project.
https://networkx.org/documentation/stable/tutorial.html
Jérôme Kunegis. KONECT – The Koblenz Network Collection. In Proc. Int. Conf. on World 
Wide Web Companion, pages 1343–1350, 2013. [ http ]
NetworkX Developers. (2023). Bipartite graphs — NetworkX documentation.
https://networkx.org/documentation/stable/reference/algorithms/bipartite.html

## Author
Cristina Galter, 2025
