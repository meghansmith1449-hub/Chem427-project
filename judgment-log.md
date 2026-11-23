# Judgment log: Meghan Smith

## How I used AI in this project

(Final submission only.)

## Entries

### Nov 21: Loading the expert annotations (Cell 8)
- **What the assistant suggested:** The assistant suggested code to create a table with one row per annotated particle, from data/annotations.zip.
- **What I did:** I ran the code to verify it and decided to include it in my project. 
- **Why, from my data:** In the project description, it is listed that there are 11,227 expert-annotated particles. The cell's total printed row count agrees with this number.  

### Nov 21: Explore section, median table
- **What the assistant suggested:** The assistant suggested code to create a table with the AFM and SEM medians for each material. 
- **What I did:** I tested the code, but knew from another cell that the gold distribution was bimodal, so it did not make sense to report the medians this way. I asked the assistant to revise the code so that the medians would be reported for each individual population, separating the two gold sizes. I approved the new code. 
- **Why, from my data:** The probability distributions showed two peaks for gold, one centered around 30 nm and one around 60 nm. The initial table printed one gold median for each method, 37.9 nm for AFM and 56.9 nm for SEM. The revised table printed two medians for each method, which matched the probability distribution. In the new table, gold (60 nm) had an AFM median of 58.7 nm, and SEM median of 61.4 nm. Gold (30 nm) had an AFM median of 28.7 nm, and an SEM median of 30.7 nm.

### Nov 21: Bootstrap
- **What the assistant suggested:** The assistant suggested code to bootstrap the medians for each population, using units of images, per my request. It also suggested showing the result for particles as the unit as an alternative run with the same data, side by side with images. 
- **What I did:** I approved the suggestion to add in the particle bootstrap and added the assistant's code into my notebook.
- **Why, from my data:** The results show that for gold (60 nm), the CI is wider for the image bootstrap (2.56 vs 1.93 nm), but the image bootstrap CI is smaller than the particle one for gold (30 nm) and silica, (1.36 vs 1.77 nm) and (2.10 vs 2.56 nm). For all, the CI excludes 0, meaning the choice of bootstrap does not change the conclusion we draw from the data, which is valuable to know. 

### Nov 21: What counts as a particle (cell 25)
- **What the assistant suggested:** The assistant suggested code with a rule that would count particles that are of a min area, MIN_PARTICLE_AREA_PX = 170 and do not touch the map edge. 
- **What I did:** I ran the code to verify it worked and included it in my project.
- **Why, from my data:** The method kept 39 whole particles and the median particle height identical to the expert's, 43.72 nm. The confidence intervals were not identical, as the experts kept 42 particles, but a 3 particle difference is small.