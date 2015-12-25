---
layout: article
title:  "Cardiac Output Prediction from Arterial Blood Pressure Waveforms"
excerpt: "Cardiac Output Prediction from Arterial Blood Pressure Waveforms"
categories: projects
tags: [icm, MATLAB]
toc: true
work: "EN.580.431"
comments: true
featured: true
image:
 teaser: icm_cardiac-teaser.png
 feature: icm_cardiac-feature.png
---

<h2 class="section-heading">Introduction</h2>
Cardiac output (CO) is a global blood flow parameter of interest in hemodynamics, as it indicates how efficiently the heart is meeting the demands of the body. CO also serves as an important metric in diagnosing circulatory system diseases such as ischemia, hypertension, and heart failure. Unfortunately, CO cannot be measured directly in a noninvasive manner. Measurement of CO by thermodilution (TCO) involves inserting a catheter into the pulmonary artery, which is only done for critically ill patients in the intensive care unit (ICU). Algorithms in the past have sought to estimate CO from peripheral arterial blood pressure (ABP) waveforms, but no single method has emerged as a leading candidate for clinical use. In this project, we reproduced results from Sun et al. 2009, Parlikar et al. 2007, and investigated how Co-from-ABP algorithms can perform differently in evaluating ABP waveforms. Using a large number of radial ABP waveform segments from the i2b2/MIMIC2 Waveform Database, we constructed two patient cohorts: patients with chronic ischemic heart disease, and patients without chronic ischemic heart disease. Our results indicate that algorithms that measure intra-beat variations (Liljestrand \& Zander) can accurately estimate CO in healthy patients, while algorithms that measure intra-beat variations (Parlikar) can accurately estimate CO in ischemic patients. Moreover, our analysis gives broad insight into how different Co-from-ABP algorithms can be tailored for different clinical data subtypes of patients in the ICU.


<h2 class="section-heading">Background</h2>
<h3 class="section-heading">Circulatory System</h3>
The circulatory system forms a closed loop in which blood flows to carry oxygen from the lungs to the tissues throughout the body and to carry carbon dioxide back to the lungs. Specifically, the left side of the heart pumps oxygen-rich blood into the systemic arteries, and nutrients diffuse to tissue in the capillaries. The oxygen-depleted blood would then return to the heart via the systemic veins and the right side of the heart would pump this blood into the pulmonary arteries to be distributed in the lungs. The oxygen-rich blood then returns to the left side of the heart via the pulmonary veins. Blood cells transit the full circuit in about one minute.

<h3 class="section-heading">Windkessel Model</h3>
In the late 1800s, German physiologist Otto Frank formulated one of the earliest models of the heart and systemic arterial system, known as the Windkessel model. The Windkessel model describes the load against the heart pumping blood throughout the systemic arterial system and the relationship between blood pressure and stroke volume in the aorta, as a closed hydraulic circuit. 

A simple model of a blood vessel assumes it has a resistance R, and that a blood flow Q is linearly proportional to the pressure drop P along the vessel. This relationship is similar to Ohm's Law, where a drop in electric potential across the resistor is linear proportional to the voltage. In Frank's 2-Element Windkessel model, as water is pumped into the chamber, the water both compresses the air in the pocket and pushes water out of the chamber, back to the pump. The compressibility of the air in the pocket simulates arterial compliance, the elasticity and extensibility of the major artery, as blood is pumped into it by the heart ventricle. The resistance that water encounters while leaving the Windkessel and flowing back to the pump, simulates the total peripheral resistance (TPR), the resistance to flow encountered by the blood as it flows through the arterial tree.

The basic 2-element Windkessel model calculates the exponential pressure curve determined by the systolic and diastolic phases of the cardiac cycle. The model assumes that the cardiac cycle starts at systole, and that the flow of blood in the blood vessel follows Pouseuille's Law. As the number of elements in the model increases, the model accounts for new physiological factors. The 2-element WindKessel takes into account the effect of arterial compliance and TPR, where in the electrical analog, the arterial compliance is represented as a capacitor, and TPR is represented as an energy dissipating resistor. The flow of blood Q from the heart is analogous to current flowing in the circuit, and the blood pressure P in the aorta is modeled as a time-varying electric potential. The following differential equation can be used to model blood flow and pressure:

