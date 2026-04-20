# AgnirathApp_FinalChallenge
Aryaman Kashyap [AM25B005]

System Architecture:
Processor:	AMD Ryzen 7 8845HS w/ Radeon 780M Graphics (3.80 GHz)
Installed RAM:	16.0 GB (15.3 GB usable)
Graphics card:	NVIDIA GeForce RTX 4050 Laptop GPU (6 GB)
                AMD Radeon(TM) Graphics (417 MB)
Storage	180 GB of 477 GB used
System type	64-bit operating system, x64-based processor

Procedure:

1. Got route data from the ORSM API call that provided ~3400 points on the route pathway from Sasolburg to Zeerust which was a clarity-level of roughly 90 metres per point-to-point difference.
2. This data was then augmented with an SRTM API call that provided point to point local elevation. With this, I calculated the average change in height between each points using this data, and joined with the total change in distance calculated by the haversine formula which used the co-ordinates and factored in the curvature of the planet I calcuted the average slope angle of point-to-point traversal. 
4. At this point, we were done with data collection so I simply exported our route data as 'path.csv'.
5.  Now, we begin to define helper functions that will deal with the physics of the problem in this setup, all of which was derived in the Foundation Section. We start with P_solar which has given to be defined as a Gauusian Distribution in time with a max irradiance of 1703 W/m**2 at mid-day, this was enough to derive irradiance at any time t.
6.  Made assumptions for physical quantities, namely k,mu and various different efficiencies. This was informed by the previous sections and also this link **https://teamsolaris.com/s11/**, which details useful information from a competitive Solar Car. I more or less tried my best not to tamper with these assumptions once I had set them in order to not cheat and improve the performance of my model by simply changing these values. Since these valuea are reasonable we must be able to build a competitve car with these numbers as restrictions.
7.  Used the constants and simply defined helper function that compute different types of powers and update the SoC of the car.
8.  We now build the optimizers, but before that I needed to build a function that "drives" our car given a certain velocity profile. I tested this function by providing a velocity profile of constant speed of 80 kmph and the function outputted reasonable values for noth final SoC and time taken.
9.  Here we start building our optimizers -- I chose to define my objective function for the first phase of the race as a dual-dimensional quadratic of time taken to finish the race and the SoC remaining with each having their respective weights. I also added a term of R_control which was to ensure that our model did not provide an impossible to follow, non-continuous velocity profile.
10.  I then used the SLSQP (for reasons mentioned in previous sections) to optimize this previously defined Objective function to produce the required velocity profile. This basically ends phase 1 of the race.
11.  We now factor in the 30 minute break. The model for phase 2 of the race is quite simple and has already been coded in the previous sections with a few changes. No we need to include a 5-min stop between each loop and also consider varying solar irradiance unlike the previous question. We approximate P_solar here by using mid-time of the loop pitstops. This is a valid approximation, since the pitstop is for only 5 minutes.
12.  We are now basically done, we initialize our 2nd model with the end results of our 1st model and let the code run.
13.  I have attached meaningful plots related to the project both in the ipynb and as images on the repository. 
