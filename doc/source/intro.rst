.. _introduction:

Introduction
!!!!!!!!!!!!

This is the core product documentation for OmicMD. Here you will find the information you need to get up to speed on the architecture, product features, and current code, as well as where you will document code you write in a user-friendly way. 


If you need background
@@@@@@@@@@@@@@@@@@@@@@
For more details on web APIs, see `this wikipedia page <https://en.wikipedia.org/wiki/Web_API>`_.
REST protocols for data transfer are described `here <https://en.wikipedia.org/wiki/Representational_state_transfer>`_.
Wikipedia also has good overviews of `bioinformatics <https://en.wikipedia.org/wiki/Bioinformatics>`_
and `DNA sequencing <https://en.wikipedia.org/wiki/DNA_sequencing>`_


General Overview
@@@@@@@@@@@@@@@@

The product contains an organized collection of research and medical databases, hosted in a secure, HIPAA-compliant location. The key data are integrated and exposed through services via an API-led architecture including system, process, and experience APIs. As such, we are not constructing a data lake; rather a highly-available distributed network of data sources and applications, including analysis "tracks" layered on for things like machine learning predictions of disease risk or drug efficacy. 