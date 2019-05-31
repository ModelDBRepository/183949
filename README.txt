This is the readme for the models associated with the paper:

 Powers RK, Heckman CJ (2015) Contribution of intrinsic motoneuron
 properties to discharge hysteresis and its estimation based on paired
 motor unit recordings. A simulation study. J Neurophysiol

These files were contributed by Dr RK Powers.

The discharge_hysteresis folder contains all of the mod files and most
of the hoc files used in simulations described in Powers and Heckman
2015.  The folder also contains a python file, pars2manyhocs.py, that
generates hoc files for each motoneuron model in a particular
population. In the paper, we used 20 models for each population, and
several model parameters (e.g., surface area and specific resistivity)
varied systematically to generate models with a range of recruitment
thresholds. We have included files to generate all of the populations
used in the paper:

standard.csv Standard model, can also be used to simulate spike
frequency adaptation (SFA) due to slow Na inactivation (see below)

SlowM.csv Used to simulate SFA due to a slow outward current

AHPlen.csv Used to simulate SFA due to a lengthening AHP

NaPi.csv Used to simulate threshold accommodation due to inactivation
of a persistent sodium current

FasterMis.csv Used to simulate threshold accommodation due to a low
threshold outward current in the initial segment

ProxCa.csv produces models with a proximal dendritic location for
Cav1.3 channels

LCai.csv produces models with inactivating Cav1.3 channels

HiDKCa.csv produces models with higher levels of dendritic
calcium-activated K channels


To generate the model populations:
In Unix/Linux, put the Python file pars2manyhocs.py anywhere in your
path. Set the file to executable mode by typing: chmod 755
pars2manyhocs.py

Make a folder with the same name as the csv file (e.g. for
standard.csv, make a folder called standard) and put the cdv file in
it. In the terminal window change directory to that folder then type:
pars2manyhocs.py ****.csv (e.g. pars2manyhocs.py standard.csv)

When the program asks you how many cells you want, type 20.  The
program generates a series of subfolders each containing a hoc file
for simulating one cell

After you have generated the model hoc files type cd .. to move up one
directory level

Now type: nrngui init_3dend_gramp.hoc

This starts NEURON and calls up the set of files needed to produce
model responses to conductance inputs.  The hoc files defines a number
of routines that can be used to generate spike times for all of the
models in the population in a response to a slow conductance input
(the default is 10 sec up and 10 sec down).

The first routine is batchrun20pool.  This takes three inputs: a
string input providing the model name, and two numeric inputs that
specify the relative density of dendritic Ca channels and the relative
amount of slow Na inactivation (1=no inactivation and 0=full
inactivation). For example, to generate the outputs of all 20 models
in the standard population with the default Cav1.3 density and no slow
Na inactivation type: batchrun20pool("standard",1.0,1.0).  To include
slow Na inactivation of the amount used in the paper type:
batchrun20pool("standard",1.0,0.2).

The same routine can be used to generate outputs from other model
populations.  The only difference is that in most cases you have to
insert a mechanism that is not part of the standard model or run a
variant of the batch routine as described below:

For SlowM, insert km_hu in the soma, is, and axonhillock compartments.

For AHPlen, insert mAHPvt in the soma and dendritic compartments and
uninsert mAHP

For NaPis, insert napsi into all compartments and uninsert naps. Then
run the routine batchpool20poolnasi with the same conventions as
batchrun20pool.

For FasterMis, insert km_hu into the is (initial segment) compartment

For ProxCa run the routine: batchrun20poolprox

For LCai, uninsert L_Ca from the dendritic comparments and insert
L_Ca_inact. Then run batchrun20poolLCainact.

20160129 A bug introduced by Tom Morse's conversion of a perl script
to pars2manyhocs.py was noted by Randy Powers and fixed.  The new
version required updating csv files to remove last line comments.