$$ Q(t) = \frac{P(t)}{R} + C\frac{dP(t)}{dt} $$


<h2 class="section-heading">Materials and Methods</h2>

Sun et al. 2009 and Parlikar et al. 2007 were used as the basis of our mathematical derivations for the intra/inter-beat averaged model.

<h3 class="section-heading">Intra-beat Averaged Models</h3>
The differential equation representing the Windkessel model is given by:

$$ Q(t) = \frac{P(t)}{R} + C\frac{dP(t)}{dt} $$

where $$P(t)$$ represents arterial blood pressure at the aortic root at time t, $$R$$ is TPR, and $$Q(t)$$ is blood flow. $$Q(t)$$ can also be seen as an impulsive current source that deposits a stroke volume $$(SV_{n})$$ into the systemic arterial system in the $$n^{th}$$ cardiac cycle.

$$ Q(t) =  \sum_{n} SV_{n} \cdot \delta(t-t_{n})$$

where $$t_{n}$$ is the onset time of the $$n^{th}$$ beat and $$\delta(t)$$ is the unit Dirac impulse. By integrating the original equation over the ejection phase, we obtain:

$$ SV_{n} = C \cdot PP_{n} $$

$$PP_{n}$$ is the peripheral pulse pressure of the $$n^{th}$$ cardiac cycle. In the 2-element Windkessel model, $$PP_{n}$$ can be calculated as the difference between systolic and distolic aterial pressure: $$PP_{n} = SAP_{n} - DAP_{n}$$. This calculation varies across different intra-beat averaged models. Using $$T_{n}$$ as the period of the $$n^{th}$$ cardiac cycle, cardiac output is given by:

$$ CO_{n} = \frac{SV_{n}}{T_{n}} = C_{n} \frac{PP_{n}}{T_{n}} $$

The three intra-beat averaged models used is the 2-element Windkessel described above, mean arterial pressure (MAP), and the Liljestrand & Zander algorithm.

<h3 class="section-heading">Inter-beat Averaged Models</h3>
To taken into account beat-to-beat variations in ABP waveforms, we can average the Windkessel differential equation over the cardiac cycle. For the $$n^{th}$$ beat we obtain:

$$ CO_{n} = \frac{\bar{P}_{n}}{R_{n}} + C_{n}\frac{\Delta P_{n}}{T_{n}} $$

where $$T_{n}$$ is the period of the $$n^{th}$$ cardiac cycle, $$\Delta P_{n}$$ is the beat-to-beat pressure change at onset times, and $$\bar{P}_{n}$$ is the average ABP over the cycle. In steady-state, the change in ABP is proportional to the change in volume of the circulation, which is equal to the volume of blood ejected from the heart, or stroke volume. 

$$ \Delta P_{n} = P(t_{n+1}) - P(t_{n}) $$

$$ \bar{P}_{n} = \frac{1}{T_{n}} \int_{t_{n}}^{t_{n+1}} P(t) dt $$

Using the CO estimation from the intra-beat average model, we can rewrite our relation over the $$n^{th}$$ cycle.

$$ C_{n} \frac{PP_{n}}{T_{n}} = \frac{\bar{P}_{n}}{R_{n}} + C_{n}\frac{\Delta P_{n}}{T_{n}} $$

$$ \frac{PP_{n}}{T_{n}} = \frac{\bar{P}_{n}}{\tau_{n}} + \frac{\Delta P_{n}}{T_{n}} $$

where $$\tau_{n}$$ is the time constant  $$R_{n}C_{n}$$. From Parlikar et al 2007, a different calculation for $$ PP_{n} $$ was used to correct for wave reflections by assuming a triangular pulse shape generated from $$\alpha = 2$$.

$$ PP_{n} = \alpha(\bar{P}_{n} - DAP_{n}) $$

Since $$R_{n}C_{n}$$ cannot be observed, we derived an expresdsion for $$\tau_{n}$$ in terms of $$PP_{n}$$, $$\bar{P}_{n}$$, $$\Delta P_{n}$$, and $$T_{n}$$, which can be observed in the ABP waveform.

