# concept-proof-travis
Concept proof about integration of Docker, GitHub Classroom and Travis CI for teaching and grading computer science courses.

The repo https://github.com/jdvelasq/concept-proof-docker contains a concept proof about the integration of Docker containers for deploying runtime environments, and Jupyter Notebooks for deploying tutorials and lecture notes. The repo https://github.com/jdvelasq/concept-proof-travis contains a concept proof for evaluating Jupyter Notebooks using Travis CI.

In this proof, this contaniner is used for grading assignments.   

## Step 1
Prepare a jupyter notebook with the assignment description and the solution. In this case, we use `gradetool` (see https://github.com/jdvelasq/gradetool); this tool saves in a json file with the expected solution for the questions conforming the assignment. The file `gradetool` is used for comparing and grading the notebook. In our case, the files are  `sql-grading.ipynb` and `sql-grading.json`.

## Step 2
Create a repo on GitHub for the assignment. In this case, we use this repo. 

## Step 3




