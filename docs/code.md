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

---

## Effect of varying \(d_3\) on tumor cells

---

```

a = 4.31e-1;
b = 2.17e-8;
c1 = 3.5e-6;
c2 = 1e-7;
d1 = 1e-6;
d2 = 4e-6;
d3 = [1e-4, 2e-4, 4e-4, 8e-4];
e = 4.12e-2;
f1 = 1e-8;
f2 = 0.01;
g = 2.4e-2;
g1 = 0;
gI = 0;
h = 3.42e-10;
h1 = 0;
i1 = 2e-2;
I = 1;
j1 = 1e-7;
k = 1e-7;
s1 = 1.3e4;
s2 = 500;
u = 0;
r1 = 0;
pI = 0;

tspan = [0, 100];
y0 = [100, 1, 1, 1];

figure
hold on
for z = 1:length(d3)


% define functions
dTdt = @(t, T, D, N, L) a*T*(1-b*T) - (c1*N - j1*D + k*L)*T;
dDdt = @(t, D, T, N, L) s2 - (f1*L + d2*N - d3(z)*T)*D - g*D;
dNdt = @(t, N, T, D, L) s1 - (g1*N*T^2)/(h1 + T^2) - (c2*T - d1*D)*N - e*N;
dLdt = @(t, D, T, L, N) f2*D*T - h*L*T - u*N*L^2 + r1*N*T + (pI*L*I)/(gI + I) - i1*L;



% solve differential equations
[t, Y] = ode45(@(t,y)[ ...
    dTdt(t, y(1), y(2), y(3), y(4)); ...
    dDdt(t, y(2), y(1), y(3), y(4)); ...
    dNdt(t, y(3), y(1), y(2), y(4)); ...
    dLdt(t,y(2), y(1), y(4), y(3))], tspan, y0);

% extract
T = Y(:, 1);
plot(t, T, LineWidth=1)
end

title("Tumor cells vs Time");
ylabel("Tumor Cells");
xlabel("Time(days)");
legend("d3 = 1e-4", "d3 = 2e-4", "d3 = 4e-4", "d3 = 8e-4");

```

---

## Effect of varying \(d_3\) on Natural Killer cells

---

```

a = 4.31e-1;
b = 2.17e-8;
c1 = 3.5e-6;
c2 = 1e-7;
d1 = 1e-6;
d2 = 4e-6;
d3 = [1e-4, 2e-4, 4e-4, 8e-4];
e = 4.12e-2;
f1 = 1e-8;
f2 = 0.01;
g = 2.4e-2;
g1 = 0;
gI = 0;
h = 3.42e-10;
h1 = 0;
i1 = 2e-2;
I = 1;
j1 = 1e-7;
k = 1e-7;
s1 = 1.3e4;
s2 = 500;
u = 0;
r1 = 0;
pI = 0;

tspan = [0, 100];
y0 = [100, 1, 1, 1];

figure
hold on
for z = 1:length(d3)


% define functions
dTdt = @(t, T, D, N, L) a*T*(1-b*T) - (c1*N - j1*D + k*L)*T;
dDdt = @(t, D, T, N, L) s2 - (f1*L + d2*N - d3(z)*T)*D - g*D;
dNdt = @(t, N, T, D, L) s1 - (g1*N*T^2)/(h1 + T^2) - (c2*T - d1*D)*N - e*N;
dLdt = @(t, D, T, L, N) f2*D*T - h*L*T - u*N*L^2 + r1*N*T + (pI*L*I)/(gI + I) - i1*L;



% solve differential equations
[t, Y] = ode45(@(t,y)[ ...
    dTdt(t, y(1), y(2), y(3), y(4)); ...
    dDdt(t, y(2), y(1), y(3), y(4)); ...
    dNdt(t, y(3), y(1), y(2), y(4)); ...
    dLdt(t,y(2), y(1), y(4), y(3))], tspan, y0);

% extract
T = Y(:, 3);
plot(t, T, LineWidth=1)
end

title("NK cells vs Time");
ylabel("Natural Killer Cells");
xlabel("Time(days)");
legend("d3 = 1e-4", "d3 = 2e-4", "d3 = 4e-4", "d3 = 8e-4");

```

---

## Effect of varying \(d_3\) on Dendritic cells

---

