---readme written by Stephen West (2024.08.26)---
## Context
The simulation used in this paper (sim9-1.3) is based on the code that was in Matsumoto & Sasakura (2016), used with permission.
Modifications were made so that the chance of selecting a destination area could be defined per link (defined in link.txt and based on least-cost path analysis).
Additionally, modifications were made to sim.c (590:602) so that an area's carrying capacity would be based on the average skill value of its agents.
Besides these minor structural changes, the simulation program is largely unchanged from its original form.

## Citation
Matsumoto, N and Sasakura, M. 2016.
Cultural and Genetic Transmission in the Jomon–Yayoi Transition Examined in an Agent-Based Demographic Simulation. 
In:. Barceló, JA and Del Castillo, F (eds.). Simulating Prehistoric and Ancient Worlds.
Springer International Publishing. pp. 311–334.

## Directories
- /sim9-1.3 #Directory with files needed to execute the simulation.
- /sim9-1.3/parameters #Directories with the input files and results for each parameter. Move files into sim9-1.3 directory and overwrite to replicate.

## Files
### Input Files
- area.txt (defines simulation areas and settings per area)
    - Columns: area code, area name, carrying capacity, movement rate, over-capacity move rate modifier, over-capacity death rate modifier
- link.txt (defines which areas are linked, chance of agents selecting one area over another)
    - Columns: from area code, to area code, chance of selection by migrating agents.
- BirthRate.txt (defines the birthrates per area, age, & sex)
    -Column 1: area code. Columns 2~129: Annual chance of childbirth for females agents between ages 0 and 127. Columns 130〜257: Annual chance of childbirth for male agents between ages 0 and 127.
- MortalityRate.txt (defines the chance that an agent will die each year per area, age, & sex)
    - Column 1: area code. Columns 2~129: Female mortality rate between ages 0 and 127. Columns 130〜257: Male mortality rate between ages 0 and 127.
- init.txt (defines the initial population, sex ratio, and gvalue of simulation areas)
    - Columms: area code, population distribution type (1 hunter-gatherer, 2 agriculturalist), initial population, male to female ratio, initial gvalue
- Skill.txt (skill value of agents in each areɑ)
    - Column 1: area code. Columns 2~129: Female skill value between ages 0 and 127. Columns 130〜257: Male skill value between ages 0 and 127.

### Output Files
- logN.txt #Log of the simulation output. I have compressed the original log files as they are very large.
- poplog.data #Population totals per year for each simulation area (year, area 1, area 2, ...)
- gvalue.data #Average gvalue per year for each simulation area (year, area 1, area 2, ...)
- skill.data #Average skill value per year for each simulation area (year, area 1, area 2, ...)
- migration.data #Number of migrants leaving area per year for each simulation area (year, area 1, area 2, ...)
- ".png #proc.rb generates graphs of the above values per year for each simulation area.
I have cleaned the output files for each parameter and put them into the Simulation Results folder.

## How to Replicate
A development environment for C, Ruby, and X11 is needed.
1. Move the files from a parameter directory into the sim9-1.3 directory and overwrite.
2. Use the terminal to bash gogo-sim.sh and execute sim. 
	-gogo-sim.sh specifies the settings that are used by the simulation (timeframe, marriage rules, etc.)
	-The results are written in logN.txt.
3. Use the terminal to bash gogo-graph.sh and organize / graph the data.
	-The relevant outputs are: poplog.data, gvalue.data, skill.data
	-These are the files I used to produce the figures that are used in the publication.
	-The graphs display the results per area and are were not used in the publication.
The ABM is stochastic so replicated results may vary slightly from the original data.

## How to Modify
- Input files can be easily modified to model new population values, carrying capacities, etc.
    - Changing the number of areas requires changes to areas.txt, init.txt, link.txt, and modifying the ruby scripts.
- Simulation settings (options) can be modified via the command given in shell script (gogo-sim.sh)
    - Refer to the original documentation below for information on simulation options.
- The variable carrying capacity functionality I added in sim.c (590:602) is hardcoded and must be manually changed.
- If changes to the C script are made, sim must be rebuilt with the makefile provided for the changes to take effect.

Refer to the original documentation for further details. A development environment for C, Ruby, and X11 is needed.

## Documentation
Refer to the original documentation (readme.txt) that was provided by Matsumoto and Sasakura (in Japanese). 
