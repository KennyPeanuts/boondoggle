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

## Calculate the change in Kd in lake GTH 91 based on the likely temperature increase of 1.1 - 6.4 deg C

Keller et al. 2008 estimate a 0.5 to 2.3 mg/L increase in DOC per deg C warming.

I cannot find the original DOC measurement that I used for the calculations in the manuscript but SCW reports a 458 umol/L DOC in GTH 91 in early July (SCW unpublished data)

### Conversiton of SCW DOC value to mg/L

    GTH91.DOCumolL.Jul2009 <- 458 # the DOC concentration of GTH 91 in early July 2009 in umol/L (SCW unpub. data)
    ug.umol.C <- 12 # micrograms per umol of carbon

    # confirm unit conversion
    umol    ug      ug
    ---- * ----  = ----
     L     umol     L

    # convert to ug/L
    GTH91.DOCugL.Jul2009 <- GTH91.DOCumolL.Jul2009 * ug.umol.C # the DOC conc. in ug/L

    # convert to mg/L
    GTH91.DOCmgL.Jul2009 <- GTH91.DOCugL.Jul2009 / 1000
    GTH91.DOCmgL.Jul2009

#### Output

    [1] 5.496 # DOC conc. of GTH 91 in early Jul 2009 (mg/L)

### Generate the range of DOC values for a 1.1 to 6.4 deg C temp. increase based on Keller et al 2008 formula

To make a vector of the DOC values that would result from a 1.1 to 6.4 deg C increase in temperature I am applying the relationship identified by Keller et al 2008, which is a 0.5 to 2.3 mg/L increase in DOC per 1 deg C increase in temp.

#### Calculations

    temp.incr <- 1:7
    keller.low <- 0.5
    keller.high <- 2.3

To generate a vector of DOC concentrations I add the current DOC in GTH 91 to the temperature increase multiplied by the Keller high and low factor increase

    DOC.low <- GTH91.DOCmgL.Jul2009 + (temp.incr * keller.low)
    DOC.high <- GTH91.DOCmgL.Jul2009 + (temp.incr * keller.high)

To calculate the change in epi SOD for the temperature alone, I use the slope of the temp effect from the temperature experiment to "adjust" the estimated current temp.

    temp.slope <- 0.61
    current.epi.area <- 4945
    current.epi.SOD <- 18.6
    temp.SOD.areal.incr <- current.epi.SOD + (temp.incr * temp.slope)
    temp.SOD.incr <- temp.SOD.areal.incr * current.epi.area

##### Variables

* temp.incr is a vetor of temperature increases in 1 deg C incremnets that approxomates the range of 'likely' temperature increases from Gudsaz et al 2010 which report a range of increases from 1.1 to 6.4

* keller.low is the low estimate of the DOC increase (mg/L) per 1 deg C increase in temp from Keller et al 2008
 
* keller.high is the high estimate of the DOC increase (mg/L) per 1 deg C increase in temp from Keller et al 2008

* DOC.low is the vector of GTH 91 DOC concentration (mg/L) following a 1 to 7 deg C temp increase based on the Keller et al 2008 low estimate

* DOC.low is the vector of GTH 91 DOC concentration (mg/L) following a 1 to 7 deg C temp increase based on the Keller et al 2008 high estimate

* temp.slope is the change in areal SOD (mmol O2/m2/d) per change in deg C

* current.epi.area is the median sediment area above the thermocline (m2) taken from the manuscript

* current.epi.SOD is the median of the SOD from the 12 deg temp exp treatment (mmol O2/m2/d)

* temp.SOD.areal.incr is a vector of the predicted areal SOD (mmol O2/m2/d) for the epilimnion over the 1 - 7 deg C range of temp increases

* temp.SOD,incr is a vector of the epi SOD (mmol O2/d) for the whole epilimnion