```

a = 4.31e-1;
b = 2.17e-8;
c1 = 3.5e-6;
c2 = 1e-7;
d1 = 1e-6;
d2 = 4e-6;
d3 = [1e-4, 2e-4, 4e-4, 8e-4];
e = 4.12e-2;
f1 = 1e-8;
f2 = 0.01;
g = 2.4e-2;
g1 = 0;
gI = 0;
h = 3.42e-10;
h1 = 0;
i1 = 2e-2;
I = 1;
j1 = 1e-7;
k = 1e-7;
s1 = 1.3e4;
s2 = 500;
u = 0;
r1 = 0;
pI = 0;

tspan = [0, 100];
y0 = [100, 1, 1, 1];

figure
hold on
for z = 1:length(d3)


% define functions
dTdt = @(t, T, D, N, L) a*T*(1-b*T) - (c1*N - j1*D + k*L)*T;
dDdt = @(t, D, T, N, L) s2 - (f1*L + d2*N - d3(z)*T)*D - g*D;
dNdt = @(t, N, T, D, L) s1 - (g1*N*T^2)/(h1 + T^2) - (c2*T - d1*D)*N - e*N;
dLdt = @(t, D, T, L, N) f2*D*T - h*L*T - u*N*L^2 + r1*N*T + (pI*L*I)/(gI + I) - i1*L;



% solve differential equations
[t, Y] = ode45(@(t,y)[ ...
    dTdt(t, y(1), y(2), y(3), y(4)); ...
    dDdt(t, y(2), y(1), y(3), y(4)); ...
    dNdt(t, y(3), y(1), y(2), y(4)); ...
    dLdt(t,y(2), y(1), y(4), y(3))], tspan, y0);

% extract
T = Y(:, 2);
plot(t, T, LineWidth=1)
end

title("Dendritic cells vs Time");
ylabel("Dendritic Cells");
xlabel("Time(days)");
legend("d3 = 1e-4", "d3 = 2e-4", "d3 = 4e-4", "d3 = 8e-4");

```

---

## Effect of varying \(d_3\) on CD8+ T cells

---

```

a = 4.31e-1;
b = 2.17e-8;
c1 = 3.5e-6;
c2 = 1e-7;
d1 = 1e-6;
d2 = 4e-6;
d3 = [1e-4, 2e-4, 4e-4, 8e-4];
e = 4.12e-2;
f1 = 1e-8;
f2 = 0.01;
g = 2.4e-2;
g1 = 0;
gI = 0;
h = 3.42e-10;
h1 = 0;
i1 = 2e-2;
I = 1;
j1 = 1e-7;
k = 1e-7;
s1 = 1.3e4;
s2 = 500;
u = 0;
r1 = 0;
pI = 0;

tspan = [0, 100];
y0 = [100, 1, 1, 1];

figure
hold on
for z = 1:length(d3)


% define functions
dTdt = @(t, T, D, N, L) a*T*(1-b*T) - (c1*N - j1*D + k*L)*T;
dDdt = @(t, D, T, N, L) s2 - (f1*L + d2*N - d3(z)*T)*D - g*D;
dNdt = @(t, N, T, D, L) s1 - (g1*N*T^2)/(h1 + T^2) - (c2*T - d1*D)*N - e*N;
dLdt = @(t, D, T, L, N) f2*D*T - h*L*T - u*N*L^2 + r1*N*T + (pI*L*I)/(gI + I) - i1*L;



% solve differential equations
[t, Y] = ode45(@(t,y)[ ...
    dTdt(t, y(1), y(2), y(3), y(4)); ...
    dDdt(t, y(2), y(1), y(3), y(4)); ...
    dNdt(t, y(3), y(1), y(2), y(4)); ...
    dLdt(t,y(2), y(1), y(4), y(3))], tspan, y0);

% extract
T = Y(:, 4);
plot(t, T, LineWidth=1)
end

title("CD8+ T cells vs Time");
ylabel("CD8+ T Cells");
xlabel("Time(days)");
legend("d3 = 1e-4", "d3 = 2e-4", "d3 = 4e-4", "d3 = 8e-4");

```

---

## Effect of varying \(s_2\) on tumor cells

---

```

a = 4.31e-1;
b = 2.17e-8;
c1 = 3.5e-6;
c2 = 1e-7;
d1 = 1e-6;
d2 = 4e-6;
d3 = 1e-4;
e = 4.12e-2;
f1 = 1e-8;
f2 = 0.01;
g = 2.4e-2;
g1 = 0;
gI = 0;
h = 3.42e-10;
h1 = 0;
i1 = 2e-2;
I = 1;
j1 = 1e-7;
k = 1e-7;
s1 = 1.3e4;
s2 = [500, 2000, 5000];
u = 0;
r1 = 0;
pI = 0;
colors= ['r' 'g' 'b'];

tspan = [0, 100];
y0 = [100, 1, 1, 1];

figure
hold on
for z = 1:length(s2)


% define functions
dTdt = @(t, T, D, N, L) a*T*(1-b*T) - (c1*N - j1*D + k*L)*T;
dDdt = @(t, D, T, N, L) s2(z) - (f1*L + d2*N - d3*T)*D - g*D;
dNdt = @(t, N, T, D, L) s1 - (g1*N*T^2)/(h1 + T^2) - (c2*T - d1*D)*N - e*N;
dLdt = @(t, D, T, L, N) f2*D*T - h*L*T - u*N*L^2 + r1*N*T + (pI*L*I)/(gI + I) - i1*L;



% solve differential equations
[t, Y] = ode45(@(t,y)[ ...
    dTdt(t, y(1), y(2), y(3), y(4)); ...
    dDdt(t, y(2), y(1), y(3), y(4)); ...
    dNdt(t, y(3), y(1), y(2), y(4)); ...
    dLdt(t,y(2), y(1), y(4), y(3))], tspan, y0);

% extract
T = Y(:, 1);

plot(t, T, LineWidth=2, Color=colors(z))
end

title("Tumor Cells vs Time");
ylabel("Tumor Cells");
xlabel("Time(days)");
legend("s2 = 500", "s2 = 2000", "s2 = 5000");
```

