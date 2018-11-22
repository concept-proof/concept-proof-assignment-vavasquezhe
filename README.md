# concept-proof-travis
Concept proof about integration of Docker, GitHub Classroom and Travis CI for teaching and grading computer science courses.

## Cambio
## Valentina

This concept proof uses four repos:

* https://github.com/jdvelasq/concept-proof-docker contains a Docker container specification for deploying runtime environments, and Jupyter Notebooks for deploying tutorials and lecture notes. 

* https://github.com/jdvelasq/concept-proof-travis contains a Jupyter notebook with the assigment description and the a `.travis.yml` file with the information for automatic grading of the assignment. Travis-CI is used to push a formated text file to the repo https://github.com/jdvelasq/concept-proof-grade with the grade. For avoiding student modification of the solution file, a non-modifiable copy of the expected result is saved in the repo https://github.com/jdvelasq/concept-proof-solutions and copied before the grading.

* https://github.com/jdvelasq/concept-proof-grade contains only text files with the detailed info of each student repo.

* https://github.com/jdvelasq/concept-proof-solutions contains a secure copy of the solution of the assignments.  


In this proof, this contaniner is used for grading assignments.   

## Step 1
Prepare a jupyter notebook with the assignment description and the solution. In this case, we use `gradetool` (see https://github.com/jdvelasq/gradetool); this tool saves in a json file with the expected solution for the questions conforming the assignment. `gradetool` is used for grading the notebook. In our case, the files are  `sql-grading.ipynb` and `sql-grading.json`.

## Step 2
Create a repo on GitHub for the assignment. In this case, we use this repo (https://github.com/jdvelasq/concept-proof-travis). Upload the assignment and the solution.


## Step 3
The file `.travis.yml` in the assignment repo must be customized for the automatic grading using Travis-CI. 

```
## Required for running docker inside of Travis
sudo: required

services:
  - docker

env:
  global:
    - USER='jdvelasq'                           ## GitHub user
    - EMAIL='jdvelasq@unal.edu.co'              ## GitHub email
    - FILENAME=${PWD##*/}.txt  ## contains the name of the current repo

before_install:
  - docker network create proof-net
  - docker pull mysql:5
  - docker run --rm --name proof-mysql --network=proof-net -e MYSQL_ROOT_PASSWORD=password -d mysql:5
  - docker pull jdvelasq/jupyter-proof
  

script:
  - docker run --rm --network=proof-net --volume="$(pwd)":/home  jdvelasq/jupyter-proof /bin/sh -c "cd home && ./gradetool  sql-grading.ipynb" > grade.txt
  - cat ${FILENAME}
  - mv grade.txt ${PWD##*/}.txt


after_success:
  - git config --global user.email ${EMAIL}
  - git config --global user.name ${USER}
  - git clone --quiet https://jdvelasq:${GITHUB_TOKEN}@github.com/jdvelasq/concept-proof-grade.git  ./repo
  - mv ${FILENAME} ./repo/${FILENAME}
  - cd repo
  - echo ${FILENAME}
  - git add ${FILENAME}
  - git commit -m "grading from Travis-CI"
  - git push origin master
```


## Step 4
Create a personal access token in  **Profile > Settings > Developer Settings > Personal Access Token** for giving access to Travis-CI to your repositories. In the **settings** of the repo on Travis-CI, create the secure environment variable `GITHUB_TOKEN` and assign it the personal access token obtained from GitHub.


## Step 4
Create an organization on GitHub and the course on GitHub Classroom as usual. Use the repo https://github.com/jdvelasq/concept-proof-travis for creating the assignment for the students.

When each student's repo is updated, Travis-CI runs the `.travis.yml` file and grade the assignment.




 