$$ \frac{PP_{n}}{T_{n}} = \frac{\bar{P}_{n}}{\tau_{n}} + \frac{\Delta P_{n}}{T_{n}} $$

$$ PP_{n}\tau_{n} = \bar{P}_{n}T_{n} + \Delta P_{n}\tau_{n} $$

$$ \tau_{n}(\Delta P_{n} - PP_{n}) = - \bar{P}_{n}T_{n} $$

$$ \tau_{n} = -\frac{\bar{P}_{n}T_{n}}{\Delta P_{n} - PP_{n}} = \frac{\bar{P}_{n}T_{n}}{PP_{n} - \Delta P_{n}} $$

The only inter-beat averaged model used was the Parlikar estimator.

<h3 class="section-heading">Clinical and Waveform Data Preprocessing</h3>
In Sun et al. 2009, we reproduced "Figure 1," which plots 20 pulses of ABP waveforms. This figure also plots features such as the onset of each beat, end of systole (estimated from beat period), and end of systole (estimated from the lowest non-negative slope method). In addition, we reproduced "Figure 4," which plots a time series of CO from ABP measurements over a 50-hour interval. CO was estimated using the Liljestrand & Zander algorithm and the Parlikar algorithm in MATLAB, and then calibrated with episodic TCO measurements. Time series data for Pulse Pressure (PP), Mean Arterial Pressure (MAP), and Heart Rate (HR) were also plotted. Each subplot is annotated with a stem plot of values taken from TPR.

<h4 class="section-heading">Figure 1 Sun et al 2009</h4>
To replicate Figure 1 in Sun et al. 2009, in MATLAB, we extracted ABP waveform data of Patient 20 (s200) from the MIMIC2 Waveform Database. In the text file, the first column represents the time (in seconds) at which ABP is sampled, and the second column represents ABP values in mmHG. Each row represents a sample taken in a sphygmomanometer, and Sun et al. 2009 estimates that the waveform data has a sample rate of 125. To obtain the first 20 pulses starting at the 10th hour, we noted that there are 4500000 samples in 10 hours, and that about 1250 samples were needed to capture the first 20 pulses of ABP waveforms. These calculations (samples to hours, hours to samples) were used to index and subset through the waveform text file in MATLAB. The function wabpresults obtains the onset sample time of the pulse in MATlab. From the onset of the pulse (trough), we used the function abpfeature to calculate an ABP Feature Matrix, which contains end of systole times from both systole estimators. 

<figure class>
	<img src="{{ site.url }}/images/icm_cardiac-0.png">
</figure>

From these calculations, we plotted a time series of ABP waveforms with markers for the onset and end of systole times. Empirically, when we graphed the time series with 1250 samples, we only captured a fraction of the 20 pulses. The trough in a ABP waveform models the heart muscle resting between each pulse; as a result, we used a for loop was used to iteratively increase the sample rate until wabpresults returned a vector of 20 onset sample times. This procedure was repeated for Patient 20 starting at 11 hours, Patient 138, Patient 214, and Patient 217.

<h4 class="section-heading">Figure 4 Sun et al 2009</h4>
For the first 12 hours, similar to Figure 1, we found onset sample times for pulse using wadpresults, and calculated the ABP Feature Matrix using abpfeature. The function jSQI returns a binary signal quality assessment of each beat from the ABP Feature Matrix and onset sample times, with “0” being good and “1” being bad. The function estimateCOv3 estimates CO from the ABP Feature Matrix, jSQI binary values, onset sample times for pulse, and a switch statement for different CO estimators.

In addition to extracting ABP waveform data from the MIMIC2 Waveform Database, we also extracted clinical data from the MIMIC2 Clinical Database, which contains episodic TCO measurements. Within the clinical data text file, we extracted the time (in minutes) at which the first CO measurement occurred, and the CO measurement itself. Unlike the waveform data, where ABP measurements were not uniformly sampled, clinical data measurements were sampled every 30 seconds. By dividing the first CO measurement in the clinical data with its corresponding CO measurement in waveform data, we created a calibration factor to scale our estimated CO. For loops were used to index the samples in the ABP waveform data by time in seconds.

