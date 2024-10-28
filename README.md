import numpy as np

# Parameter
L = 0.5  # Induktansi Henry
C = 10e-6  # Kapasitansi Farad

def f(R):
    return (1 / (2 * np.pi)) * np.sqrt(1 / (L * C) - (R**2 / (4 * L**2)))

def f_prime(R):
    # Turunan dari f(R) menggunakan aturan rantai
    return (-R / (4 * np.pi * (L * C)**(1.5))) * (1 / np.sqrt(1 / (L * C) - (R**2 / (4 * L**2))))

def bisection_method(f, target, a, b, tol):
    if f(a) - target > 0 and f(b) - target > 0:
        raise ValueError("f(a) dan f(b).")
    
    while (b - a) / 2.0 > tol:
        midpoint = (a + b) / 2.0
        if f(midpoint) - target == 0:
            return midpoint
        elif (f(a) - target) * (f(midpoint) - target) < 0:
            b = midpoint
        else:
            a = midpoint
            
    return (a + b) / 2.0

# Parameter
target_frequency = 1000  # Hz
tolerance = 0.1  # Ω
a, b = 0, 100  # interval awal

R_bisection = bisection_method(f, target_frequency, a, b, tolerance)
print( {R_bisection:.4f} Ω")

def newton_raphson(f, f_prime, target, R0, tol):
    R = R0
    while True:
        R_new = R - (f(R) - target) / f_prime(R)
        if abs(R_new - R) < tol:
            return R_new
        R = R_new

# Parameter
R_newton = newton_raphson(f, f_prime, target_frequency, 50, tolerance)
print(f"Nilai R menggunakan metode Newton-Raphson: {R_newton:.4f} Ω")

import matplotlib.pyplot as plt

# Data untuk visualisasi
iterations_bisect = []
iterations_newton = []
error_bisect = []
error_newton = []

# Metode Biseksi
a, b = 0, 100
while (b - a) / 2.0 > tolerance:
    midpoint = (a + b) / 2.0
    iterations_bisect.append(midpoint)
    if f(midpoint) - target_frequency == 0:
        break
    elif (f(a) - target_frequency) * (f(midpoint) - target_frequency) < 0:
        b = midpoint
    else:
        a = midpoint
    error_bisect.append(abs(f(midpoint) - target_frequency))

# Metode Newton-Raphson
R = 50
while True:
    R_new = R - (f(R) - target_frequency) / f_prime(R)
    iterations_newton.append(R_new)
    error_newton.append(abs(f(R_new) - target_frequency))
    if abs(R_new - R) < tolerance:
        break
    R = R_new

# Visualisasi
plt.figure(figsize=(12, 6))

# Plot Biseksi
plt.subplot(1, 2, 1)
plt.plot(error_bisect, label='Biseksi', marker='o')
plt.title('Error Metode Biseksi')
plt.xlabel('Iterasi')
plt.ylabel('Error')
plt.yscale('log')
plt.legend()

# Plot Newton-Raphson
plt.subplot(1, 2, 2)
plt.plot(error_newton, label='Newton-Raphson', marker='o')
plt.title('Error Metode Newton-Raphson')
plt.xlabel('Iterasi')
plt.ylabel('Error')
plt.yscale('log')
plt.legend()

plt.tight_layout()
plt.show()

Nilai 
R menggunakan metode biseksi dan Newton-Raphson akan ditampilkan di konsol.
Grafik akan menunjukkan konvergensi error untuk masing-masing metode.

