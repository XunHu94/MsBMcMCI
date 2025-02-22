# MsBMW
A geostatistical seismic inversion technique based on the multi-scale blocking moving window (MsBMW) algorithm. 

![Key_parameters](https://github.com/XunHu94/MsBMW/blob/main/Key_parameters.png)  
**Picture:** The changes of key parameters in the iterative process using MsBMW. (a) RMSE, (b) AR. (c) Temperature, (d) Grid level, and (e) normalization of the above 4 key parameters.

![Generated_models](https://github.com/XunHu94/MsBMW/blob/main/Generated_models.png)  
**Picture:** The generated models at different scales using the MsBMW, including lithofacies, density, velocity, and synthetic seismic.

![Results_of_different_AR_values](https://github.com/XunHu94/MsBMW/blob/main/Results_of_different_AR_values.png)  
**Picture:** Comparison of inversion results of different range of the (AR_G, AR_T) in MsBMW. (a) RMSE. (b) AR. (c) Temperature. (d) Time.

## Licenses
All material is made available under MIT license. You can use, redistribute, and adapt the material for non-commercial purposes, as long as you give appropriate credit by citing our paper and indicating any changes that you've made.

## System requirements
* Hardware requirements: 1.6 GHz or faster processor, 1 GB of RAM  
* Program language: Fortran  
* Software required: VS 2013 or newer, Fortran 2013 or newer  

## Make input data?
We need to prepare 8 files as input data, including trining image file "TI", 4 conditional data files (seismic data file "SeismicData", well interpretation facies data file "CondFacies", well interpretation density data file "CondDen", and well interpretation Longitudinal velocity data file "CondVp"), Seismic wavelet file, "SeismicWave", template file "template5_1_7", and input parameters file "MsBMW.par".

### (1) Make trining image file "TI"
The training image is generated in Petrel software, and export them into one file as model properties with "Gslib" format. An Gslib format example of the exported file is "TI".

First lines of the exported file are like:  
> PETREL: Properties  
1  
Facies  
0.000000  
0.000000  
0.000000  
1.000000  
...  
1.000000   
0.000000

### (2) Make seismic date file "SeismicData"
At first, we import the seismic data in "SEG" or "SEGY" format into the Petrel software. Then, we resampling seismic data into the model. Finally, we export them into one file as model properties with "Gslib" format as well as training image file "TI". An Gslib format example of the exported file is "SeismicData".

First lines of the exported file are like:  
> PETREL:Property  
1  
Seismic_SNY_1  
  4.5725177E-03  
 -3.2153115E-02  
 -3.6438044E-02  
 -4.7419112E-02  
  ...  
  4.6712905E-04  
 -2.1066867E-02  
 
 ### (3) Make 3 type of well interpretation data file: "CondFacies", "CondDen", and "CondVp"
 At first, we import the well log in "las" or "txt" format into Petrel software. After well interpretation and coarsening the interpretation data into model,  we export them into one file as model properties with "Gslib" format. Note that the three exported file include IJK cell values. For example, one of the Gslib format file is "CondDen".
 
 First lines of the exported file are like:  
> PETREL: Properties  
4  
i_index unit1 scale1  
j_index unit1 scale1  
k_index unit1 scale1  
density unit1 scale1  
40 1 80 2.379229  
120 1 80 2.366748  
40 1 79 2.427364  
120 1 79 2.394222  
...  
40 1 1 2.126683  
120 1 1 2.430258 a 

### (4) Make seismic wavelet file "SeismicWavelet"
The file is formed by the values of the wavelet sampling point sequence. Note that the time interval of sampling in seismic wavalet and the depth of a single grid in model should be consistent. 

 First lines of the exported file are like:  
> -0.00192775  
-0.0031539  
-0.00505651  
-0.00794273  
...  
-0.01841405  
-0.02716234  

### (5) Make template file "template5_1_7"

> First lines of the exported file are like:  
 Template of ** data locations  
3  
x-relative coordinate  
y-relative coordinate  
z-relative coordinate  
-2	0	-3  
-2	0	-2  
-2	0	-1  
-2	0	0  
...  
2	0	2  
2	0	3  

### (6) Make input parameters file "MsBMW.par"
We can set several key parameters in "MsBMW.par". Such as the upper bound of average acceptance rate (AR) parameter (i.e., Cooling criteria βT), the lower bound of AR paremeter (i.e., The grid degradation criteria βG), grid level (i.e., the number of multi-grid), initial temperature, the maximum number of iterations per chain, the search scope of TI, data events match rate τ, and so on.

> Parameters for MsBMW  
> 			      ********************  
> START OF PARAMETERS:  
> Cond_Facies    -Lithofacies conditional data  
Cond_Den                                                -Density conditional data  
Cond_Vp                                                  -Vp conditional data  
SeismicWavelet                                                       -Seismic wave  
SeismicData                                                      -Actual  seismic data  
RockPhysicsModel                                                             -Rock physics Model  
50                                                              -The number of chains  
1                                                              -"ObjectiveFuction"：1--RMSE，2--CorrelationCoefficient
2                                                              -"assignway"：1--single grid，2--patch  
1       2       3        4                                   -columns for "x," "y," "z," variable  
2                                                               -number of categories  
0 1                                                            -category codes  
1                                                              -number of realizations to generate  
150     0.5     1                                           -"nx,xmn,xsiz"  
1     0.5     1                                               -"ny,ymn,ysiz"  
80      0.5     1                                             -"nz,zmn,zsiz"  
69569                                                         -random number seed  
template5_1_7                                              -file for data template  
200                                                             -max number of conditioning data  
80000                                                        -The maximum number of iterations per chain  
20000                                                        -The maximum number of iteratons in coarse models  
0.30                                                          -The grid degradation criteria βG  
0.25                                                          -The search scope of TI  
0.90                                                           -Data events match rate τ  
1.0                                                             -Initial temperature  
0.90                                                           -Cooling rate  
0.50                                                             -Cooling criteria βT  
3       0                                                      -number of "mult-grid," number with search trees  
TI                                                             -file for training image  
300     1     160                                            -training image dimensions: "nxtr," "nytr," nztr  
1                                                             -column for training variable  
10      10      10                                          -maximum search radii "(hmax,hmin,vert)"  
0      0       0                                               -angles for search ellipsoid  

 
## Running MsBMW
Once making the input data mentioned above and downloading related codes successfully, you can run MsBMW code.

# Results
All of results are generated in the folder under directory "SIMOUTFL", including Density "DensityOut1", inverted facies "Facies1", Longitudinal wave velocity "VelocityOut1",Parameters "ParametersChain1.txt", synthetic seismic data file "SyntheticRecordOut1".

We can import the 4 file ("Facies1",  "DensityOut1", "VelocityOut1", "SyntheticRecordOut1")  into Petrel software to visualize the model.