We plotted the time series of the calculated CO, PP, MAP, and HR from estimateCOv3, in addition to stem plots of the corresponding measurements from episodic TCO. In the clinical data, most measurements of CO were 0. When the sphygmomanometer recorded a value for CO, the clinical data was subsetted to also obtain PP, MAP, and Heart Rate at that time. This procedure was reproduced with patients 20 & 214.

<h4 class="section-heading">Inter vs. Intra beat comparison</h4>
Within i2b2, we selected 5 patients with chronic ischemic heart disease and 5 patients without chronic ischemic heart disease within our cohort. Patients were selected within i2b2 by searching for patients with the ICD9 code corresponding to chronic ischemic heart disease, and cross-checked with a patients that had CO TCO measurements.

<figure class>
	<img src="{{ site.url }}/images/icm_cardiac-14.png">
</figure>

<h2 class="section-heading">Results & Discussion</h2>
<h3 class="section-heading">ABP Waveform Visualization & Measuring End of Systolic </h3>
A goal of our project was to visually mark these key features, such as onset point times and the end of systole, as done in Figure 1 of Sun et al. 2009. Onset point, labeled by an asterisk, was determined by our algorithm as the lowest arterial blood pressure point in each beat. This was clearly indicated in each patient waveform. The end of systole was identified in two manners: 1) $$0.3 \cdot \sqrt{\text{beat-period}}$$, and 2) the point after systolic peak with the lowest non-negative slope. These methods were indicated in the waveform plot with X’s and O’s respectively. Interestingly enough, the accuracy of both methods in identifying the end of systole among different patients vary. In patients 20, 138, 214, and 217 of figure 4, estimating the end of systole with the lowest non-negative slope proved to be a superior method than estimating by beat-period. Visually, the O's in each waveform plot was typically placed at the trough of each beat, a good estimator of the end of systole. However, X’s were placed in a much less precise manner, as most clearly shown in patient 138 and 214. These X’s were placed in the rapid decrease in ABP prior to the trough.

<figure class>
	<img src="{{ site.url }}/images/icm_cardiac-1.png">
</figure>

Interestingly, the accuracy of both methods in identifying the end of systole among different patients vary. In patients 20, 138, 214, and 217 of our Figure 1, estimating the end of systole with the lowest non-negative slope proved to be a superior method than estimating by beat-period. Visually, the O's in each waveform plot was typically placed at the trough of each beat, a good estimator of the end of systole. However, X’s were placed in a much less precise manner, as most clearly shown in patient 138 and 214. These X’s were placed in the rapid decrease in ABP prior to the trough.

The issue with estimating the end of systole due to beat period is that this methodology is heavily parameter dependent. Every patient will vary in CO, ABP, HR etc, causing great variability in the beat period. This method faced difficulty with waveform data in patients 138 and 214 and would mark the end of systole too early due to a longer cardiac cycle. Waveform data for patient 20 and 217 was more accurately marked. Estimating the end of systole by measuring the lowest non-negative slope is the best method since troughs will have this property.

<h3 class="section-heading">Comparison of Intra-beat averaged models</h3>
Figures 2 show four plots, the continuous CO from ABP estimated by the Liljestrand algorithm, PP, MAP, and HR. Stem plots were also placed throughout the plots to represent discrete episodic thermodilution CO measurements as controls.

Comparison between cardiac output by the Liljestrand estimator and mean arterial pressure demonstrates the superiority of the former. Compared to mean arterial pressure, the Liljestrand estimator cardiac output is more sensitive to major changes in CO and enhances the display vital signs. This estimator also unfortunately is much more sensitive to spikes, as noted throughout the plots in figure 3 of patient 20 and 214. There seems to be little correlation between heart rate and cardiac output.

<figure class="half">
	<img src="{{ site.url }}/images/icm_cardiac-2.png">
	<img src="{{ site.url }}/images/icm_cardiac-3.png">
</figure>

