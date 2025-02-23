# Multi-Phase Cannonball

Maximizing the range of a cannonball in a vacuum is a typical
introductory problem for optimal control. In this example we are going
to demonstrate a more multidisciplinary take on the problem. We will
assume a density of the metal from which the cannonball is constructed,
and a cannon that can fire any diameter cannonball but is limited to a
maximum muzzle energy. If we make the cannonball large it will be heavy
and the cannon will not be capable of propelling it very far. If we make
the cannonball too small, it will have a low ballistic coefficient and
not be able to sustain its momentum in the presence of atmospheric drag.
Somewhere between these two extremes is the cannonball radius which
allows for maximum range flight.

The presence of atmospheric drag also means that we typically want to
launch the cannonball with more horizontal velocity, and thus use a
launch angle less than 45 degrees.

The goal of our optimization is to find the optimal design for the cannonball (its
radius) and the optimal flight profile (its launch angle)
simultaneously.

## Using two phases to capture an intermediate boundary constraint

This problem demonstrates the use of two phases to capture the state of
the system at an event in the trajectory. Here, we have the first phase
(ascent) terminate when the flight path angle reaches zero (apogee). The
descent phase follows until the cannonball impacts the ground.

The dynamics are given by

\begin{align}
  \frac{dv}{dt} &= \frac{D}{m} - g \sin \gamma \\
  \frac{d\gamma}{dt} &= - \frac{g \cos \gamma}{v} \\
  \frac{dh}{dt} &= v \sin \gamma \\
  \frac{dr}{dt} &= v \cos \gamma \\
\end{align}

The initial conditions are

\begin{align}
  r_0 &= 0 \rm{\,m} \\
  h_0 &= 100 \rm{\,m} \\
  v_0 &= \rm{free} \\
  \gamma_0 &= \rm{free}
\end{align}

and the final conditions are

\begin{align}
  h_f &= 0 \rm{\,m}
\end{align}

## Designing a cannonball for maximum range

This problem demonstrates a very simple vehicle design capability that
is run before the trajectory.

We assume our cannon can shoot a cannonball with some fixed kinetic
energy and that our cannonball is made of solid iron. The volume (and
mass) of the cannonball is proportional to its radius cubed, while the
cross-sectional area is proportional to its radius squared. If we
increase the size of the cannonball, the ballistic coefficient

\begin{align}
  BC &= \frac{m}{C_D A}
\end{align}

will increase, meaning the cannonball overcome air resistance more
easily and thus carry more distance.

However, making the cannonball larger also increases its mass. Our
cannon can impart the cannonball with, at most, 400 kJ of kinetic
energy. So making the cannonball larger will decrease the initial
velocity, and thus negatively impact its range.

We therefore have a design that affects the objective in competing ways.
We cannot make the cannonball too large, as it will be too heavy to
shoot. We also cannot make the cannonball too small, as it will be more
susceptible to air resistance. Somewhere in between is the sweet spot
that provides the maximum range cannonball.

## Building and running the problem

The following code defines the components for the physical
cannonball calculations and ODE problem, sets up trajectory using two phases,
and links them accordingly. The initial flight path angle is free, since
45 degrees is not necessarily optimal once air resistance is taken into
account.


{{ embed_test('dymos.examples.cannonball.doc.test_doc_two_phase_cannonball.TestTwoPhaseCannonballForDocs.test_two_phase_cannonball_for_docs') }}
