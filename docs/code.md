# All Codes Used in the Project

This page contains the full MATLAB code snippets used to generate each figure and analysis in the project. Each section corresponds to the specific concept or result discussed on its respective page.

---

## Effect of Logistic Shape Parameter (`α`)

---

```
clc;
clear;

u = 1;
C0 = 0.1;
Kc = 8;
tspan = [0, 20];
a = [0.4 1 5];
colors = ['r' 'b' 'g'];

figure
hold on

for k = 1:length(a)
    fprintf("%d\n", k)
    dc_dt = @(t,C) u * C * (1 - (C / Kc)^a(k));
    [t, C] = ode45(dc_dt, tspan, C0);
    plot(t, C, colors(k), 'LineWidth', 2);
end

ylabel('# of Cancer Cells')
xlabel('Time')
legend('a < 1', 'a = 1', 'a > 1')
title('Sharpness due to a')
```

---

## Behavior of the Von Bertalanffy model

---

```
clc, clear

a = 1.6e-7; %m^3 /day
b = 0.2e-7; %m^3 /day
k = a/b;
t = 0:0.1:20;
V0 = [k^3 - 500, k^3, k^3+500];
t0 = 0;
colors = ['r' 'b' 'g'];

figure
hold on

for l = 1:length(V0)

    V_t = @(t, V0) (k+(V0^(1/3)-k)*exp(-(1/3).*(t-t0))).^3;

    plot(t, V_t(t, V0(l)),colors(l), 'LineWidth', 2)
end

ylabel("V(t) (m^3)")
xlabel("time(days)")
legend('V0 < K^3', 'V0 = K^3', 'V0 > K^3')
title("Von Bertalanffy model")

```