In figure 4, we also compared different estimators (Liljestrand, MAP, Windkessel) to the cardiac output measured by thermodilution in patients 20 and 214. For patient 20, Liljestrand cardiac output estimator demonstrated the least sample variance, while MAP demonstrated the most. For patients 214, MAP cardiac output estimator demonstrated the least sample variance, whereas the Windkessel model showed the most.

<figure class="half">
	<img src="{{ site.url }}/images/icm_cardiac-4.png">
	<img src="{{ site.url }}/images/icm_cardiac-5.png">
</figure>

In figures 5, we plotted a regression line through mean arterial pressure values and compliance values at the onset of each pulse for patients 20 and 214. In figures 6, CO, PP, MAP, and HR were measured based on the Parlikar estimator. All four displayed measurements demonstrated similar patterns and changes throughout time. Cardiac output was the most noisy and sensitive, whereas heart rate was the least noisiest. Additionally, cardiac output was more sensitive to high peaks in heart rate, as opposed to PP and MAP.

<figure class>
	<img src="{{ site.url }}/images/icm_cardiac-6.png">
</figure>

<h3 class="section-heading">Comparison of Intra and Inter-beat averaged models</h3>
<h4 class="section-heading">Liljestrand & Zander vs. Parlikar Estimator in Patients 20, 214</h4>
In figures 1 and 5, we plotted a regression line through mean arterial pressure values and compliance values at the onset of each pulse for patients 20 and 214.

In figures 3 and 4, CO, PP, MAP, and HR are measured based on the Parlikar estimator. All four displayed measurements demonstrate similar patterns and changes throughout time. Cardiac output is the most noisy and sensitive, whereas heart rate is the least noisiest. Additionally, cardiac output is more sensitive to high peaks in heart rate, as opposed to PP and MAP.

<figure class="half">
	<img src="{{ site.url }}/images/icm_cardiac-9.png">
	<img src="{{ site.url }}/images/icm_cardiac-10.png">
</figure>

<figure class="half">
	<img src="{{ site.url }}/images/icm_cardiac-7.png">
	<img src="{{ site.url }}/images/icm_cardiac-8.png">
</figure>

In figures 3 and 6, qualitatively, the Parlikar cardiac output estimates were noisier than the Liljestrand cardiac output estimates, as seen in the abundance of highly variable peaks. In figure 9, we quantitatively compared Parlikar and Liljestrand cardiac output estimates using sample variance from known thermodilution values. For Patient 20, we see that the Parlikar performed much worse than Liljestrand, however, in Patient 214, Parlikar performed relatively better than Liljestrand, though its sample variance was still high. This can be explained by how the Parlikar estimator uses beat­to­beat variations to calculate cardiac output (ΔPn). ΔP n ​greatly contributes to the noise seen in figures 1 and 5, as any variation between beats will amplify the signal. Since Liljestrand uses only intracycle features (SAP, DAP), changes in arterial blood pressure are not amplified. It is interesting to note that Patient 214 belonged to a cohort in i2b2 that has circulatory system diseases, while Patient 20 does not. Patients such as Patient 214 with circulatory system diseases will have greater variability in arterial blood pressure at onset, which Parlikar accounts for using ΔP n. Therefore, Parlikar estimator produces noisier signals because it accounts for patients with beat­to­beat variations. This also explains why Parlikar performed better compared to Liljestrand in Patient 214. In addition, Parlikar notes that algorithms that use intracycle features are only valid for patients with a cyclic steady state, such as Patient 20, who doesn’t have circulatory system diseases. This finding helped motivate the way we constructed out cohorts later on.

<figure class="half">
	<img src="{{ site.url }}/images/icm_cardiac-11.png">
	<img src="{{ site.url }}/images/icm_cardiac-12.png">
</figure>

<figure class>
	<img src="{{ site.url }}/images/icm_cardiac-13.png">
</figure>

Figures 8 and 9 show total peripheral resistance as determined by the Parlikar estimator. Similar to cardiac output data estimated by Parlikar, estimated TPR is very noisy. Furthermore, our data shows that high values of cardiac output at a specific time are correlated with a decrease in TPR. This agrees with the equation we utilized to estimate TPR; TPR and cardiac output are inversely related.

