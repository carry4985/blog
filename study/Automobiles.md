# Automobiles

## Lesson 1 localization

### localization

traditional way: satellites (GPS) mot very accurate ~10m need:2cm-10cm

### total probability

x-axis: location

y-axis: belief

all same: uniform maximum confusion

other: posterior

If you see a door, then more probability behind a door. Others: sensor error.

After move, the peak move: convolution.

### sense and move

sense : gain information

move : lose information

sense : initial belief

Bayes theorem reminder:

<img src="http://latex.codecogs.com/gif.latex?P(A\mid%20B)=\frac{P(B\mid%20A)%20\times%20P(A)}{P(B)}">

## Motion-Total Probability

<img src="http://latex.codecogs.com/gif.latex?P(X_i^{t})%20=%20\sum_j%20P(X^{t-1}_j)%20\dot%20P(X_i\mid%20X_j)">


t - time

i - grid cell

## Kalman Filters

kalman filter: continuous

distribution: gaussian

**two steps:**

* measurement update : Bayes rule, product
* prediction: total probability, convolution

### measurement update

Notice: the new mean is between the previous two means and the new variance is LESS than either of the previous variance.

<img src="http://latex.codecogs.com/gif.latex?\mu'=\frac{1}{\sigma&space;^2&space;&plus;&space;\gamma^2}[\gamma^2&space;\mu&space;&plus;&space;\sigma&space;^&space;2&space;\nu]\\&space;\sigma'^2&space;=&space;\frac{1}{\frac{1}{\sigma^2}&plus;\frac{1}{\gamma^2}}" title="\mu'=\frac{1}{\sigma ^2 + \gamma^2}[\gamma^2 \mu + \sigma ^ 2 \gamma]\\ \sigma'^2 = \frac{1}{\frac{1}{\sigma^2}+\frac{1}{\gamma^2}}" />

### motion

<img src="http://latex.codecogs.com/gif.latex?\mu'&space;=&space;\mu&space;&plus;&space;\nu&space;\\\sigma'^2&space;=&space;\sigma&space;^2&space;&plus;&space;\gamma&space;^2" title="\mu' = \mu + \nu \\\sigma'^2 = \sigma ^2 + \gamma ^2" />


### 2-d kalman

<img src="http://latex.codecogs.com/gif.latex?x'&space;\leftarrow&space;x&space;&plus;&space;\dot{x}&space;\\&space;\dot{x}'&space;\leftarrow&space;\dot{x}" title="x' \leftarrow x + \dot{x} \\ \dot{x}' \leftarrow \dot{x}" />

design a kalman filter

<img src="http://latex.codecogs.com/gif.latex?%5Cbegin%7Bpmatrix%7D%20x%27%5C%5C%20%5Cdot%7Bx%7D%27%20%5Cend%7Bpmatrix%7D%5Cleftarrow%20%5Cbegin%7Bpmatrix%7D%201%20%26%201%5C%5C%200%20%26%201%20%5Cend%7Bpmatrix%7D%5Cbegin%7Bpmatrix%7D%20x%5C%5C%20%5Cdot%7Bx%7D%20%5Cend%7Bpmatrix%7D" />

<img src="http://latex.codecogs.com/gif.latex?z%5Cleftarrow%20%5Cbegin%7Bpmatrix%7D%201%20%26%200%20%5Cend%7Bpmatrix%7D%5Cbegin%7Bpmatrix%7D%20x%5C%5C%20%5Cdot%7Bx%7D%20%5Cend%7Bpmatrix%7D" />

prediction:

x: estimate

P: uncertainty covariance

F: state transition matrix
<img src="http://latex.codecogs.com/gif.latex?%5Cbegin%7Bpmatrix%7D%201%20%26%201%5C%5C%200%20%26%201%20%5Cend%7Bpmatrix%7D%" />

v: motion vector

<img src="http://latex.codecogs.com/gif.latex?x%27%20%3D%20Fx%20&plus;%20v" />

<img src="http://latex.codecogs.com/gif.latex?P%27%20%3D%20F%5Ccdot%20P%20%5Ccdot%20F%5ET" />

measurement update

z: measurement

H: measurement function <img src="http://latex.codecogs.com/gif.latex?%5Cbegin%7Bpmatrix%7D%201%20%26%200%20%5Cend%7Bpmatrix%7D" />

R: measurement noise

I: identity matrix

<img src="http://latex.codecogs.com/gif.latex?y%20%3D%20z%20-H%20%5Ccdot%20x" />

<img src="http://latex.codecogs.com/gif.latex?S%20%3D%20H%20%5Ccdot%20P%20%5Ccdot%20H%20%5E%20T%20&plus;%20R" />

<img src="http://latex.codecogs.com/gif.latex?K%20%3D%20P%20%5Ccdot%20H%5ET%20%5Ccdot%20S%20%5E%7B-1%7D" />

<img src="http://latex.codecogs.com/gif.latex?x%27%20%3D%20x%20&plus;%20%28K%20%5Ccdot%20y%29" />

<img src="http://latex.codecogs.com/gif.latex?P%27%20%3D%20%28I%20-%20K%20%5Ccdot%20H%29%5Ccdot%20P" />

#### Notice

The kalman filter introduced above is only fit the linear system. When it comes to nonlinear system, EKF or UKF or PF are more useful.

#### Extended Kalman filter

##### A simple example

Imagine an airplane, we can think of the current altitude as a fraction of the previous altitude. The altitude at the current time is 98% of its altitude at the previous time:

<img src="http://latex.codecogs.com/gif.latex?altitude_{current\_time}%20=%200.98%20\times%20altitude_{previous\_time}" />

