# Optical Failure Dataset

The following dataset was created using the ARNO testbed within the InRete Lab at the TeCIP Institute, Scuola Superiore Sant'Anna.
Using the system presented in A. Sgambelluri et al., ["Reliable and scalable Kafka-based framework for optical network telemetry"](https://www.osapublishing.org/jocn/abstract.cfm?uri=jocn-13-10-E42), we continously collected metrics from the optical testbed in normal condition in which we emulated periodic failures.

## Testbed description 
The testbed is composed of two Ericcson SPO 1400 devices (named *SPO1* and *SPO2*), each equipped with a 100Gb/s Optical Transport Network (OTN) muxponder (installed at the slot 18) with a DWDM optical line (port 11) and 10 tributary ports. Each muxponder is able to collect coherent metrics (i.e., BER and OSNR).
The output of the first SPO (*SPO1*) has been attached to a WSS, which is then attached to multi-span link over a 10dB attenuator. 
The multispan is actually composed of 3 spans, each with a length of 80km (240km total length), which have 4 EDFA amplifiers in the middle. 
Those amplifiers are controlled by the SPO devices: more specifically, the first and second amplifier (*Ampli1* and *Ampli2*) are controlled by the *SPO1*, while the other two (*Ampli3* and *Ampli4*) by the *SPO2*. 
Each amplifier has been configured in constant gain mode, that allows to enter each span with 0dBm of optical power.
The reverse link, from *SPO2* to *SPO1* is in back-to-back configuration, presenting a 10dB attenuator.
![Testbed](testbed.jpg)

## Dataset details
The dataset is composed of more than 10.000 samples, which are gathered from both the SPO devices and the amplifiers, every 3,5 seconds.

For each SPO, the following metrics are retrieved:
- Optical Signal to Noise Ratio (OSNR)
- Bit Error Rate (BER)

For each amplifier instead, we retrieved:
- Input Power 
- Ouput Power

For this reason, the csv has been structrured with the following fields:
- **Timestamp**: Unix format.
- **Type**: which has two possible values according to which device the metric is referring to:
  - *Infrastructure*, for the amplifiers.
  - *Devices*, for the SPO devices.
- **ID**: which refers to the id used in the testbed picture:
  - *SPO1*;
  - *SPO2*;
  - *Ampli1*;
  - *Ampli2*;
  - *Ampli3*;
  - *Ampli4*.
- **BER**: only for the SPOs related entries.
- **OSNR**: only for the SPOs related entries.
- **InputPower**: only for the amplifiers related entries.
- **OutputPower**: only for the amplifiers related entries.
- **Failure**: field showing entries acquired while simulating the failure.

This 10 hours dataset is divided in two halves:
- 5 hours in normal network condition;
- 5 hours with periodic failures.

The entry with timestamp *1624471835* delimits the two parts. 

More precisely, for the second half we used the WSS to dastrically increase the attenuation every 4 minutes, and so putting the network in a failure condition for 1 minute. After that, the WSS is reconfigured so that the network starts working properly again. 

The *Failure* field is used as an indicator that shows all the entries that are gathered while the input power of the *Ampli1* is too low due to the WSS configuation. 