---

## Effect of varying \(s_2\) on Natural killer cells

---

```

a = 4.31e-1;
b = 2.17e-8;
c1 = 3.5e-6;
c2 = 1e-7;
d1 = 1e-6;
d2 = 4e-6;
d3 = 1e-4;
e = 4.12e-2;
f1 = 1e-8;
f2 = 0.01;
g = 2.4e-2;
g1 = 0;
gI = 0;
h = 3.42e-10;
h1 = 0;
i1 = 2e-2;
I = 1;
j1 = 1e-7;
k = 1e-7;
s1 = 1.3e4;
s2 = [500, 2000, 5000];
u = 0;
r1 = 0;
pI = 0;
colors= ['r' 'g' 'b'];

tspan = [0, 100];
y0 = [100, 1, 1, 1];

figure
hold on
for z = 1:length(s2)


% define functions
dTdt = @(t, T, D, N, L) a*T*(1-b*T) - (c1*N - j1*D + k*L)*T;
dDdt = @(t, D, T, N, L) s2(z) - (f1*L + d2*N - d3*T)*D - g*D;
dNdt = @(t, N, T, D, L) s1 - (g1*N*T^2)/(h1 + T^2) - (c2*T - d1*D)*N - e*N;
dLdt = @(t, D, T, L, N) f2*D*T - h*L*T - u*N*L^2 + r1*N*T + (pI*L*I)/(gI + I) - i1*L;



% solve differential equations
[t, Y] = ode45(@(t,y)[ ...
    dTdt(t, y(1), y(2), y(3), y(4)); ...
    dDdt(t, y(2), y(1), y(3), y(4)); ...
    dNdt(t, y(3), y(1), y(2), y(4)); ...
    dLdt(t,y(2), y(1), y(4), y(3))], tspan, y0);

% extract
T = Y(:, 3);

plot(t, T, LineWidth=2, Color=colors(z))
end

title("Natural Killer Cells vs Time");
ylabel("Natural Killer Cells");
xlabel("Time(days)");
legend("s2 = 500", "s2 = 2000", "s2 = 5000");
```

---

## Effect of varying \(s_2\) on Dendritic cells

---

```

a = 4.31e-1;
b = 2.17e-8;
c1 = 3.5e-6;
c2 = 1e-7;
d1 = 1e-6;
d2 = 4e-6;
d3 = 1e-4;
e = 4.12e-2;
f1 = 1e-8;
f2 = 0.01;
g = 2.4e-2;
g1 = 0;
gI = 0;
h = 3.42e-10;
h1 = 0;
i1 = 2e-2;
I = 1;
j1 = 1e-7;
k = 1e-7;
s1 = 1.3e4;
s2 = [500, 2000, 5000];
u = 0;
r1 = 0;
pI = 0;
colors= ['r' 'g' 'b'];

tspan = [0, 100];
y0 = [100, 1, 1, 1];

figure
hold on
for z = 1:length(s2)


% define functions
dTdt = @(t, T, D, N, L) a*T*(1-b*T) - (c1*N - j1*D + k*L)*T;
dDdt = @(t, D, T, N, L) s2(z) - (f1*L + d2*N - d3*T)*D - g*D;
dNdt = @(t, N, T, D, L) s1 - (g1*N*T^2)/(h1 + T^2) - (c2*T - d1*D)*N - e*N;
dLdt = @(t, D, T, L, N) f2*D*T - h*L*T - u*N*L^2 + r1*N*T + (pI*L*I)/(gI + I) - i1*L;



% solve differential equations
[t, Y] = ode45(@(t,y)[ ...
    dTdt(t, y(1), y(2), y(3), y(4)); ...
    dDdt(t, y(2), y(1), y(3), y(4)); ...
    dNdt(t, y(3), y(1), y(2), y(4)); ...
    dLdt(t,y(2), y(1), y(4), y(3))], tspan, y0);

% extract
T = Y(:, 2);

plot(t, T, LineWidth=2, Color=colors(z))
end

title("Dendritic Cells vs Time");
ylabel("Dendritic Cells");
xlabel("Time(days)");
legend("s2 = 500", "s2 = 2000", "s2 = 5000");
```