##### Dealing with noise

Real-world measurement like altitude are obtained from sensors like a GPS or barometer. These sensors offer varying degrees of accuracy. So these noises add a "noisy" to the observed sensor reading.

 <img src="http://latex.codecogs.com/gif.latex?observed\_altitude_{current\_time}%20=%20altitude_{current\_time}+noise_{current\_time}" />

##### Putting it together

To make the above two equations more general:

 <img src="http://latex.codecogs.com/gif.latex?x" /> - current state of our system,

 <img src="http://latex.codecogs.com/gif.latex?x%20_%20{k%20-%201}" /> - previous state,

a - some constant,

<img src="http://latex.codecogs.com/gif.latex?z_k" /> - our current observation of the system,

<img src="http://latex.codecogs.com/gif.latex?v_k" /> - the current noise measurement,

<img src="http://latex.codecogs.com/gif.latex?x_k=ax_{k-1}" />

 <img src="http://latex.codecogs.com/gif.latex?z_k%20=%20x_k%20+%20v_k" />

 So the current observed altitude is defined as:

  <img src="http://latex.codecogs.com/gif.latex?altitude_{current\_time}=0.98\times%20altitude_{previous\_time}+turbulence_{current\_time}" />

  More generally:

<img src="http://latex.codecogs.com/gif.latex?w_k" /> - process noise, like turbulent, it is an inherent part of the process but not an artificial of observation or measurement. We will ignore process noise for a while in order to focus on the other topics.

<img src="http://latex.codecogs.com/gif.latex?x_k=ax_{k-1}+w_k" />



### Particle Filter

#### Resampling

draw N from the particles, every particle has a weight, the weight is proportional to the distance between the particle and the landmark



then build a resample wheel

### Bicycle models

TURNING ANGLE - <img src="http://latex.codecogs.com/gif.latex?\beta" />

MOVING DISTANCE - d

CAR LENGTH - L

STEERING ANGLE - <img src="http://latex.codecogs.com/gif.latex?\alpha" />

RADIUS - R

<img src="http://latex.codecogs.com/gif.latex?\beta%20=%20\frac{d}{L}\cdot%20tan(\alpha)" />

<img src="http://latex.codecogs.com/gif.latex?R%20=%20\frac{d}{\beta}" />

x-center of car - <img src="http://latex.codecogs.com/gif.latex?c_x" />

y-center of car - <img src="http://latex.codecogs.com/gif.latex?c_y" />

x-center of back wheel - x

y-center of back wheel - y

rotation angle of back wheel - <img src="http://latex.codecogs.com/gif.latex?\theta" />

<img src="http://latex.codecogs.com/gif.latex?c_x%20=%20x%20-%20sin%20(\theta)\cdot%20R" />

<img src="http://latex.codecogs.com/gif.latex?c_y%20=%20y%20-%20cos%20(\theta)\cdot%20R" />

<img src="http://latex.codecogs.com/gif.latex?x_1%27%20=%20c_x%20+%20sin(\theta%20+%20\beta)\cdot%20R" />

<img src="http://latex.codecogs.com/gif.latex?y_1%27%20=%20c_y%20+%20cos(\theta%20+%20\beta)\cdot%20R" />

<img src="http://latex.codecogs.com/gif.latex?\theta%27%20=%20(\theta%20+%20\beta)%20mod%202%20\pi" />

if <img src="http://latex.codecogs.com/gif.latex?\mid%20\beta%20\mid%20%3C%200.001" />

<img src="http://latex.codecogs.com/gif.latex?x%27%20=%20x%20+%20d%20\cdot%20cos%20\theta" />

<img src="http://latex.codecogs.com/gif.latex?y%27%20=%20y%20+%20d%20\cdot%20sin%20\theta" />

<img
 src="http://latex.codecogs.com/gif.latex?\theta%20%27%20=%20(\theta%20+%20\beta)%20mod%202%20\pi" />

### motion planning

A star disadvantages: cannot deal with branching outcomes, cannot deal with information gathering

### robot  

smooth:

<img src="http://latex.codecogs.com/gif.latex?y_i=y_i+\alpha%20(x_i%20-%20y_i)%20+%20\beta%20(y_{i+1}%20+%20y_{i-1}%20-2%20\times%20y_i)" />

### P control

p: steering = -tau * crosstrack_error

p may lead to overshoot

p larger -> oscillates faster

<img src="http://latex.codecogs.com/gif.latex?\alpha%20=%20-\tau%20_p%20\times%20CTE" />

### PD control

D to decrease overshooting

<img src="http://latex.codecogs.com/gif.latex?\alpha%20=%20-\tau%20_p%20\times%20CTE%20-%20\tau%20_d\frac{d}{dt}CTE" />

### PID control

I to decrease stead error

<img src="http://latex.codecogs.com/gif.latex?\alpha%20=%20-\tau%20_p%20\times%20CTE%20-%20\tau%20_d\frac{d}{dt}CTE-\tau_I%20\sum{CTE}" />

### how to assign parameters: TWIDDLE

    run() -> return a goodness, always the average crosstrack error
------------------------

    p = [0,0,0] #parameters
    dp = [1,1,1] #potential changes

    best_err = run(p)
    for i in range(len(p)):
      p[i] += dp[i]
      err = run(p)

    if err < best_err:
      best_err = err
      dp[i] *= 1.1
    else:
      p[i] -= 2dp[i]
    .
    .
    .
    else:
      p[i] += dp[i]
      dp[i] *= 0

  ### slam

  mu = omega^{-1}*xi