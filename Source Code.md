# 三点三次欸米尔特插值
```Matlab
clear;close all;
n = 4;
X = 1:4/n:5;
x0=2;x1=3;x2=4;
Y = log(X);
Y_d = X.^-1;
X2show = 1:0.05:5;
Y_ori = log(X2show);
for i = 1:length(X2show) Y2show(i) = Three_Her(X2show(i),x0,x1,x2,Y,Y_d);
end
plot(X2show,Y2show);hold on; plot(X2show,Y_ori); legend('三点三次埃尔米特插值','原函数')
```
```Matlab
function [p] = Three_Her(x,x0,x1,x2,Y,Y_d) 
f0 = Y(x0);
f1 = Y(x1);
f2 = Y(x2); 
f0_1 = (f1 - f0)/(x1-x0);
f1_2=(f2-f1)/(x2-x1); 
f0_2 = (f1_2-f0_1)/(x2-x0); 
A = (Y_d(x1)-f0_1-(x1-x0)*f0_2)/((x1-x0)*(x1-x2));
p = Y(x0)+f0_1*(x-x0)+f0_2*(x-x0)*(x-x1)+A*(x-x0)*(x-x1)*(x-x2); 
end
```
# 分段线性插值
```Matlab
clear;close all;
n = 4;
X = 1:4/n:5;
Y = log(X);
Y_d = X.^-1;
X2show = 1:0.05:5;
Y_ori = log(X2show); 
K>> for i = 1:length(X2show) k=0;
for j=1:length(X)-1 
if X2show(i) >= X(j) && X2show(i) <= X(j+1) k=j; 
end
end
Y2show(i) = Line(X2show(i),X(k:k+1),Y(k:k+1)); end plot(X2show,Y2show);
hold on;
plot(X2show,Y_ori);
legend('分段线性插值','原函数')
```
```Matlab
function [p] = Line(x, X, Y) 
a0 = (x-X(2))/(X(1)-X(2));
a1 = (x-X(1))/(X(2)-X(1)); 
p = a0*Y(1) + a1*Y(2); 
end
```
# 分段两点三次欸米尔特插值
```Matlab
clear;close all; 
n = 4;
X = 1:4/n:5; 
Y = log(X); 
Y_d = X.^-1; 
X2show = 1:0.05:5; 
Y_ori = log(X2show);
for i = 1:length(X2show) k = 0; 
for j=1:length(X)-1 
if X2show(i) >= X(j) && X2show(i) <= X(j + 1) k=j; end 
end 
Y2show(i) = Hermite(X2show(i),X(k:k+1),Y(k:k+1),Y_d(k:k+1));
end plot(X2show,Y2show,'LineWidth',4);
hold on; 
plot(X2show,Y_ori,'LineWidth',2); 
legend('分段Hermite插值','原函数')
```
```Matlab
function [estimation] = Hermite(x, X, Y, Y_d) alpha_0 = (1 + 2 * (x - X(1)) / (X(2) - X(1))) * ((x - X(2)) / (X(1) - X(2)))^2; 
alpha_1 = (1 + 2 * (x - X(2)) / (X(1) - X(2))) * ((x - X(1)) / (X(2) - X(1)))^2; 
beta_0 = (x - X(1)) * ((x - X(2)) / (X(1) - X(2)))^2; 
beta_1 = (x - X(2)) * ((x - X(1)) / (X(1) - X(2)))^2;
estimation = Y(1) * alpha_0 + Y(2) *alpha_1 + Y_d(1) * beta_0 + Y_d(2) * beta_1; 
end
```