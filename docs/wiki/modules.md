OpenMRF is a modular framework. All magnetization preparation and readout modules are implemented as Matlab structs and follow a similar structure. 
!!! tip "Module vs. instance"
    The Matlab structure defines the module. If your sequence features a specific module more than once, you do not need to initialize them individually. See code examples [below](#code-examples).

## Overview of module names
### Preparations
The source code for the following modules can be found in `include_pulseq_toolbox/src_preparations`. 

| Module Name | Long Name | Notes
|-|-|-|
| ADIASL | <u>A</u>diabatic <u>S</u>pin <u>L</u>ock | Defines an adiabatic spin lock module for T1\(\rho\) preparation |
| CEST | <u>C</u>hemical <u>E</u>xchange <u>S</u>aturation <u>T</u>ransfer ||
| CRUSH | <u>Crush</u>er ||
| FAT | <u>Fat</u> suppression | |
| INV | <u>Inv</u>ersion | |
| MLEV | <u>M</u>alcolm <u>Lev</u>itt | Defines a T2\(\rho\) preparation featuring Malcolm Levitt phase cycling. |
| SAT | <u>Sat</u>uration | |
| SL | <u>S</u>pin <u>L</u>ock | Defines a continuous wave spin lock module for T1\(\rho\) preparation. |
| T2 | <u>T2</u> preparation | |

### Readouts
The source code for the following modules can be found in `include_pulseq_toolbox/src_readouts`. 

| Module Name | Long Name | 
|-|-|
| EPI | <u>E</u>cho <u>P</u>lanar <u>I</u>maging |
| PRESS | <u>P</u>oint <u>Res</u>olved <u>S</u>pectroscopy |
| RAD | <u>Rad</u>ial ||
| SPI | <u>Spi</u>ral | 
| SPITSE | <u>Spi</u>ral <u>T</u>urbo <u>S</u>pin <u>E</u>cho |
| TSE | <u>T</u>urbo <u>S</u>pin <u>E</u>cho | 
| UTE | <u>U</u>ltra short <u>TE</u>|

### Other
The source code for the following modules can be found in `include_pulseq_toolbox/src_misc`.

| Module Name | Long Name | Notes
|-|-|-|
| FOV | <u>F</u>ield <u>o</u>f <u>v</u>iew | Defines the field of view geometry. 
| TRAJ | <u>Traj</u>ectory | |
| WASABI | <u>Was</u>erstein based <u>a</u>natomical <u>brain</u> <u>I</u>ndex | Used for simultaneous B0 and B1 mappin |
<!-- | MRF | <u>M</u>agnetic <u>R</u>esonance <u>F</u>ingerprinting | Defines MRF specific parameters, e.g., flip angles and repetition times. | -->

## The `..._init()` function
Before they can be added to a sequence, all magnetization preparations and readouts have to be initialized. However, the structure has to be created before since it's always the first input argument to the respective `..._init()` function. You don't have to define all parameters of the module in question - optional parameters have a default value that will be assigned by the `..._init()` function unless specified explicitly. 

The following two examples show how advanced magnetization preparations can be easily initialized in OpenMRF:
=== "Hyperbolic Secant Inversion"
    ```matlab
    INV.rf_type      = 'HYPSEC_inversion';
    INV.tExc         = 10 *1e-3;  % [s]  hypsec pulse duration
    INV.beta         = 700;       % [Hz] maximum rf peak amplitude
    INV.mu           = 4.9;       % [ ]  determines amplitude of frequency sweep
    INV.inv_rec_time = [15 75 150 250] *1e-3;
    INV = INV_init(INV, FOV, system);
    ```
=== "Adiabatic BIR4 T2 preparation"
    ```matlab
    T2.exc_mode   = 'adiabatic_BIR4';
    T2.rfc_dur    = 2 *1e-3;   % [s]  duration of composite refocusing pulses
    T2.bir4_tau   = 10 *1e-3;  % [s]  bir4 pulse duration
    T2.bir4_f1    = 640;       % [Hz] maximum rf peak amplitude
    T2.bir4_beta  = 10;        % [ ]  am waveform parameter
    T2.bir4_kappa = atan(10);  % [ ]  fm waveform parameter
    T2.bir4_dw0   = 30000;     % [rad/s] fm waveform scaling
    T2.prep_times = [40 80] * 1e-3;  % [s] inversion times
    T2            = T2_init(T2, FOV, system);
    ```

## The `..._add()` function 

## Code examples 