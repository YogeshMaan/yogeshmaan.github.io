This document contains a detailed description of How to formulate a optimization problem for a given dynamic system and solve it as an MIQP. The dynamic system follows piecewise affine trajectories and involves mode scheduling.

**Further documentation is done in Latex for towards thesis.** [May 14, 2024]

## Overview


## Introduction

## Problem formulation
At the end, am I designing an MPC controller for a desired control input to the dynamic system? How's a optimization-based trajectory planner different from a normal trajectory planner (given obstacle map, start_post(), end_pose())? 


### Dynamic system

### Cost Function and constraint

**Cost function**: Quadratic in terminal state vector

**Constraints**: How to constraint $\bold{u[:,~1:T] = 0}$ such that the mpc controller solves for $\bold{u[:,~0] = u_0}$ only?

## Code

### Dependencies

### Flow diagram

### Simulation Results

## Learning resources 

### Topics

### Research papers
- Robotics at Google: Planar Quadcopter with inverted pendulum https://arxiv.org/pdf/2109.07081.pdf

### Questions
1. Can I assume perfect collisions for the quadrotor and plan an time-optimal trajectory?
