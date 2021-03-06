<!-- README.md is generated from README.Rmd. Please edit that file -->



# S4 generic functions for Bioconductor mass spectrometry infrastructure

## Description:

These generic functions provide basic interfaces to operations on and
data access to proteomics and mass spectrometry infrastructure in the
Bioconductor project.

For the details, please consult the respective methods' manual pages.

## Usage:

```r
psms(object, ...)
peaks(object, ...)
modifications(object, ...)
database(object, ...)
rtime(object, ...)
tic(object, ...)
spectra(object, ...)
intensity(object, ...)
mz(object, ...)
peptides(object, ...)
proteins(object, ...)
accessions(object, ...)
scans(object, ...)
mass(object, ...)
ions(object, ...)
chromatograms(object, ...)
chromatogram(object, ...)
isCentroided(object, ...)
```

## Arguments:

- `object`: Object of class for which methods are defined.
- `...`: Further arguments, possibly used by downstream methods.

## Details:

### When should one define a generics?:

Generics are appropriate for functions that have _generic_
names, i.e. names that occur in multiple circumstances, (with
different input classes, most often defined in different
packages) or, when (multiple) dispatching is better handled by
the generics mechanism rather than the developer. The
dispatching mechanism will then automatically call the
appropriate method and save the user from explicitly calling
`package::method` or the developer to handle the multiple input
types cases. When no such conflict exists or is unlikely to
happen (for example when the name of the function is specific to
a package or domain, or for class slots accessors and
replacement methods), the usage of a generic is arguably
questionable, and in most of these cases, simple,
straightforward functions would be perfectly valid.

### When to define a generic in `ProtGenerics`?:

`ProtGenerics` is not meant to be the central package for generics,
nor should it stop developers from defining the generics they need. It
is a means to centralise generics that are defined in different
packages (for example `mzR::psms` and `mzID::psms`, or
`IRanges::score` and `mzR::score`, now `BioGenerics::score`) or
generics that live in a rather big package (say `mzR`) on which one
wouldn't want to depend just for the sake of that generics'
definition.

The goal of `ProtGenerics` is to step in when namespace conflicts
arise so as to to facilitate inter-operability of packages. In case
such conflict appears due to multiple generics, we would (1) add these
same definitions in `ProtGenerics`, (2) remove the definitions from
the packages they stem from, which then (3) only need to import
`ProtGenerics`. This would be very minor/straightforward changes for
the developers and would resolve issues when they arise.

More generics can be added on request by opening an issue or sending a
pull request on:

[https://github.com/lgatto/ProtGenerics](https://github.com/lgatto/ProtGenerics)


## See Also:

- The `BiocGenerics` package for S4 generic functions needed by many
  Bioconductor packages.
- `showMethods` for displaying a summary of the methods defined for a
  given generic function.
- `selectMethod` for getting the definition of a specific method.
- `setGeneric` and `setMethod` for defining generics and methods.

## Examples:


```r
library("ProtGenerics")
## 
## Attaching package: 'ProtGenerics'
## The following object is masked from 'package:stats':
## 
##     smooth
## List all the symbols defined in this package:
ls('package:ProtGenerics')
##  [1] "accessions"                "acquisitionNum"           
##  [3] "aggregateFeatures"         "analyser"                 
##  [5] "analyserDetails"           "analyzer"                 
##  [7] "analyzerDetails"           "centroided"               
##  [9] "centroided<-"              "chromatogram"             
## [11] "chromatograms"             "collisionEnergy"          
## [13] "collisionEnergy<-"         "combineFeatures"          
## [15] "database"                  "dataOrigin"               
## [17] "dataOrigin<-"              "dataStorage"              
## [19] "dataStorage<-"             "detectorType"             
## [21] "expemail"                  "exptitle"                 
## [23] "filterAcquisitionNum"      "filterDataOrigin"         
## [25] "filterDataStorage"         "filterEmptySpectra"       
## [27] "filterIsolationWindow"     "filterMsLevel"            
## [29] "filterMz"                  "filterNA"                 
## [31] "filterPolarity"            "filterPrecursorMz"        
## [33] "filterPrecursorScan"       "filterProductMz"          
## [35] "filterRt"                  "impute"                   
## [37] "instrumentCustomisations"  "instrumentManufacturer"   
## [39] "instrumentModel"           "intensity"                
## [41] "intensity<-"               "ionCount"                 
## [43] "ions"                      "ionSource"                
## [45] "ionSourceDetails"          "isCentroided"             
## [47] "isolationWindowLowerMz"    "isolationWindowLowerMz<-" 
## [49] "isolationWindowTargetMz"   "isolationWindowTargetMz<-"
## [51] "isolationWindowUpperMz"    "isolationWindowUpperMz<-" 
## [53] "mass"                      "modifications"            
## [55] "msInfo"                    "msLevel"                  
## [57] "msLevel<-"                 "mz"                       
## [59] "mz<-"                      "peaks"                    
## [61] "peaks<-"                   "peptides"                 
## [63] "polarity"                  "polarity<-"               
## [65] "precAcquisitionNum"        "precScanNum"              
## [67] "precursorCharge"           "precursorCharge<-"        
## [69] "precursorIntensity"        "precursorIntensity<-"     
## [71] "precursorMz"               "precursorMz<-"            
## [73] "processingData"            "processingData<-"         
## [75] "productMz"                 "productMz<-"              
## [77] "proteins"                  "psms"                     
## [79] "rtime"                     "rtime<-"                  
## [81] "scanIndex"                 "scans"                    
## [83] "smooth"                    "smoothed"                 
## [85] "smoothed<-"                "spectra"                  
## [87] "spectra<-"                 "spectraData"              
## [89] "spectraData<-"             "spectraNames"             
## [91] "spectraNames<-"            "spectraVariables"         
## [93] "tic"                       "tolerance"                
## [95] "writeMSData"

library("mzR")
## Loading required package: Rcpp
## What methods exists for 'peaks'
showMethods("peaks")
## Function: peaks (package ProtGenerics)
## object="mzRnetCDF"
## object="mzRpwiz"
## object="mzRramp"

## To look at one method in particular
getMethod("peaks", "mzRpwiz")
## Method Definition:
## 
## function (object, ...) 
## {
##     .local <- function (object, scans) 
##     .peaks(object, scans)
##     .local(object, ...)
## }
## <bytecode: 0x557bfff39e90>
## <environment: namespace:mzR>
## 
## Signatures:
##         object   
## target  "mzRpwiz"
## defined "mzRpwiz"
```
