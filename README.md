# Application of a Global Navigation Satellite System based on Least square method

<strong>The Global Navigation Satellite System (GNSS) exploits the positional information from a satellite constellation (such as GPS, GLONASS, GALILEO, etc.) to determine the location of a receiver.</strong>  

## Geolocation principles with GNSS
The determination of the receiver's position relies on the coverage provided by the satellite constellation. To obtain accurate positioning information, it requires the availability of ephemeris data from at least 4 satellites. Nowadays, on average, a GPS receiver is covered by 7 satellites. Having a higher number of satellites in view generally leads to more accurate positioning information.  

### Error sources
In addition to the coverage constraints, the accuracy of location information provided by GNSS is affected by various technical and physical factors:

 - GPS receiver clock error $Δt_{Rx}$ : Due to the inaccuracy of the internal GPS clocks compared to the atomic clocks in satellites, there can be clock errors that introduce inaccuracies in the measurements.

 - Satellite clock error $Δt_{SV}$ : The size of the message containing navigation data, its processing, and relativistic effects can lead to slight clock errors (~1ms) even if the internal clock is stable. This error is broadcasted with the signal and can introduce an error of up to 6 meters.

 - Satellite orbital range error $Δd_{SV}$ : This error combines cross-track and along-track errors. 
    - Cross-track error represents the radial displacement from Earth
    - Along-track error represents the offset of the satellite's position with respect to its trajectory axis. 
   These errors are due to the sensitivity of IMU sensors in satellites and the time it takes to update the position data from control stations. Before correction, this error can reach up to 20 meters.

 - Ionospheric range error $Δd_{ion}$ : The density of electrons in Earth's ionospheric layers causes signal delays due to changes in the signal's wavelength (Ionospheric Scintillation) caused by various refractive indices. This error is variable worldwide and is provided by the almanac data. Receivers process it with a tropospheric model, but it can still introduce significant errors ranging from 10 to 20 meters.

 - Tropospheric range error $Δd_{tro}$ : This error is caused by variations in refractive index due to atmospheric layers, particularly influenced by pressure and temperature. Tropospheric models included in GPS receivers are more accurate than ionospheric models. The error introduced is similar to the ionospheric range error, around 10 to 20 meters, but after correction, these errors are reduced to under a meter.

 - Multipath range error $Δd_{mp}$ : This error is induced by signal delays caused by reflections off surrounding surfaces before reaching the receiver. Control stations are strategically placed to minimize this error, but GPS receivers can be located anywhere and need correction. The multipath delay is around 1 to 2 meters and is reduced to approximately 0.5 meters after correction.

 - Receiver noise range error $Δd_{Rx}$ : This error affects both carrier-phase and pseudorange measurements, similar to the multipath phenomenon, and exhibits high variability. The magnitude of this error is linked to the receiver's performance and depends on the detected RMS bandwidth. Shorter bandwidths require more correction. This error contributes to an uncertainty of approximately 2 meters.

The pseudorange $PR_{i}$ from the satellite $i$ to the receiver is the combination of the real range $ρ_{i}$ *(that we want to know accurately)* with the enumerated errors:  

$PR_{i} = ρ_{i} + c.Δt_{Rx_{i}} + c.Δt_{SV_{i}} + Δd_{SV_{i}} + Δd_{ion_{i}} + Δd_{tro_{i}} + Δd_{mp_{i}} + Δd_{Rx_{i}}$  



### True range determination

**The true range $ρ$ between the satellites and the GPS receiver have to be determined. Here the position $(X_{i}, Y_{i}, Z_{i})$ of the satellite $i$ is 
known while the position of the receiver $(X_{Rx}, Y_{Rx}, Z_{Rx})$ has to be found by estimating each error.**

$ρ_{i} = \sqrt{ (X_{i}-X_{Rx})^{2} + (Y_{i}-Y_{Rx})^{2} + (Z_{i}-Z_{Rx})^{2}}$



