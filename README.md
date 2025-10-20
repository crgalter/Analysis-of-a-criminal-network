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

- Built the initial bipartite graph using NetworkX.
- Projected the bipartite graph onto the set of persons to analyze co-involvement in crimes.
- Visualized the network:
  - Nodes colored by gender or role.
  - Used spring layout (NetworkX) and Force Atlas (Gephi) for clear grouping.
  - Maintained metadata for further analysis in Gephi or similar tools.

