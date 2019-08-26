---
title: "Statistical learning mitigation of false positives from template-detected data in automated acoustic wildlife monitoring"
layout: post
---

My second publication from my PhD work is out: [Statistical learning mitigation of false positives from template-detected data in automated acoustic wildlife monitoring](https://www.tandfonline.com/doi/full/10.1080/09524622.2019.1605309), published as an open access article in Bioacoustics. The Github repository accompanying the paper is located at [https://github.com/cbalantic/false-positive-mitigation](https://github.com/cbalantic/false-positive-mitigation).

Abstract: Audio sampling of the environment can provide long-term, landscape-scale presence-absence data to model populations of sound-producing wildlife. Automated detection systems allow researchers to avoid manually searching through large volumes of recordings, but often produce unacceptable false positive rates. We developed methods that allow researchers to improve template-based automated detection using a suite of statistical learning algorithms when false positive rates are problematic. To test our method, we acquired 668 hours of recordings in the Sonoran Desert, California USA between March 2016 and May 2017, and created spectrogram cross-correlation templates for three target avian species. We trained and tested five classification algorithms and four performance-weighted ensemble classifier methods on target signals and false alarms from March 2016, and then selected high-performing ensemble classifiers from the train/test phase to predict the class of new detections thereafter. For three target species, our ensemble classifiers were able to identify 98%, 81%, and 100% of false alarms compared with the baseline template detection system, and comparative positive predictive values improved from 6% to 69%, 87% to 95%, and 2% to 77%. We show that statistical learning approaches can be implemented to mitigate false detections acquired via template-based automated detection in automated acoustic wildlife monitoring.