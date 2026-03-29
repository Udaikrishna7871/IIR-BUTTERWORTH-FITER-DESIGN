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
# program
```
clc;
close;

wp1 = input('Enter wp1 = ');
wp2 = input('Enter wp2 = ');
ws1 = input('Enter ws1 = ');
ws2 = input('Enter ws2 = ');
alphap = input('Enter passband attenuation (dB) = ');
alphas = input('Enter stopband attenuation (dB) = ');
T = input('Enter sampling time = ');

// Pre-warping
Wp1 = (2/T)*tan(wp1/2);
Wp2 = (2/T)*tan(wp2/2);
Ws1 = (2/T)*tan(ws1/2);
Ws2 = (2/T)*tan(ws2/2);

B = Wp2 - Wp1;
W0 = sqrt(Wp1*Wp2);

// Equivalent LPF
Ws = min(abs((Ws1^2 - W0^2)/(B*Ws1)), abs((Ws2^2 - W0^2)/(B*Ws2)));
Wp = 1;

// Order
N = log10(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1)) / (2*log10(Ws/Wp));
N = ceil(N);

// Analog LPF
hs = analpf(N, 'butt', [0,0], 1);

// LPF → BPF transformation
s = poly(0,'s');
hs_bp = horner(hs, (s^2 + W0^2)/(B*s));

// Bilinear
z = poly(0,'z');
Hz = horner(hs_bp, (2/T)*((z-1)/(z+1)));

// Plot
HW = frmag(Hz,512);
w = 0:%pi/511:%pi;

plot(w/%pi, abs(HW));
title('Butterworth BPF');
```
# program
```
clc;
close;

wp1 = input('Enter wp1 = ');
wp2 = input('Enter wp2 = ');
ws1 = input('Enter ws1 = ');
ws2 = input('Enter ws2 = ');
alphap = input('Enter passband attenuation (dB) = ');
alphas = input('Enter stopband attenuation (dB) = ');
T = input('Enter sampling time = ');

// Pre-warping
Wp1 = (2/T)*tan(wp1/2);
Wp2 = (2/T)*tan(wp2/2);
Ws1 = (2/T)*tan(ws1/2);
Ws2 = (2/T)*tan(ws2/2);

B = Wp2 - Wp1;
W0 = sqrt(Wp1*Wp2);

// Equivalent LPF
Ws = min(abs((B*Ws1)/(Ws1^2 - W0^2)), abs((B*Ws2)/(Ws2^2 - W0^2)));
Wp = 1;

// Order
N = log10(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1)) / (2*log10(Ws/Wp));
N = ceil(N);

// Analog LPF
hs = analpf(N, 'butt', [0,0], 1);

// LPF → BSF transformation
s = poly(0,'s');
hs_bs = horner(hs, (B*s)/(s^2 + W0^2));

// Bilinear
z = poly(0,'z');
Hz = horner(hs_bs, (2/T)*((z-1)/(z+1)));

// Plot
HW = frmag(Hz,512);
w = 0:%pi/511:%pi;

plot(w/%pi, abs(HW));
title('Butterworth Band Stop Filter');
```

## OUTPUT (LPF) : 

<img width="1348" height="780" alt="image" src="https://github.com/user-attachments/assets/aed074b5-7eff-4418-87ef-3efc5a803e11" />

## OUTPUT (HPF) : 

<img width="1362" height="732" alt="image" src="https://github.com/user-attachments/assets/2b90808c-9dc3-41e5-beef-c0bb63bc72fa" />

## OUTPUT (BPF) : 
<img width="701" height="736" alt="image" src="https://github.com/user-attachments/assets/a434afb2-f56e-44b6-bc7c-fd8f8c7f3293" />

## OUTPUT (BSF) : 
<img width="760" height="713" alt="image" src="https://github.com/user-attachments/assets/0c39bf3c-95fc-425b-bc08-d759a2173992" />


## RESULT: 

The design of IIR Butterworth filter (LPF & HPF) using SCILAB was successfully executed
