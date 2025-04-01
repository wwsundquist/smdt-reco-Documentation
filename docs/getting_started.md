# Getting started

## Introduction
This code is designed by the ATLAS groups at the University of Massachusetts Amherst and the University of Michigan to reconstruct raw data files from MDT test chambers.

## Installation Guide
Clone the repository: **repo address**. Dependancies: CMake, ROOR, **...?**

## Building the code
To build the code, run
````
cd smdt_reco/
source setup.sh
cd ./build/
make -j10
cd ../
````
You may find it helpful to save these commands to a shell script in the ````smdt_reco/```` directory.

## Getting raw data

## Running the code
After building there will be several executables in you build area. The setup script creates aliases for these so they may be run quickly. These are—
````
./build/decodeRawData --conf ./conf/<your-file>.conf
````
````
./build/doT0Fit --conf ./conf/<your-file>.conf
./build/autoCalibration --conf ./conf/<your-file>.conf
./build/residuals --conf ./conf/<your-file>.conf
````
Finds difference between tracks predicted distance and the R(t) curve distance.
````
./build/residuals -h --conf ./conf/<your-file>.conf
````
Finds the residual for each hit based on track fitted without that hit. Should be N times slower. 
````
./build/efficiency --conf ./conf/<your-file>.conf
````
Choose the appropriate configuration file ````<your-file>.conf```` to your run.

## The configuration file

````
[General]
RawFileName         = run00192196_20211101_merged.dat
RunNumber           = 1
IsMC                = 0
MaxEventCount       = 100000000 #max number of packets = number of triggered events
MaxPassEvents       = 1000000 #max number of events which pass autoCalibration, events past this number aren't checked
DataFormat          = HPTDC

[RecoUtility]
MIN_HITS_NUMBER     = 6
MAX_HITS_NUMBER     = 12
MAX_TIME_DIFFERENCE = 20000
MIN_CLUSTER_SIZE    = 1
MAX_CLUSTER_SIZE    = 8
MIN_CLUSTERS_PER_ML = 1
MAX_CLUSTERS_PER_ML = 4

#for time calibration
TRIGGER_OFFSET      = 0
SIG_VOLTAGE_INVERT  = 1 #for legacy data formats
TRG_VOLTAGE_INVERT  = 0 #for legacy data formats
IS_RELATIVE_DATA    = 0 #relevant to Phase2
WIDTHSEL            = 2

#for histogram fitting
ADC_NOISE_CUT       = 35 #lower bound cut on ADC to remove noise, very early check
ADC_HIST_TOTAL_BIN = 256 #hist parameters
TDC_HIST_TOTAL_BIN = 512
ADC_HIST_LEFT      = 0 
ADC_HIST_RIGHT     = 400 
TDC_HIST_LEFT      = -400 
TDC_HIST_RIGHT     = 400 
DTmaxEstimate      = 200 #added to T0 for tmax seed guess


[TimeCorrection]
IsASD2 = 1 #charge scaling : converting ADC width to times

[T0Fit]
UseFullTDC = 0 #Legacy?

[Geometry]
RunNumber           = 1
Geo_config          = [something] #text config file for geometry. (sMDT there is no geo_config file, so use the geometry info below)
IsMDT               = 0
TriggerMezz         = 14 #Legacy
TriggerChannel      = 23 #Legacy
ActiveTDCs          =  0  : 1 : 2  : 3  : 4  : 5  : 6  : 7  : 8  : 9  : 20 : 21 : 22 : 23 : 24 : 25 : 26 : 27 : 28 : 29 #still used to check that TDC words are on list of active TDCs in Signal.cxx
TDCMultilayer       =  0  : 1 : 0  : 1  : 0  : 1  : 0  : 1  : 0  : 1  : 0  : 1  : 0  : 1  : 0  : 1  : 0  : 1  : 0  : 1 #Legacy geometry? might be used as checks still
TDCColumn           =  0  : 0 : 5  : 5  : 11 : 11 : 17 : 17 : 23 : 23 : 29 : 29 : 35 : 35 : 41 : 41 : 47 : 47 : 53 : 53 #Legacy geometry? might be used as checks still
ChamberType         = A #sMDT-related, has to do with number of chambers?
TdcColByTubeNo      = 1
MAX_TDC             = 36 #max TDC No to expect
MAX_TUBE_COLUMN     = 58 #max No tube columns to expect per layer
ML_distance         = 84.83 #mm #used to build chamber sMDTs, else in text config
TubeLength          = 1.625 #meters #used to build chamber sMDTs, else in text config
FlipTDCs            = 1
layer_distance      = 13.0769836 #mm 
column_distance     = 15.1  #mm
radius              = 7.5  #mm #radius of tube, used in both MDT and sMDT
min_drift_distance  = 0  #mm #used in R(t) fitting
max_drift_distance  = 7.1 #used in R(t) fitting
DisabledChannel     =  9999 # TDC7 channel9 disabled #method to disable specific channel



[AutoCalibration] #R(t) fitting
Parameterization = Chebyshev
MinEvent         =       0
NEvents          =  100000
MaxResidual      =  5
UseFullCheby     = 1
NRT              = 1
Tolerance        = 0.001
ConstrainZero    = 1
ConstrainEndpoint= 0
MaxLinear        = 0.15

[Residuals] #calculate residuals for first NEvents
NEvents          = 100000
MinEvent         =      0
UseResForSysts   =      0

[Resolution] #use to generate few different possible kinds of MC
DeconvolutionRun          = 14
DeconvolutionRunSoftEDown = 11
DeconvolutionRunSoftEUp   = 10
DeconvolutionRunNoSoftE   = 12
ReconvolutionRun          = 15
````
### Data format
Data formats related to signal type, it's a cypher to decode the words in the packet. Value should be one of these strings (w/o quotes!)
Legacy formats
  "AMT"
  "HPTDC"
    Kevin was using this 6 yrs ago.
Modern formats
  "Phase2"
    most common, miniDAQ or any DAQ collects data in this format
  "FELIX"
    format for HL LHC
  You'll know if you put it in wrong, code immediately exits for not one of these strings. src/reco/Signal.cxx
  Used in decodeRawData

### AutoCalibration
Can choose chebyshev or polynomial interpolation. Piecewise does not work. used when fitting the R(t) curve

## Other resources

[Depricated documentation](https://kenelson.web.cern.ch/documentation/smdt-reco/)

<p style="text-align:center;">[Ack! Take me Home](index.md)</p>
