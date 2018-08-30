# drlnd_p1_navigation
This is my submission for Deep Reinforcement Learning Nanodegree (DRLND) Project 1.  The task is to create a policy that solves the "banana environment".

## Requirements

* pytorch
* numpy
* matplotlib

In addition, you need the Unity Environment linked to from the Project website.

## Install

In order to install, setup a conda environment in an Anaconda prompt

> conda create -n drlnd_p1_navigation

Install pytorch by following instructions on [pytorch.org](https://pytorch.org/)

Install the pre-built "Banana" environment using the instructions in the project.

Install other dependencies, e.g. 

> pip install matplotlib

## Project Files

* p1.py - outer loop runs the env and passes results to the learning agent
* agent.py - contains the (Double) DQN learning algorithm written in torch
* model.py - code for creating the Q(s,a) networks using torch

## Running the algorithm

Simply run:

> python p1.py

## Report

See [Report.md](Report.md)