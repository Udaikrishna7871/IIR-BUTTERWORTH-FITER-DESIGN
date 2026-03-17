# EXP 3 : IIR-BUTTERWORTH-FITER-DESIGN

## AIM: 

 To design an IIR Butterworth filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```
clc;
close;

wp      = input('Enter the pass band frequency (Radians) = ');
ws      = input('Enter the stop band frequency (Radians) = ');
alphap  = input('Enter the pass band attenuation (dB) = ');
alphas  = input('Enter the stop band attenuation (dB) = ');
T       = input('Enter the Value of sampling Time = ');

omegap = (2/T) * tan(wp/2);
disp(omegap, 'omegap = ');

omegas = (2/T) * tan(ws/2);
disp(omegas, 'omegas = ');

N = log10(((10^(0.1*alphas)) - 1) / ((10^(0.1*alphap)) - 1)) / ...
    (2 * log10(omegas / omegap));
disp(N, 'N = ');

N = ceil(N);
disp(N, 'Round off value of N = ');

omegac = omegap / (((10^(0.1*alphap)) - 1)^(1/(2*N)));
disp(omegac, 'omegac = ');

disp('Normalised Analog LPF Transfer function H(S) = ');
hs_Normalised = analpf(N, 'butt', [0,0], 1);
disp(hs_Normalised);

disp('Analog LPF Transfer function H(S) = ');
hs = analpf(N, 'butt', [0,0], omegac);
disp(hs);

z  = poly(0, 'z');                 // Defining variable z
Hz = horner(hs, (2/T) * ((z-1)/(z+1)));  

disp('Digital LPF Transfer function H(Z) = ');
disp(Hz);

HW = frmag(Hz, 512);              
w  = 0:%pi/511:%pi;

plot(w/%pi, abs(HW));
xlabel('Normalized Digital Frequency (×π)');
ylabel('Magnitude');
title('Frequency Response of Butterworth IIR LPF');
```


## PROGRAM (HPF):
```
clc;
clear;
close;

wp = input('Enter the pass band frequency (Radians )= ');
ws = input('Enter the stop band frequency (Radians )= ');
alphap = input('Enter the pass band attenuation (dB)= ');
alphas = input('Enter the stop band attenuation (dB)= ');
T = input('Enter the Value of sampling Time=');

omegap = (2/T)*tan(wp/2);
disp(omegap, 'omegap=');
omegas = (2/T)*tan(ws/2);
disp(omegas, 'omegas=');

N = log10(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1))/(2*log10(omegap/omegas));
disp(N, 'N=');
N = ceil(N);
disp(N, 'Round off value of N=');

omegac = omegas/(((10^(0.1*alphas)) -1)^(1/(2* N)));
disp(omegac, 'omegac=');

disp('Normalized Analog LPF Transfer function H(s)=');
hs_Normalised = analpf(N,'butt',[0,0],1);
disp(hs_Normalised);

disp('Analog LPF Transfer function H(s)=');
hs = analpf(N,'butt',[0,0],omegac);
disp(hs);

s = poly(0,'s');
hs_hp = horner(hs, (omegac^2)/s);  

disp('Analog HPF Transfer function H(s)=');
disp(hs_hp);

z = poly(0,'z');
Hz = horner(hs_hp, (2/T)*((z - 1)/(z + 1)));

disp('Digital HPF Transfer function H(z)=');
disp(Hz);
HW = frmag(Hz,512);
w = 0:%pi/511:%pi;

plot(w/%pi, abs(HW));
xlabel('Normalized Digital Frequency (×π rad/sample)');
ylabel('Magnitude');
title('Frequency Response of Butterworth IIR High-pass Filter');
xgrid();

```


## OUTPUT (LPF) : 

<img width="1348" height="780" alt="image" src="https://github.com/user-attachments/assets/aed074b5-7eff-4418-87ef-3efc5a803e11" />

## OUTPUT (HPF) : 

<img width="1362" height="732" alt="image" src="https://github.com/user-attachments/assets/2b90808c-9dc3-41e5-beef-c0bb63bc72fa" />



## RESULT: 

The design of IIR Butterworth filter (LPF & HPF) using SCILAB was successfully executed
