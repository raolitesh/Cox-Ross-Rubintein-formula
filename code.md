```python
from scipy.stats import binom
import numpy as np
from simple_colors import *
```
## Exercise 1 (Different method applied on project 3(1))
```python
# Calculation of option price using 'Cox-Ross-Rubintein formula' for the same exercise in project 3(1)

print(yellow('Call option pricing by Cox-Ross-Rubintein formula', ['bold', 'reverse']))

s0 = 100 #initial stock price at t = 0
f = 2 # add and sub factor at each step 
n = 5 # no. of time steps to maturity
x = 100 # strike price
s = np.zeros((n+1,n+1))
s[0,0] = s0
for i in range(1,n+1): # setting first row
    s[0,i] = s[0,0] - f*i  #setting first row
for i in range(n,0,-1): #setting columns now starting from last colmn
    for j in range(1,i+1):
        s[j,i] = s[j-1,i] + 4

print()
print('__________________________________________________')
print(magenta('Binomial tree of Stock price at each node:',['bold', 'bright']))
print('__________________________________________________')
print(s)
print()

s0 = 100
st = np.array([90,94,98,102,106,110])
z = 0
n = 5 # number of time steps to maturity
p = 0.5 # risk neutral probability is always half
for i in range(0,n+1):
    dist = binom.pmf(i,n,p)
    p1 = binom.pmf(i,n,p)* max(s[i,5]-s0,0)
    print(magenta('Calculation for J=',['bold', 'bright']), i)
    print(round(p1,2))
    print()
    z = z + p1
    
print()
print('__________________________________________________')
print(magenta('Option Price (Total of Calculations for all J)',['bold', 'bright']))
print('Option price=', round(z,2))
print('__________________________________________________')
```
## Exercise 2 (Up and down movements in stock price are not constant but their rate is)
```python
# Setting up the known parameters
n = 6
s0 = 100
k = 100
u = 0.05
d = 0.04
r = 0.03

# Setting up the binomial tree of stock prices
s = np.zeros((n+1,n+1))
s[0,0] = s0
for i in range(1,7):
    for j in range(0,i+1):
        s[j,i] = round(s[0,0]* (1+u)**j * (1-d)**(i-j),2)
        
# Printing the matrix of stock prices

print('__________________________________________________')
print(magenta('Binomial tree of Stock price at each node:',['bold', 'bright']))

for i in range(0,7):
    print()
    print(s[i,:])
    
print('__________________________________________________')

# Calculating and printing risk neutral probability and Q    
print()
print()
print('__________________________________________________')
print(magenta('Risk-neutral probability and Q',['bold', 'bright']))

print()
p = (r + d)/(u + d)
print('p =',round(p,2))
print()
q1 = p*(1+u)
q2 = (1+r)
q = q1/q2
print('q =',round(q,2))
print('__________________________________________________')


# Setting up binomial PMF function to calculate option price

z1 = 0
z2 = 0
t = 6
for i in range(3,t+1):
    dist1 = binom.pmf(i,t,q)
    dist2 = binom.pmf(i,t,p)
    z1 = z1 + dist1
    z2 = z2 + dist2
    
# Printing the sum of two binomials
print()
print('__________________________________________________')
print(magenta('Sum of two binomial functions',['bold', 'bright']))

print()
print('z1=',round(z1,2))
print()
print('z2=',round(z2,2))
print('__________________________________________________')
print()
    
# Calculating the option price
a1 = z1*s0
disc1 = (1+r)**t
disc2 = k/disc1
a2 = z2*disc2
price = a1-a2
print('__________________________________________________')
print(magenta('Option Price',['bold', 'bright']))

print()
print('Option price=', round(price,2))
print('__________________________________________________')
```
