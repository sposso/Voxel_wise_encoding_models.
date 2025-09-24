## Voxel-Wise Encoding Models  
Training fMRI encoders  

Prelimaries:
fMRI: It is a medical imaging technique that measures brain activity indirectly by detecting changes in blood flow and oxygen level (BOLD) using an MRI scanner. 
The reasoning behind using BOLD to measure brain activity is simple. When a region of the brain becomes more active, its neurons use more energy. To compensate for this energy demand,
local blood vessels respond by increasing blood flow to that region, so more oxygen and glucose become available to the neuron. These resulting changes in blood oxygenation and volume induce
changes in the magnetic field,  which is recorded by the MRI scanner. 

How does the blood flow affect a magnetic field? 
Blood contains red blood cells, which in turn contain hemoglobin, a protein responsible for transporting oxygen. When hemoglobin is bound to oxygen, called oxyhemoglobin, it is diamagnetic,
meaning it weakly repels magnetic fields. Oxygenated blood is delivered by the blood flow to active neurons in the brain, where oxygen is consumed to produce energy. As neurons use this oxygen,
oxyhemoglobin is converted into deoxyhemoglobin, which lacks the oxygen molecules. 

Deoxyhemoglobin is paramagnetic, meaning it is attracted to magnetic fields. This paramagnetism causes local disturbances or variations in the otherwise uniform magnetic field used in MRI scanners.
These variations affect the magnetic resonance signals, creating contrasts known as the BOLD effect that allows MRI to indirectly measure brain activity by detecting changes in blood oxygenation
related to neural activity.

### What is a Voxel-Wise Encoding Model (VEM)?  
A Voxel-Wise Encoding Model (VEM) is an approach built on encoding models to map brain representations from fMRI recordings. In essence, these models describe how features of a stimulus 
can predict neural activity in specific brain regions.  
If an encoding model successfully predicts activity in a particular area, it suggests that the information represented by the features is also encoded in that brain region[^1].  
---
### Example  
Imagine you are listening to your favorite podcast while your brain activity is recorded in an MRI scanner.  

- **Stimulus:** The audio from the podcast.  
- **Features:** Characteristics of that audio (e.g., root mean square energy, amplitude envelope, spectral centroid) or latent representations extracted by foundation models
  + such as wav2vec 2.0[^2].
- **Brain recordings:** BOLD recorded by fMRI.   
- **Encoding model:** Regression model that is trained to predict brain activity from the set of features (feature space)
---
### Steps to use a VEM to predict brain activity:
1. Brain activity is recorded while subjects perceive a stimulus or perform a task.
2. A set of features is extracted from  the stimulus across several moments (time points).
3. An encoding model is trained to map the feature space to the brain activity.

#### Upper bound (Noise ceiling) of the model prediction accuracy in each voxel:

The Noise ceiling is linked to the explainable variance. The explainable variance quantifies the fraction of the variance in the data that is consistent 
across repetitions of the same stimulus. When the same stimulus is presented multiple times, not all variations in the recorded data are due to the stimulus
itself; some variations arise from noise or random fluctuations. The explainable variance captures how much of the observed data variability is actually stimulus-driven and 
repeatable. 

An encoding model, which predicts the brain response to a stimulus, produces identical predictions for repeated presentation of the same stimulus. Therefore,  the best such a model
can do is to explain only the consistent stimulus-related part of the variance. The noise ceiling represents the maximum possible accuracy for the model, constrained by measurement
noise and inherent variability.

Mathematically, explainable variance is calculated as:

$$
\text{Explainable Variance} = \frac{\mathrm{Var}(\bar{y})}{\mathrm{Var}(y)}
$$

where: 

$\mathrm{Var}(\bar{y})$: Variance of the average response across repeated trials of the same stimulus  

$\mathrm{Var}(y)$: Total variance across all trials

---



    
---














[^1]: [The Voxelwise Encoding Model framework: A tutorial introduction to fitting encoding models to fMRI data](https://doi.org/10.1162/imag_a_00575)
[^2]: [wav2vec 2.0: A Framework for Self-Supervised Learning of Speech Representations](https://doi.org/10.48550/arXiv.2006.11477)
