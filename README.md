	%line encoding 
clearvars;
clc;
5.	L = 64; %number of digital samples per data bit
6.	Fs = 10*L; %Sampling frequency
7.	voltageLevel = 5;
8.	data = rand(1000,1)>0.5;
9.	clk = mod(0:2*numel(data)-1,2).';
10.	ami = 1*data;
11.	previousBit = 0; %AMI encoding
12.	for idx=1:numel(data)
13.	if (ami(idx)==1) && (previousBit==0)
14.	ami(idx)= voltageLevel;
15.	previousBit=1;
16.	end
17.	if (ami(idx)==1) && (previousBit==1)
18.	ami(idx)= -voltageLevel;
19.	previousBit = 0;
20.	end
21.	end
22.	clk_sequence=reshape(repmat(clk,1,L).',1,length(clk)*L);
23.	data_sequence=reshape(repmat(data,1,2*L).',1,length(data)*2*L);
24.	unipolar_nrz_l = voltageLevel*data_sequence;
25.	nrz_encoded = voltageLevel*(2*data_sequence - 1);
26.	unipolar_rz = voltageLevel*and(data_sequence,not(clk_sequence));
27.	ami_sequence = reshape(repmat(ami,1,2*L).',1,length(ami)*2*L);
28.	manchester_encoded = voltageLevel*(2*xor(data_sequence,clk_sequence)-1);
29.	figure(1);
30.	subplot(4,1,1);
31.	plot(clk_sequence(1:800),'LineWidth',3);
32.	title('Clock');
33.	grid on
34.	subplot(4,1,2);
35.	plot(data_sequence(1:800),'LineWidth',3);
36.	title('Data');
37.	grid on
38.	subplot(4,1,3);
39.	plot(unipolar_nrz_l(1:800),'LineWidth',3);
40.	title('Unipolar non-return-to-zero level');
41.	grid on;
42.	subplot(4,1,4);
43.	plot(nrz_encoded(1:800),'LineWidth',3);
44.	title('Bipolar Non-return-to-zero level');
45.	grid on;
46.	figure(2);
47.	subplot(4,1,1);
48.	plot(clk_sequence(1:800),'LineWidth',3);
49.	title('Clock');
50.	grid on;
51.	subplot(4,1,2);
52.	plot(data_sequence(1:800),'LineWidth',3);
53.	title('Data');
54.	grid on;
55.	subplot(4,1,3);
56.	plot(ami_sequence(1:800),'LineWidth',3);
57.	title('Alternate Mark Inversion (AMI)');
58.	grid on;
59.	subplot(4,1,4);
60.	plot(manchester_encoded(1:800),'LineWidth',3);
61.	title('Manchester Scheme');
62.	grid on;
63.	Rb=1;
64.	Tb=1/Rb;
65.	f=0:0.025*Rb:2*Rb;
66.	x=f*Tb;
67.	P1 = (Tb)*(sinc(x).*sinc(x));
68.	figure(1);
69.	plot(f,P1,'r');
70.	grid on;
71.	box on;
72.	xlabel('frequency as a multiple of Bitrate(fRb)---->');
73.	ylabel('Power Spectral Density ---->');
74.	title('PSD for Polar Signal');
75.	hold on;
76.	P2 = (Tb)*(sinc(x/2)).*(sinc(x/2)).*(sin(pi*x)).*(sin(pi*x));
77.	plot(f,P2,'m');
78.	grid on;
79.	box on;
80.	xlabel('frequency as a multiple of Bitrate(fRb)---->');
81.	ylabel('Power Spectral Density ---->');
82.	title('PSD for Polar Signal');
83.	hold on;
84.	P3 = (Tb/4)*(sinc(x).*sinc(x)) + (1/4)*dirac(f);
85.	plot(f,P3,'g');
86.	grid on;
87.	box on;
88.	xlabel('frequency as a multiple of Bitrate(fRb)---->');
89.	ylabel('Power Spectral Density ---->');
90.	title('PSD for Polar Signal');
91.	hold on;
92.	P4 = Tb*(sinc(x/2)).*(sinc(x/2)).*(sin(pi*x/2)).*(sin(pi*x/2));
93.	plot(f,P4,'b');
94.	grid on;
95.	box on;
96.	xlabel('frequency as a multiple of Bitrate(fRb)---->');
97.	ylabel('Normalised Power Spectral Density ---->');
98.	title('PSD for Different Line Coding Schemes');
99.	hold on;
100.	legend('Polar','Bipolar','Unipolar','Manchester');
