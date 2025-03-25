# AAE6102-Lab-Group2

**Group Number**: 2  
**Group Members**: CHEN Zhixing, LI Bingxian, LI Peisen, LUO Wenting, WANG Junhui, WANG Yang.

---

## 1. Lab Overview

Use open source positioning software (`rtklib demo5-b34k` in this lab) to analyze positioning performance under different positioning modes and different parameter settings, including:

- Positioning accuracy  
- Processing time  
- Robustness to challenging conditions.  

---

## 2. Experimental Data Analysis

The experimental data include:

- Base station HKSC  
- Dynamic sampling data of the rover station in the urban environment  

The figure below shows data from the base station, which includes four systems: GPS, GLONASS, Galileo, and Beidou. Most of the data are dual-frequency and triple-frequency.

![image](https://github.com/user-attachments/assets/0395c48c-6ce4-40cf-af08-455ee124f036) 
*Base station observation information*

The following presents data from the rover station, which reveals the presence of regional systems in addition to the four major GNSS systems. The data for these major GNSS systems are predominantly dual-frequency and single-frequency, with significant interruptions and cycle slips. Furthermore, the signal-to-noise ratio (SNR) is mostly between 30-40 dBHz, with some satellites and instances falling below 30 dBHz.

![image](https://github.com/user-attachments/assets/a38ef691-40ed-4954-a518-8b926c11e19b)
*Rover station observation information*

![image](https://github.com/user-attachments/assets/e0731300-d1a9-4873-8123-5372525922ce)
*SNR and elevation angle information of Rover station observations*

---

## 3. Single GPS and Multi-GNSS Positioning

| Metric                | Single GPS SPP       | Multi-GNSS SPP       |
|-----------------------|----------------------|----------------------|
| Positioning Accuracy (RMS)  | N: 20.12 m           | N: 10.71 m           |
|                       | E: 17.76 m           | E: 10.43 m           |
|                       | U: 65.80 m           | U: 40.75 m           |
| Processing Time       | Short                | Medium               |
| Robustness under Challenging Conditions  | Poor                 | Medium               |

### (1) Positioning Accuracy

By comparing the root mean square error (RMS) results of single GPS SPP and Multi-GNSS SPP, it is evident that the Multi-GNSS system exhibits superior positioning accuracy. In the error plot of single GPS SPP, the RMS values are 20.12 meters in the North direction, 17.76 meters in the East direction, and 65.80 meters in the Up direction, resulting in a relatively large error range. Conversely, in the error plot for Multi-GNSS SPP, the RMS values for the North and East directions are 10.71 meters and 10.43 meters, respectively, significantly reducing positioning errors. The geometric shape of the polygons in the visualization results also reflects that the data from single GPS SPP tends to be more scattered, whereas the data from Multi-GNSS SPP exhibits a more compact concentration trend, indicating that the Multi-GNSS system can capture actual position changes more accurately.

### (2) Processing Time

In terms of processing time, the processing time of Multi-GNSS is longer than that of single GPS, because Multi-GNSS has more satellites and requires more time to calculate satellite positions, clock errors, error processing, and construct design matrices and error matrices.

### (3) Robustness under Challenging Conditions

Multi-GNSS systems demonstrate superior performance under challenging conditions, primarily due to their ability to utilize signals from more satellites, thereby enhancing signal reliability and stability. The visualization results indicate that single GPS SPP may show a reduced concentration of data points due to signal occlusion or reflection, resulting in irregular trajectory shapes. In contrast, Multi-GNSS SPP generates smoother and more regular trajectories from signals captured by multiple navigation satellites, maintaining relatively consistent positioning performance even in environments with poor signal conditions. The geometric shape differences further highlight the advantages of the Multi-GNSS system in complex environments, showcasing its potential benefits in practical applications.

![image](https://github.com/user-attachments/assets/571dee72-dc44-433b-abf3-d12f56444deb)  
*Positioning result under Single GPS SPP mode*

![image](https://github.com/user-attachments/assets/a44b6596-9c44-426b-995f-ab964aa59f2c)  
*Single GPS SPP Results*

![image](https://github.com/user-attachments/assets/89ee5f8a-5a2a-46a8-9931-d91bca7c8893) 
*Positioning result under Multi-GNSS SPP mode*

![image](https://github.com/user-attachments/assets/7db21ebe-b5a3-4a65-9dec-bc5955fa60e1)  
*Multi-GNSS SPP Results*

---

## 4. Different Positioning Modes

In order to discover the impact of different positioning modes, the other parameters were set to the same experienced values. The key parameter settings are shown in the table below.

### Key Parameter Settings

| Positioning Mode | Filter Type | Elevation Mask | SNR Mask |
|------------------|-------------|----------------|----------|
| Single           | Forward     | 15°            | 0        |
| DGNSS            | Forward     | 15°            | 0        |
| Static           | Forward     | 15°            | 0        |
| Kinematic        | Forward     | 15°            | 0        |

### Positioning Results

| Positioning Mode | Positioning Accuracy (RMS)       | Processing Time | Robustness to Challenging Conditions |
|------------------|----------------------------------|-----------------|---------------------------------------|
| Single           | Low (especially large error in U direction)    | Short           | Poor                                  |
| DGNSS            | High (small errors in N and E)                 | Medium          | Good                                  |
| Static           | Very low (large errors in N and E directions)  | Long            | Poor                                  |
| Kinematic        | High (best in N and E directions)              | Long            | Good                                  |

### (1) Positioning Accuracy

The positioning accuracy of the Single mode is relatively low, particularly with a large error in the U direction (RMS = 40.75 m). This may be due to its reliance on a single GNSS signal, making it susceptible to urban environmental factors. The DGNSS mode significantly improves positioning accuracy, especially in the N (RMS = 6.13 m) and E (RMS = 8.19 m) directions, indicating that differential correction effectively reduces errors. In this result, the Static mode exhibits very large errors, particularly in the N (RMS = 284.16 m) and E (RMS = 214.73 m) directions. This could be because Static mode typically requires a stable environment and good signal conditions, and urban data may suffer from signal obstructions, multipath effects, or other interferences, leading to decreased positioning accuracy. The Kinematic mode demonstrates the best accuracy in the N (RMS = 5.42 m) and E (RMS = 7.48 m) directions, with U (RMS = 27.68 m) errors comparable to those of the DGNSS mode. It is suitable for high-recision positioning in dynamic environments, as it can quickly process data and update positions, making it ideal for tracking moving targets.

### (2) Processing Time

SPP has a shorter processing time because it relies only on pseudoranges for positioning. In DGNSS mode, there are issues such as selecting reference stars, so the processing time is longer than SPP. Static and Kinematic modes use not only pseudoranges but also carrier, so the processing time is longer. Static mode is usually used for long-term observations to improve accuracy, but it does not perform well in this result. Kinematic mode is designed for real-time positioning in dynamic environments and has a shorter processing time, so it is suitable for applications that require fast position updates.

### (3) Robustness under Challenging Conditions

In the Single mode, under challenging conditions such as signal obstructions or multipath effects in urban data, there may be significant impacts, leading to large fluctuations in the U direction RMS values. The DGNSS mode, by using differential correction, can better resist some challenging conditions, demonstrating good robustness, with RMS values more stable compared to the Single mode. The Static mode is more suited for high-precision positioning under stable conditions, but in complex urban environments with unstable signals, RMS values exhibit significant variations, indicating poor performance and robustness. The Kinematic mode is designed for dynamic environments and generally performs well under moving conditions, with relatively stable RMS values, especially in the N and E directions, indicating good robustness.

![image](https://github.com/user-attachments/assets/b7f65667-b1d6-40bb-a686-3f17273c6a55)
*Positioning result under Single Positioning Mode*

![image](https://github.com/user-attachments/assets/d2c6b38e-fae6-4e82-886e-1061a32c493f)
*Multi-GNSS SPP NEU results*

![image](https://github.com/user-attachments/assets/b8ae9847-4932-49b6-9e77-e716713f6099)
*Positioning result under DGNSS Positioning Mode*

![image](https://github.com/user-attachments/assets/a07c40b5-fd54-4110-91b7-dc7f47ffc435)
*DGNSS NEU Results*

![image](https://github.com/user-attachments/assets/d138e688-9c9c-46bb-8b18-193b84b5938b)
*Positioning result under Static Positioning Mode*

![image](https://github.com/user-attachments/assets/99e47a35-dee1-4840-b875-00798035d617)
*Static NEU Results*

![image](https://github.com/user-attachments/assets/c8bdb192-8723-49bf-90ae-c1e08f380fca)
*Positioning result under Kinematic  Positioning Mode*

![image](https://github.com/user-attachments/assets/1cfce271-f005-47b7-b736-97c2689d3e6a)
*Kinematic NEU Results*

---

## 5. Different cut-off elevation angles and SNR

In DGNSS positioning mode, other default settings remain unchanged, and the results of exploring elevation angles of 5 and 15 are as follows:

### Comparison of Positioning Effects Under Different Cut-off Elevation Angles

| Elevation [°] | Accuracy       | Processing time | Robustness     |
|---------------|----------------|-----------------|----------------|
| 5             | Slightly lower | Likely slower   | Likely higher  |
| 15            | Slightly higher| Likely longer   | Likely lower   |

### (1) Positioning Accuracy

We analyze the positioning accuracy using RMS errors:

- **5° elevation**: N RMS = 6.16 m, E RMS = 8.19 m, U RMS = 27.02 m  
- **15° elevation**: N RMS = 6.13 m, E RMS = 8.19 m, U RMS = 26.95 m  

The RMS values for the N and E components are nearly identical between the two elevation angles, indicating similar horizontal positioning accuracy. The U component shows a slight improvement in accuracy at the 15° elevation angle (26.95 m vs. 27.02 m).

### (2) Processing Time

For processing speed: a higher cut-off elevation angle (15°) can lead to faster processing speeds as fewer satellites are used, reducing the computational load. However, this can also mean less redundancy in satellite signals, which might affect accuracy and robustness. On the contrary, the 5° elevation angle includes more satellites, potentially increasing processing time due to more data to process, but it can improve redundancy and robustness.

### (3) Robustness to Challenging Conditions

For robustness to challenging conditions: 5° elevation allows for more satellites to be included in the solution, which can be beneficial in challenging conditions (e.g., urban canyons) where satellite signals are obstructed. More satellites can help maintain a solution when some signals are lost. While potentially less robust in challenging conditions due to fewer satellites being used, the higher cut-off angle (15°) can reduce the impact of multipath errors and atmospheric disturbances, which are more prevalent at lower elevation angles.

So, for accuracy, both elevation angles provide similar horizontal accuracy, with a slight improvement in vertical accuracy at 15°. For processing speed, 15° is likely to be faster due to fewer satellites being processed. For robustness, 5° may offer better robustness in challenging environments due to the inclusion of more satellites, while the 15° angle might provide cleaner signals with reduced multipath effects.

In conclusion, the choice between a 5° and 15° elevation angle involves a trade-off between robustness and processing speed, with accuracy remaining relatively consistent.

![image](https://github.com/user-attachments/assets/53309603-4081-4957-bd90-b088acf64cab)
*Positioning result under DGNSS with elevation mask of 5°*

![image](https://github.com/user-attachments/assets/3ed744c0-64b1-49c6-b4d4-17881add90a4) 
*NEU results under DGNSS with elevation mask of 5°*

![image](https://github.com/user-attachments/assets/9cccf45f-df11-4e65-8cca-2dfb882dc74a)
*Positioning result under DGNSS with elevation mask of 15°*

![image](https://github.com/user-attachments/assets/78761055-69e0-406d-adb2-927198242a0f) 
*NEU results under DGNSS with elevation mask of 15°*

In DGNSS positioning mode, other default settings remain unchanged, and the results of exploring SNR mask of 0 and 30 dBHZ are as follows:

### Comparison of Positioning Effects Under Different SNR Masks

| SNR masks [dBHZ] | Accuracy | Processing time | Robustness |
|------------------|----------|-----------------|------------|
| 0                | Lower    | Faster          | Lower      |
| 30               | Higher   | Slower          | Higher     |

### (1) Positioning Accuracy

We analyze the positioning accuracy using root mean square (RMS) errors:

- **30 dBHZ SNR Mask**: N RMS: 6.07 m, E RMS: 7.37 m, U RMS: 16.59 m  
- **0 dBHZ SNR Mask**: N RMS: 6.13 m, E RMS: 8.19 m, U RMS: 26.95 m  

In general, the positioning accuracy is relatively better in the horizontal components (N and E) compared to the vertical component (U). The RMS values indicate that the errors are smaller in the horizontal plane. The accuracy of 0 dBHZ is slightly worse in the horizontal components compared to the 30 dBHZ, and significantly worse in the vertical component.

### (2) Processing Time

For processing speed, a higher SNR mask (30 dBHZ) might involve more stringent filtering of signals, potentially requiring more processing time. Conversely, a lower SNR mask (0 dBHZ) might allow more signals to be processed quickly but with reduced accuracy.

### (3) Robustness to Challenging Conditions

For robustness to challenging conditions, the higher SNR mask likely filters out weaker signals, which can improve robustness in challenging conditions by reducing the impact of noise and interference. This is reflected in the relatively stable RMS values, especially in the horizontal components. On the contrary, the lower SNR mask allows more signals to be used, including weaker ones, which can be beneficial in environments with poor signal reception. However, this comes at the cost of increased noise and reduced accuracy, particularly in the vertical component.

So, the 30dBHZ mask provides better positioning accuracy, especially in the vertical component, compared to the 0dBHZ mask. The 30dBHZ mask is likely more robust to challenging conditions due to its ability to filter out weaker signals, reducing noise and interference. The 0dBHZ mask may offer faster processing due to less stringent filtering, but at the cost of reduced accuracy and robustness.

In conclusion, the choice between these SNR masks involves a trade-off between accuracy and robustness versus processing speed and signal availability. The 30 dBHZ mask is preferable for applications requiring higher accuracy and robustness, while the 0 dBHZ mask might be suitable for scenarios where signal availability is a priority.

![image](https://github.com/user-attachments/assets/d6facb6b-dfdb-4a21-9789-1d2057ee6f42)  
*Positioning result under DGNSS with SNR mask of 0*

![image](https://github.com/user-attachments/assets/9c55f924-3abb-4a3e-be9b-ed7c66c18606)  
*NEU results under DGNSS with SNR mask of 0*

![image](https://github.com/user-attachments/assets/6018eadb-cd99-450d-8a97-a7d7f69a106a)  
*Positioning result under DGNSS with SNR mask of 30 dBHZ*

![image](https://github.com/user-attachments/assets/e5fd95e2-2c7d-499a-a5c6-7bee6e65bac8)  
*NEU results under DGNSS with SNR mask of 30 dBHZ*

---

## 6. Different Filtering Methods

DGNSS positioning mode, with other default settings unchanged, explores the difference between forward filtering and forward-backward combined filtering:

### Comparison of Positioning Effects Under Different Filtering Methods

| Filtering | Accuracy | Processing time | Robustness |
|-----------|----------|-----------------|------------|
| Forward   | Medium   | Medium          | Good       |
| Combined  | Medium   | Longer          | Good       |

In the forward filtering results, the RMS values (N: 6.13 m, E: 8.19 m, U: 26.95 m) indicate moderate positioning accuracy, with the vertical component showing the largest errors. Because the algorithm processes measurements in a single forward pass, it typically offers shorter processing times compared to more complex filtering methods. However, without the benefit of backward smoothing, it can be less robust to data gaps or poor satellite geometry, as errors can accumulate if the filter encounters challenging signal conditions.

The combined filtering solution shows slightly improved positioning accuracy (N: 5.71 m, E: 7.35 m, U: 25.97 m), reflecting the advantage of using both forward and backward passes. This improvement usually comes at the cost of increased processing time, since the filter must run through the data in both time directions. Nevertheless, the additional computational effort enhances robustness, helping the system to better handle intermittent signal blockages or low-quality data by leveraging future and past measurements to refine the final position estimates.

![image](https://github.com/user-attachments/assets/4c8e5116-83fa-46b3-b33f-d7d7355af3ec)  
*Positioning result under DGNSS with forward filtering*

![image](https://github.com/user-attachments/assets/6a5e2052-72af-4c3a-b57e-cc898248461b)  
*NEU results under DGNSS with forward filtering*

![image](https://github.com/user-attachments/assets/3108ea9c-2a4c-436e-bef1-3146a82c01d9)  
*Positioning result under DGNSS with combined filtering*

![image](https://github.com/user-attachments/assets/dacd293e-c279-48b9-981e-55c075398494)
*NEU results under DGNSS with combined filtering*
