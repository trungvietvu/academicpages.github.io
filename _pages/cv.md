---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

[[pdf]](/files/CV_TrungVu.pdf)

Education
======
* B.S. in Computer Science, Hanoi University of Science and Technology, 2014
* Ph.D in Computer Science, Oregon State University, 2021 (expected)

Research Interest
======
My research focuses on scalable optimization methods in machine learning and signal processing. I am passionate about the challenging problems that arise in the core of optimization methods such as the convergence guarantees and the accommodation of acceleration. Currently, I am working on the theoretical analysis of projected gradient descent for structured non-convex problems such as sparse recovery and low-rank matrix completion.
{: .text-justify}

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
Conference Activity/Participation
======
* Papers Presented
  * On Convergence of Projected Gradient Descent for Minimizing a Large-scale Quadratic over the Unit Sphere, MLSP 2019, October 13-16, Pittsburgh, US.
  * Accelerating Iterative Hard Thresholding for Low-Rank Matrix Completion via Adaptive Restart, ICASSP 2019, May 11-17, London, UK.
* Posters
  * Local Convergence of the Heavy Ball method in Iterative Hard Thresholding for Low-Rank Matrix Completion, ICASSP 2019, May 11-17, London, UK.
  * Adaptive Step Size Momentum Method for Deconvolution, SSP Workshop 2018, June 10-13.
* Campus or Departmental Talks
  * Accelerating Iterative Hard Thresholding for Low-Rank Matrix Completion via Adaptive Restart, Signal Processing group, March 1, 2019.
  * Adaptive Step Size Momentum Method for Deconvolution, April, 2018.

Work experience
======
* Winter 2017 - Fall 2018: Research Assistant
  * Oregon State University
  * Duties included: developing an intelligent decision support system to improve irrigation management in vineyards and other west coast crops
  * Supervisor: Dr. Raviv Raich

Teaching Assistant
======
  <ul>{% for post in site.teaching reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
Service
======
* Reviewer
  * ICASSP 2018, 2019
  * MLSP 2019