<figure class="half">
	<img src="{{ site.url }}/images/icm_cardiac-15.png">
	<img src="{{ site.url }}/images/icm_cardiac-16.png">
</figure>

The total peripheral resistance (TPR) is the net resistance to flow seen by the heart, and is the ratio of mean ABP to CO (in close analogy to electrical resistance, which is the ratio of potential difference to current). TPR plays an integral role in determining blood pressure, and as a result can be an indicator for many cardiovascular diseases such as hypertension or atherosclerosis. It can also give some indication on the fluid dynamics and composition of the blood. For example, a more viscous blood flow due to clotting factors or important blood components would cause more resistance to flow.


<h4 class="section-heading">Liljestrand & Zander vs. Parlikar Estimator in patients with chronic ischemic heart disease</h4>
We selected two cohorts: patients with chronic ischemic heart disease, and patients without chronic ischemic heart disease. Although our second cohort reflected patients diagnosed without hypertensive disease, it does not exclude patients with other circulatory diseases, such as heart failure, cardiac dysrhythmia, or cardiomyopathy. In fact, there were only two patients within the entire database without any type of circulatory disease. These alternative circulatory diseases may have a larger impact on cardiac output than chronic ischemic heart disease. While ischemic heart disease is associated with high arterial blood pressure, diseases such as heart failure is associated with low stroke volume, changing a parameter for cardiac output that is not accounted for by our ABP estimators. As a consequence, this may have confounded our cohorts. We also noticed that, for our cohort representing chronic ischemic heart disease patients, five of the six patients had mean systolic blood pressure lower than 110mmHg, which is below the commonly accepted reading for healthy systolic blood pressure (120mmHg). We picked this cohort under the impression that these patients would have a higher than normal blood pressure. However, based only on the mean systolic blood pressure, we found that these patients do not reflect expected blood pressure readings.

Using the Parlikar and Liljestrand algorithms, we were able to generate plots that allowed us to make comparisons between the two algorithms shown in the following figures:

<figure class="half">
	<img src="{{ site.url }}/images/icm_cardiac-17.png">
	<img src="{{ site.url }}/images/icm_cardiac-18.png">
</figure>

Based on these representative plots (Figures 1 and 3) of the Liljestrand and Parlikar algorithms for both healthy and ischemic heart disease patients, it seems that Parlikar qualitatively performs better as a cardiac output estimator for ischemic patients than it does for healthy patients. This is also reflected in the variances (Figures 5 and 6), in which Parlikar had higher variance in four of the five healthy patients (one healthy patient not shown due to y­scaling) but had lower variance in four of the five ischemic patients. Not much can be said regarding the difference between the total peripheral resistances between the healthy and ischemic patients. These results corroborate our hypothesis that because the Parlikar algorithm takes into account intra­ and inter­beat variations, it should be a better estimator for patients with chronic ischemic heart disease.

<figure class="half">
	<img src="{{ site.url }}/images/icm_cardiac-19.png">
	<img src="{{ site.url }}/images/icm_cardiac-20.png">
</figure>

<figure class="half">
	<img src="{{ site.url }}/images/icm_cardiac-21.png">
	<img src="{{ site.url }}/images/icm_cardiac-22.png">
</figure>


<h4 class="section-heading">Challenges</h4>
Some interesting things in working with this data set are the possible confounding factors that may arise when measuring this data, such as previous health conditions, hospital room environment, nurse/doctor care, etc.

We faced several challenges while working with patient data. The first challenge was the noise or missing information in the given data sets. Patient files often revealed inconsistent time intervals, forcing our group had to write an algorithm that would manually look for patient data that shared the same time interval. A second challenge was the conversion of sample and time. The sample rate was not consistently sampling at 125 in the ABP waveform data, and as a result, for loops were often used to index matrices by time. This challenge added a second layer of difficulty when trying to index through the clinical data, as the clinical data sampled every 60 seconds instead of every ~1/125 second. A third challenge was presented when we tried to compare the cardiac outputs of different estimation algorithms. Without calibration, plotting the cardiac outputs from different algorithms was nearly impossible due to a wide variety of pressure values. Ultimately, we needed to rescale each algorithm using the C2 method to effectively compare cardiac outputs.