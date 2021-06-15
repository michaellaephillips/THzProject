## Data: 
Stability of pharmaceuticals is of the utmost importance. The Food and Drug Administration (FDA) requires that companies know and study the forms of their drug products to ensure safety and efficiency of their products. Many solids can exist as polymorphs, meaning they can have two or more crystalline forms, but are composed of the same material. For example, diamonds and graphite are both made of carbon, but the atoms are arranged differently. This affects their physical properties, for diamonds and graphite their hardness is most notably different.

![alt text](https://github.com/michaellaephillips/THzProject/blob/master/Diamond_and_graphite.jpg?raw=true)
Image from: This file is a composite of Image:GraphiteUSGOV.jpg (public domain), en:Image:Brillanten.jpg (GFDL), and parts of Image:Eight_Allotropes_of_Carbon.png (GFDL) taken from wikipedia.

Several drug products on the market have had issues with polymorphism. Including Ritonavir, a drug used for treating human immunodeficiency virus (HIV). In the 1980s, Abbott Labs had an issue with the effectiveness of their drug compound as theirs had changed crystal forms. This new form of Ritonavir was not effective in treating HIV and thus had to be removed from the market until the problem was solved. This caused Abbot to shut down several manufacturing plants, due to trace amounts of the new form catalyzing their desired drug product. As such, many new techniques have been developed to monitor crystal transforms, including THz spectroscopy. THz spectroscopy is sensitive to the arrangement of the molecules in space.

![alt text](https://github.com/michaellaephillips/THzProject/blob/master/Screen%20Shot%202021-06-14%20at%207.12.05%20PM.png)

This work uses a proprietary, organic crystalline solid as a model system. Two forms are known to exist of this solid and it is unknown what causes it to transform. In order to run different experiments to test the sample, a model must be built to determine the percent converted to form B. This will allow researchers to compare their samples and determine what percentage of their material they have.


## Data Cleaning: 
This data was collected using a THz spectrometer and the raw Single Beam data was exported as a CSV into MatLab. The data was converted using proprietary scripts into THz absorbance spectra. The first and last spectra were collected. The starting material is Form A and the ending material is Form B.  

![alt text](https://github.com/michaellaephillips/THzProject/blob/master/startend.png?raw=true)

Unfortunately, we were unable to collect concentration information, but it was estimated by using the spectral features to approximate the percentage converted. 
Several assumptions were made:
1. Beer's Law is true: A=ebc. Absorbance is additive and proportionate to concentration.
2. The starting spectrum was 100% Form A and ending spectrum was 100% Form B.
3. Form A immediately becomes Form B and there is not an intermediate.

First, the maximum for the peaks of interest were located. Knowing the peaks associated with the forms allows us to interpret the concentration. The percent transformed can be determined using the equation below where Ai is a given absorbance in a spectra at a certain wavelength, by dividing by the highest absorbance (Ahigh) we can get a rough percent, but due to instrumental fluctuation and sample changes, we can adjust a bit more by subtracting the low at the wavelength from both the numerator and denominator. 

% transformed = (A<sub>i</sub> - A<sub>low</sub>)/(A<sub>high</sub> - A<sub>low</sub>)

## EDA: 
Exploring the data, there are several peaks that one would expect to be able to use to model: 27.68, 50.11, 64.45, 84.85, and 94.35 cm<sup>-1</sup>. By normalizing to the highest absorbance fo each peak, it is easier to compare them. The concentrations associated with Form A over time is shown below. Here it can be seen that there almost is no telling if the starting material is in fact pure Form A. This suggests that the relative amounts are going to be incorrect. The 27.68 peak is also very high in noise. This is due to the low signal to noise ration at that wavelength. Lower intensity means higher variation in the measurement. Finally, there is a dip below 0, this further siggests that there is an issue with the normalization.

![alt text](https://github.com/michaellaephillips/THzProject/blob/master/formA.png)

The peaks associated with Form B had much higher signal, but there were more issues with these peaks. The peak at 94.35 was not useable. As shown below, there is a large negative dip. Looking back at the orginal spectra above, you can see that it appears to be three peaks overlapping one another. This is causing interference in this peak and is the reason the peak does not behave well. 

![alt text](https://github.com/michaellaephillips/THzProject/blob/master/Percentconvertissue.png)

Once removed, the peaks behaved very well. I expected these to overlap and they nearly do. 

![alt text](https://github.com/michaellaephillips/THzProject/blob/master/FormB.png)

Now to compare the Form A and Form B peaks, we see some evidence that there is more going on than what meets the eyes. If Form A was in fact only converting to Form B, you would expect to see an intersection at 50%. We do not see that. This suggests that we actually may have an intermediate where Form A turns into A' before becoming Form B. Since the spectra collected at the halfway point does not show evidence of features related to another form, I have to assume this is not detectable with this instrumentation. This means it could be an amorphous form. This is a state similar to glass, where it is solid, but it has properties of a liquid. Amorphous materials lack a consistent structure and thus cannot be detected by THz well.

![alt text](https://github.com/michaellaephillips/THzProject/blob/master/endperc.png?raw=true)

Looking at the absorbances with concentration of Form B, I removed the 27.68 peak due to noise, the 94.35 peak due to the interference, and the 84.85 because I used it to calculate the percentage of Form B. Below it can be seen that the peak at 64.65 cm<sup>-1</sup> has a “U” shape. This is most probably due to the starting material not being pure and the material and that Form A is very unstable and is rapidly converting into the transition state. Both 50.1 cm<sup>-1</sup> does not have these issues. I will be using the peak at 50.1 cm<sup>-1</sup> to build the model. 

![alt text](https://github.com/michaellaephillips/THzProject/blob/master/normalizedAasfuncofFormB.png?raw=true)

## Supervised Learning: 
After exploring the data, there was a clear linear dependence on one of the peaks. I chose to use a linear regression and model it at 50.1 cm<sup>-1</sup>. In the chemistry domain there are differences in terms of building a model. You do not always solve for x. Dependent variables are always on the y-axis and indepenant variables are on the x-axis. If I were make a sample to test my model I can control only the concentration, so it is an independent variable and is on the x-axis. If I receive an unknown concentration and I run this unknown sample. I would use the abosrbance to solve for the concentration.

![alt text](https://github.com/michaellaephillips/THzProject/blob/master/linearmodel.png?raw=true)

Above shows the model with the raw data. I built the model after splitting the data using an 80-20 Train-test split. It is clear that the model appears to fit the data well. To further investigate this, I compared the actual values of the absorbance with the predicted. The line trace shown in the below graph falls on top of the data, meaning that the predictions are similar. At the lower concentrations, there is a bit of scatter. This could be due to how we normalized or the other various issues I mentioned above. The regression statistics are shown below and as well as plot comparing the actual values of the test data to the calculated values and there is very little error. 

![alt text](https://github.com/michaellaephillips/THzProject/blob/master/ActualVsPrediction.png?raw=true)

Mean Absolute Error: 0.0013480936167417135 <br>
Mean Squared Error: 4.009023149318015e-06 <br>
Root Mean Squared Error: 0.0020022545166182085 <br>
Mean Absolute Percent Error: 0.002269403582822871 <br>


## Future work: 
In the future it would be nice to re run the experiment and evaluate the data. This time having a secondary way of obtaining the concentrations. I also would like to have a method to model the transition state that I am unable to observe properly in this model. Better understanding of it would be useful in building this model.