---

## Effect of varying \(s_2\) on Cytotoxic cells

---

```

a = 4.31e-1;
b = 2.17e-8;
c1 = 3.5e-6;
c2 = 1e-7;
d1 = 1e-6;
d2 = 4e-6;
d3 = 1e-4;
e = 4.12e-2;
f1 = 1e-8;
f2 = 0.01;
g = 2.4e-2;
g1 = 0;
gI = 0;
h = 3.42e-10;
h1 = 0;
i1 = 2e-2;
I = 1;
j1 = 1e-7;
k = 1e-7;
s1 = 1.3e4;
s2 = [500, 2000, 5000];
u = 0;
r1 = 0;
pI = 0;
colors= ['r' 'g' 'b'];

tspan = [0, 100];
y0 = [100, 1, 1, 1];

figure
hold on
for z = 1:length(s2)


% define functions
dTdt = @(t, T, D, N, L) a*T*(1-b*T) - (c1*N - j1*D + k*L)*T;
dDdt = @(t, D, T, N, L) s2(z) - (f1*L + d2*N - d3*T)*D - g*D;
dNdt = @(t, N, T, D, L) s1 - (g1*N*T^2)/(h1 + T^2) - (c2*T - d1*D)*N - e*N;
dLdt = @(t, D, T, L, N) f2*D*T - h*L*T - u*N*L^2 + r1*N*T + (pI*L*I)/(gI + I) - i1*L;



% solve differential equations
[t, Y] = ode45(@(t,y)[ ...
    dTdt(t, y(1), y(2), y(3), y(4)); ...
    dDdt(t, y(2), y(1), y(3), y(4)); ...
    dNdt(t, y(3), y(1), y(2), y(4)); ...
    dLdt(t,y(2), y(1), y(4), y(3))], tspan, y0);

% extract
T = Y(:, 4);

plot(t, T, LineWidth=2, Color=colors(z))
end

title("CD8+ T Cells vs Time");
ylabel("CD8+ T Cells");
xlabel("Time(days)");
legend("s2 = 500", "s2 = 2000", "s2 = 5000");
```

---

## Dynamics of tumor cells with of varying \(v_L\)

---

```

a = 4.31e-1;
b = 2.17e-8;
c1 = 3.5e-6;
c2 = 1e-7;
d1 = 1e-6;
d2 = 4e-6;
d3 = 1e-4;
e = 4.12e-2;
f1 = 1e-8;
f2 = 0.01;
g = 2.4e-2;
g1 = 0;
gI = 0;
h = 3.42e-10;
h1 = 0;
i1 = 2e-2;
I = 1;
j1 = 1e-7;
k = 1e-7;
s1 = 1.3e4;
s2 = 500;
u = 0;
r1 = 0;
pI = 0;
vL = [1 1e1 1e2 1e3 1e4 1e5 1e6];
colors = ['y' 'k' 'b' 'c' 'r' 'g' 'm'];

tspan = [0, 100];
y0 = [100, 1, 1, 1];

figure
hold on
for z = 1:length(vL)


% define functions
dTdt = @(t, T, D, N, L) a*T*(1-b*T) - (c1*N - j1*D + k*L)*T;
dDdt = @(t, D, T, N, L) s2 - (f1*L + d2*N - d3*T)*D - g*D;
dNdt = @(t, N, T, D, L) s1 - (g1*N*T^2)/(h1 + T^2) - (c2*T - d1*D)*N - e*N;
dLdt = @(t, D, T, L, N) f2*D*T - h*L*T - u*N*L^2 + r1*N*T + (pI*L*I)/(gI + I) - i1*L + vL(z);



% solve differential equations
[t, Y] = ode45(@(t,y)[ ...
    dTdt(t, y(1), y(2), y(3), y(4)); ...
    dDdt(t, y(2), y(1), y(3), y(4)); ...
    dNdt(t, y(3), y(1), y(2), y(4)); ...
    dLdt(t,y(2), y(1), y(4), y(3))], tspan, y0);

% extract
T = Y(:, 1);

plot(t, T, LineWidth=2, color=colors(z));
end

title("Tumor Cells vs Time");
ylabel("Tumor Cells");
xlabel("Time(days)");
legend("v_L = 1", "v_L = e1", "v_L = 1e2", "v_L = 1e3", "v_L = 1e4", "v_L = 1e5", "v_L = 1e6");

```

---

## Eff of varying \(d_3\) on tumor cells

---

```

```

---

## Eff of varying \(d_3\) on tumor cells

---

```

```

---
