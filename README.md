# Tutorial on data analysis and fitting with ROOT for the LIP Internship Program

## Preparation (for students with access to LIP computing resources)

#### Clone code repository
Clone this repository in your current folder with
```bash
git clone https://github.com/aboletti/LIP-analysis-tutorial.git
```
You can consult the GitHub documentation for information on how to [set up git](https://docs.github.com/en/get-started/getting-started-with-git) and [clone a repository](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository-from-github). However, if the command above worked without errors, there is no need to do that to continue with the tutorial.

Enter the folder with the code with
```bash
cd LIP-analysis-tutorial
```

#### Download the dataset
The data that you will use in this tutorial have to be downloaded with
```bash
wget https://aboletti.web.cern.ch/aboletti/LIP-tutorial/Skim4.root
```

#### Other preparatory steps
Load root and create a folder for the plots you will produce with
```bash
module load root
mkdir plots
```

## Step 0: open the data file and check its content

```bash
root Skim4.root
```
The TTree contains a set of [TLorentzVector](https://root.cern.ch/doc/master/classTLorentzVector.html), one for each muon, and one that os obtained somming the other two. Try to plot the dimuon invariant mass spectrum with:
```cpp
oniaTree->Draw("dimuon_p4.M()")
```
How can we improve this plot?

## Step 1: test and familiarize with the code

Check the code, trying to understand its structure.
Detailed information about any ROOT or RooFit class can be found in the reference manual, looking for the class name in [this list](https://root.cern/doc/master/classes.html), or simply writing the class name in a search engine.

Set the value of `a._sdig` to the day of your birthday (i.e. the day of the month, from 1 to 31), to "randomize" the set of data you are going to study.

Run the default configuration of the code
```bash
root -b -q dimuons.C
```
and check the output graphs and note down the fit results.

Optional: the code contains also a function to fit using standard ROOT objects (without RooFit). This is commented by default. You can try to run it (now, and also for the following steps) to compare the results of the fits. In any case, we will consider the RooFit results as the official ones.

## Step 2: variations in fit models

The current fit uses a [Gaussian function](dimuons.C#L189-L192) as signal model and an [exponential](dimuons.C#L185-L187) as background model. This is not always enough to describe the peaks we are going to fit.
On the J/psi peak, the bad description is more evident if you fit the full sample (setting `a._sdig = -1`, try it).

Try different functions both for the signal model (like [Crystal-Ball](https://root.cern.ch/doc/master/classRooCBShape.html), [Breit-Wigner](https://root.cern.ch/doc/master/classRooBreitWigner.html), or the sum of two Gaussian functions), and for the background model (like a 1st or 2nd degree [polynomial](https://root.cern.ch/doc/master/classRooPolynomial.html)).
Pay attention to the quality of the fit with each function and how the signal yield varies.

## Step 3: fit of a different peak

Until now, you worked only with the J/psi peak, but other ones are present in our spectrum.

Choose a different peak and adapt the code to fit it (by [setting the appropriate mass range](dimuons.C#L37-L41)). Note that the Upsilon peaks are more challenging since they have to be fitted all three in the same fit.

Try also here different functions to find the one that better describes your peak.

## Bonus step: unbinned fit

Adapt the code to perform an unbinned fits (using a [RooDataSet](https://root.cern.ch/doc/master/classRooDataSet.html)) instead of binned ones.

## Bonus step 2: yield measurement in bins of transverse momentum

So far, you have measured the signal yield inclusively for the all sample. However, one interesting improvement is measuring the differential yield as a function of one particular variable (which can be used to measure a differential cross-section or branching ratio). This is obtained by splitting the sample in bins of that variable and for each bin computing the ratio between the yield and the size of the bin.

Start by drawing the distribution of the transverse momentum of the dimuon candidate, for the events present in the mass region you have chosen in _step 2_. Divide this distribution in a set of bins to use for your measurement; you are free to define the binning that you prefer, but try to follow these guidelines: it should cover the entire spectrum of the dimuon transverse momentum, it should be uneven (narrow bins where there is a high density of events, larger bins where the density is lower), and it should have at least 5 bins (but if your peak has a lot of events, you can try a larger number).

Then, add to the code a selection on the transverse momentum of the dimuon candidate, and measure the yield of your peak only in this region.

Finally, measure the yield for every single bin and create a graph with the dimuon transverse momentum on the x-axis, the measured yield divided by bin size on the y-axis.
