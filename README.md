
The following document served as notes for organizing initial thoughts about the project.
Some of the parameters of the project changed over time and as a result the final analysis and code does not follow this outline exactly. Nevertheless it will give you a good sense of what this project entailed.

# Study Design for Clustered Randomized Trials
Molfenter and Wolfe: ‘Reversing pharyngeal muscle loss in aging through diet and exercise’

## Project Overview

**Project Background**
A brief summary of the research from the researchers:

>“As we age, we lose muscles mass and function in the throat. This can cause difficulty swallowing can cause pneumonia (when material is misdirected into the lungs during swallowing) and can harm nutrition, hydration, quality of life. Can we prevent and or reverse this phenomenon?”

### Primary Research Questions

Q1: “Can we **exercise** the muscles in the throat to strengthen them for improved swallowing function?”

Q2: “Can we provide increased **protein** to impact muscle mass and improve swallowing function?”

### Objectives

O1: Study design: generate set of study designs that are best suited to provide best estimate of treatment effect and avoid confounding.

O2: Power analysis: Use simulation to conduct power analysis and determine sample size; use R package [simstudy](https://cran.r-project.org/web/packages/simstudy/vignettes/simstudy.html) to conduct this analysis

### Intervention
T1. Exercise: Set of 4 swallowing exercises 3 times a week for 8 weeks
T2. Protein: Supplemental protein for 8 weeks
T3: Exercise + Protein: combined exercise and supplemental protein for 8 week
T4: Control: No active intervention


### Outcome Measures

Y1: Peak Pressure (mmHg, continuous, primary)

Y2: Battery of throat related measures including:

+ Normalized residue ratio scale (continuous)
+ Pharyngeal shortening (continuous)
+ Pharyngeal constriction (continuous)
+ Pre-albumin (continuous)
+ Physical function/strength (continuous)

### Unit of Randomization
Randomization will occur at the individual level (i) within clusters. Cluster will be composed of a selection non-random (?) sites ranging from local senior centers, naturally occurring retirement communities (NORCs) and community centers.

From each site, we will recruit n participants and randomly assign each participant to 1 of 4 treatment groups (3 treatment groups and 1 control). Sample size will be determined using simulation and power analysis. Currently, planning to recruit 12-15 individuals per site/cluster.

### Unit of Inference
The unit of inference is the individual. Results from this study will be used to make generalizations to the target population (senior citizens).

### Study Design
Key considerations:
+ Multi-factor intervention
	- two interventions alone and the combination of the two = 3 treatment groups + 1 control
	- Multi-factorial design allows assessment of effect of two treatment together
+ Attrition -- interventions and evaluation can cause considerable discomfort
	- How likely are participants in treatment condition to drop out?
	- Should controls be subjected to same discomfort? And assumed to have same drop out rate? Should we take three measurements for everyone?
	- Missing data issues!
+ Effect of clusters (icc) -- there might be important differences between centers regarding to access to healthcare.
	- Do we need to control for the site?
	- Variance inflation factor (VIF) = 1 + (cluster size - 1) x ICC
	- ICC degree of similarity among units within a cluster
+ Selection bias with site -- selection of individuals after clustering
	- Randomization within cluster should help with this. But what should we look out for where? Where could this go wrong?
+ Confounding of treatments
	- How to keep protein supplement from confounding on exercise (eating as exercise)
+ Figure out how to include aging effect on pre-post

**Different designs to try**

+ Factorial/multi-site design (randomize within clusters/sites)
+ Standard clustered RCT (randomize by clusters/sites)
+ Stepped wedge (all individuals in each cluster receives treatment but clusters receive treatment at different time points; randomize time of treatment)
+ Cross-over (no randomization, each individual acts as own control)

### Analysis
Methods to consider:

+ Conditional mixed effects
+ ANOVA
+ Marginal structural models

Explain with causal, anova, and mixed effects approaches

**Model building**
(this notation was originally written in Google sheets so it may be bit unreliable)

Outcome yijk, for person i in cluster j in experimental group k (k = {treatment, control}) is given as:
	Y_ijk = mu_k + u_j + (mu x u)jk + e_ijk

Where ukis a fixed effect and is the expected outcome for a person in the k treatment group
And:	

+ mu_j ~ N(0, (sigma_0)^2) is the random cluster effect
+ mu_jk ~ N(0, (sigma_1)^2)is the random interaction between cluster and treatment
+ epsilon_ijk ~ N(0, (sigma_e)^2)is the random person effect

Total variance is: (sigma_0)^2 + (sigma_1)^2 + (sigma_e)^2

Intracluster cluster correlation coefficient is:	 icc = ((sigma_0)^2 + (sigma_1)^2) / ((sigma_0)^2 + (sigma_1)^2 + (sigma_e)^2) 

+ ICC can be between 0 and 1
+ If 1, then all variability in outcome at cluster level and persons with cluster are identical
+ If 0, then all variability in outcome at person level and person outcomes within clusters are no more correlated than with persons in different clusters
+ ICC can be broken into two parts:
	- Proportion of ICC due to random cluster variation: 
icccluster = (sigma_0)^2 / ((sigma_0)^2 + (sigma_1)^2 + (sigma_e)^2)
	- Proportion of ICC due to random cluster-treatment interaction variation:icccluster iccinteraction = (sigma_1)^2 / ((sigma_0)^2 + (sigma_1)^2 + (sigma_e)^2)

**Determining sample size**
If N persons needed for study using simple random sample, n1 is the number of persons per cluster and n2 is the number of clusters then:

+ N(1 - icc_cluster + (n1 - 1)icc_interaction), if randomizing within clusters
+ N(1 + (n1 - 1)icc), if randomizing by clusters

Resources:
[Improved Designs for Cluster Randomized Trials](https://www.annualreviews.org/doi/full/10.1146/annurev-publhealth-032315-021702)
[rdatagen blog by Keith Goldfeld](https://www.rdatagen.net/post/using-simulation-for-power-analysis-an-example/)
[rdatagen blog by Keith Goldfeld](https://www.rdatagen.net/post/testing-many-interventions-in-a-single-experiment/)
[Randomization of Clusters Versus Randomization of Persons Within Clusters: Which Is
Preferable?](https://www-jstor-org.proxy.library.nyu.edu/stable/pdf/27643654.pdf?refreqid=excelsior%3Ac50b4e71f9b99d1ca142b5476fa84717)
