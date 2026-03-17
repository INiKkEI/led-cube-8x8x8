# Project Summary

This project is a portfolio-focused embedded-systems build centered on a custom **8×8×8 monochrome LED cube**. The goal is to produce a working hardware system and document the full engineering path clearly enough that another person can understand not only the final result, but also the design logic behind it.

The system is based on a **custom ESP32 control PCB** that powers, controls, and refreshes a **512-LED cube** through a dedicated driver stage and multiplexed layer scanning. The intended end-user interface for revision 1 is **BLE smartphone control**, while programming and bring-up remain available through a development interface.

The repository is structured as an engineering record rather than only a code dump. It is meant to show requirements definition, architecture locking, hardware/firmware boundary decisions, bring-up preparation, validation planning, and later implementation results. In portfolio terms, the value of the project is the combination of a real embedded build and a traceable, professional development workflow.
