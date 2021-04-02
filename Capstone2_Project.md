## Data: 
Stability of pharmaceuticals is of the utmost importance. The Food and Drug Administration (FDA) requires that companies know and study the forms of their drug products to ensure safety and efficiency of their products. Many solids can exist as polymorphs, meaning they can have two or more crystalline forms, but are composed of the same material. For example, diamonds and graphite are both made of carbon, but the atoms are arranged differently. This affects their physical properties, for diamonds and graphite their hardness is most notably different.



Several drug products on the market have had issues with polymorphism. Including Ritonavir, a drug used for treating human immunodeficiency virus (HIV). In the 1980s, Abbott Labs had an issue with the effectiveness of their drug compound as theirs had changed crystal forms. This new form of Ritonavir was not effective in treating HIV and thus had to be removed from the market until the problem was solved. This caused Abbot to shut down several manufacturing plants, due to trace amounts of the new form catalyzing their desired drug product. As such, many new techniques have been developed to monitor crystal transforms. 

This work uses a proprietary, organic crystalline solid as a model system. Two forms are known to exist of this solid and it is unknown what causes it to transform. In order to run different experiments to test the sample, a model must be built to determine the percent converted to form B. This will allow researchers to compare their samples and determine what percentage of their material they have.


## Data Cleaning: 
This data was collected using a THz spectrometer and the raw Single Beam data was exported as a CSV into MatLab. The data was converted using proprietary scripts into THz absorbance spectra. The first and last spectra were collected. The starting material is Form A and the ending material is Form B.  

Unfortunately, the researchers were unable to collect concentration information, but it was determined by using the spectral features to approximate the percentage converted. Beer's Law states A=ebc, where absorbance is proportional to concentration. First, the maximum for the peaks of interest were located. Knowing the peaks associated with the forms allows us to interpret the concentration. The percent transformed can be determined using the equation below where Ai is a given absorbance in a spectra at a certain wavelength, by dividing by the highest absorbance (Ahigh) we can get a rough percent, but due to instrumental fluctuation and sample changes, we can adjust a bit more by subtracting the low at the wavelength from both the numerator and denominator. 

% transformed = (Ai - Alow)/(Ahigh - Alow)

## EDA: 
Exploring the data, there are several peaks that one would expect to be able to use to model. Due to how the light hits and interacts with this sample there are several sine waves that are convoluted in the data. This make it difficult to see if there is actually more happening. By normalizing to the highest absorbance in the data, you can see that the peaks at 27.68, 64.45, and 94.35 cm<sup>-1</sup> have a “U” shape to them. This is most probably due to the underlying issues of the convoluted sine waves. Both 50.1 and 84.85 cm<sup>-1</sup> do not have these issues. I will be using the peak at 50.1 cm<sup>-1</sup> to build the model as the peak at 84.85 cm<sup>-1</sup> was used to develop the percent converted values.

## Supervised Learning: 
After exploring the data, there was a clear linear dependence on one of the peaks. I chose to use a linear regression and model it at 50.1 cm<sup>-1</sup>. This will allow the researcher to model their data accordingly.
