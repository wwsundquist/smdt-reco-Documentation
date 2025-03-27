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
sc ./build/
make -j10
cd ../
````
You may find it helpful to save these commands to a shell script in the ````smdt_reco/```` directory.

## Getting raw data

## Running the code
After building there will be several executables in you build area. The setup script creates aliases for these so they may be run quickly. These are—
````
./build/decodeRawData --conf ./conf/<your-file>.conf
./build/decodeRawData --conf ./conf/<your-file>.conf
./build/autoCalibration --conf ./conf/<your-file>.conf
./build/residuals --conf ./conf/<your-file>.conf
./build/residuals -h --conf ./conf/<your-file>.conf
./build/efficiency --conf ./conf/<your-file>.conf
````
Choose the appropriate configuration file ````<your-file>.conf```` to your run.

## The configuration file

## Other resources

[Depricated documentation](https://kenelson.web.cern.ch/documentation/smdt-reco/)

[Ack! Take me Home](index.md)
