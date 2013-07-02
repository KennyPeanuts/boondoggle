# Re-analysis of the case study results with the range of temperatures from Gudsaz et al 2010.

## Introduction

This analysis is in response to the comments by Reviewer D where they question the use of a single temperature and Kd value for the model.

> [ ]6.      Their future projections for climate change impacts are based on one Kd
value (which in turn was based on one study from literature) and one
possible future temperature increase based on one other study.  I suggest
they at least base these temperature estimates on climate change projections
for the region?  Or used a range of temperatures to capture the uncertainly
that is widely acknowledged for future climate projections?

I do not think that a more sophisticated climate analysis is warrented since the purpose of the model is to show the sensitivity of the system to **any** change in the temperature and Kd.  Furthermore, the range of temperatures indicated in Gudsaz et al 2010 are based on the best available global predictions from the IUCC.

Gudsaz et al 2010 indicate that the **most likely** warming scenario from the IUCC assessment is 1.8 - 4 deg C and the **likely** warming scenario is from 1.1 - 6.4 deg C. (This is based on: Solomon, S. et al. in Climate Change 2007: The Physical Science Basis. eds Solomon, S. et al. 1–18 Cambridge Univ. Press, 2007).

## Data Import

Use working directory:

    getwd()
    [1] "/Volumes/NO NAME/working_files/current_research/boondoggle/inland_waters_submission/revisions/revision_analysis"

### Load workspace

    # load boondoggle workspace with environmental data on GTH 91
    load("./data/boondoggle")

    # load GTH 91 case study workspace with data on the case study model
    load("./data/GTH91.case.ms")

Use the `boon.tot` data.frame for the environmental data

Use the `GTH91.case.ms` data.frame for the model data

##Calculate the percent area of sediments in the epiliminion of lake GTH 91

### Generate the constants needed for the calculations

    z.max <- 10
    GTH91.alpha <- 0.5 

#### Variable descriptions

* z.max is the maximum depth of lake GTH 91 (m)

* GTH91.alpha is the non-dimensional constant that was calculated from the hyposgraphic curve in in the GTH91.bathy data.frame. See "boondoggle_data_analysis.pdf" for origin of value
 
### Perform calculation of the sediment area of the mixed layer in lake GTH91 using the measured TD

     mix.A <- GTH91.case.ms$Area - (GTH91.case.ms$Area * (1 - (GTH91.case.ms$TD / z.max))^GTH91.alpha)

#### Output

The sediment area above the thermocline (ha) in lake GTH 91 using the observed TD 

~~~~

> mix.A
 [1] 0.2639320 0.4083499 0.4844356 0.7322330 0.8229490 0.8229490 0.3349365
 [8] 0.4844356 0.4844356 0.6459504 0.8229490

~~~~

This calculation reproduced the values in the current manuscript (GTH91.case.ms$Mix.Area)

### Perform calculation of sediment area of the mixed layer in lake GTH 91 using the predicted TD based on the linear model of Kd, Area, and Julian day from my dissertation

Model parameters came from table 6. Also from p 64, table 17 of my dissertation

     pred.mix.A <- GTH91.case.ms$Area - (GTH91.case.ms$Area * (1 - (-3.08 * GTH91.case.ms$Kd + 0.04 * GTH91.case.ms$Area + 0.067 * GTH91.case.ms$Julian -7.08)/z.max)^GTH91.alpha)

#### Output

The sediment area above the thermocline (ha) in lake GTH 91 using the observed TD 

~~~~

> pred.mix.A
 [1] 0.1552879 0.2864717 0.4001524 0.5238605 0.5601250 0.8381659 0.3750829
 [8] 0.4459662 0.5707793 0.5685219 0.7323844

~~~~

This analysis very closely reproduces the values in the manuscript in GTH91.case.ms$pred.Mix.Area but there are some slight discrepancies that I suspect are due to rounding errors somewhere.

The values in GTH91.case.ms$pred.Mix.Area are:

~~~~

> GTH91.case.ms$pred.Mix.Area
 [1] 0.1546216 0.2857659 0.3994084 0.5230699 0.5593197 0.8372260 0.3743477
 [8] 0.4452056 0.5699696 0.5677131 0.7315006

~~~~
