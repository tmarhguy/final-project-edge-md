[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/-Acvnhrq)

# EdgeMD — Final Project

> **Team 6 · University of Pennsylvania**

| Team Member | Email |
|-------------|-------|
| Justin Monchais | monchais@seas.upenn.edu |
| Damodar Pai | damodarp@wharton.upenn.edu |
| Tyrone Marhguy | tmarhguy@seas.upenn.edu |

**GitHub Repository:** https://github.com/upenn-embedded/final-project-edge-md/

**GitHub Pages:** *(link to be added for final submission)*

---

## Table of Contents

1. [Abstract](#1-abstract)
2. [Motivation](#2-motivation)
3. [System Block Diagram](#3-system-block-diagram)
4. [Design Sketches](#4-design-sketches)
5. [Software Requirements Specification (SRS)](#5-software-requirements-specification-srs)
6. [Hardware Requirements Specification (HRS)](#6-hardware-requirements-specification-hrs)
7. [Bill of Materials (BOM)](#7-bill-of-materials-bom)
8. [Final Demo Goals](#8-final-demo-goals)
9. [Sprint Planning](#9-sprint-planning)
- [Sprint Review #1](#sprint-review-1)
- [Sprint Review #2](#sprint-review-2)
- [MVP Demo](#mvp-demo)
- [Final Report](#final-report)

---

## Project Proposal

### 1. Abstract

EdgeMD is a portable English–Spanish medical translator that trains on the cloud and runs inference on the edge. The device accepts spoken audio input, filters background noise, and outputs a clear translation through a speaker — enabling real-time bilingual communication between doctors and patients without requiring network connectivity at the point of care.

### 2. Motivation

Many patients in the US and in underserved communities abroad require a translator to communicate with their doctor. Relying on a human intermediary raises HIPAA concerns and limits the number of patients a doctor can serve. A compact, low-cost translation device eliminates that bottleneck — allowing NGOs, clinics, and field physicians to extend care to more patients regardless of language barriers.

### 3. System Block Diagram

![Block Diagram](media/blockDiagram.png)

### 4. Design Sketches

![Design Sketch](media/firstSketch.png)

---

### 5. Software Requirements Specification (SRS)

The software pipeline covers three stages: speech-to-text, translation, and post-processing. A pretrained model (Whisper or a Hugging Face translation model) runs on the edge device for fast inference, while a medical dictionary enforces terminology accuracy.

**5.1 Definitions & Abbreviations**

| Term | Definition |
|------|------------|
| STT | Speech-to-Text |
| TTS | Text-to-Speech |
| SRS | Software Requirements Specification |
| NLP | Natural Language Processing |

**5.2 Functional Requirements**

| ID | Description |
|----|-------------|
| `SRS-01` | Convert spoken English or Spanish input into text with **≥ 90% accuracy** under low-noise conditions, measured against ground-truth transcriptions. |
| `SRS-02` | Translate text between English and Spanish with **≥ 85% semantic accuracy** on a predefined set of medical phrases and sentences. |
| `SRS-03` | Apply a medical dictionary so critical medical terms are translated correctly in **≥ 95% of test cases**. |
| `SRS-04` | Produce translated audio output **within 15 seconds** of input completion, measured end-to-end from speech input to playback. |
| `SRS-05` | Support basic conversational exchanges by preserving meaning across **at least two consecutive sentences**. |
| `SRS-06` | Detect low-confidence or failed translations and trigger an error state or retry in **≥ 90% of such cases**. |

---

### 6. Hardware Requirements Specification (HRS)

On a button press, the device begins recording through the MEMS microphone, digitizes the signal via I2S, routes it through the translation pipeline, and plays back the translated result through an I2S amplifier and speaker. LEDs provide continuous visual feedback on system state.

**6.1 Definitions & Abbreviations**

| Term | Definition |
|------|------------|
| I2S | Inter-IC Sound (serial audio bus) |
| ADC | Analog-to-Digital Converter |
| DAC | Digital-to-Analog Converter |
| GPIO | General-Purpose Input/Output |
| HRS | Hardware Requirements Specification |

**6.2 Functional Requirements**

| ID | Description |
|----|-------------|
| `HRS-01` | LEDs shall indicate system state: power on, active listening, processing, and error conditions (e.g., failed or low-confidence translations). |
| `HRS-02` | The audio input path shall capture intelligible speech in environments with minimal background noise, suitable for clinical-style use. |
| `HRS-03` | The system shall support real-time or near-real-time operation so speech is translated and played back with minimal perceptible delay. |
| `HRS-04` | Tactile push buttons shall provide user control (e.g., initiating recording and switching modes) via GPIO to the STM32. |
| `HRS-05` | The audio output path shall drive a speaker through the processed translation (I2S amplifier + 4Ω speaker) for intelligible playback. |

---

### 7. Bill of Materials (BOM)

| Component | Description | Qty |
|-----------|-------------|-----|
| Raspberry Pi 5 (8 GB) | Edge inference host | 1 |
| RPi 5 27W USB-C Power Supply | Official power supply | 1 |
| MicroSD Card, 128 GB (A2) | Storage (Samsung EVO Plus or equivalent) | 1 |
| STM32F446RE Nucleo-64 | Microcontroller development board | 1 |
| INMP441 I2S MEMS Microphone | Audio input breakout | 1 |
| MAX98357A I2S Class-D Amplifier | Audio output breakout | 1 |
| Speaker, 3W 4Ω (40 mm) | Audio output driver | 1 |
| Resistor, 220 Ω | Current limiting for LEDs | 5 |
| Resistor, 10 kΩ | Pull-ups for push buttons | 4 |
| LEDs (assorted colors) | System state indicators | 4 |
| Tactile Push Buttons (12 mm, through-hole) | User input | 4 |
| Logic Analyzer | I2S / GPIO debug tool | 1 |

---

### 8. Final Demo Goals

The demo will cover three scenarios:

1. **Single-sentence translation** — validates basic functionality and end-to-end latency.
2. **Multi-turn conversation** — demonstrates continuous English↔Spanish interaction between a simulated doctor and patient.
3. **Failure & recovery** — injects a low-confidence translation to show that the error-detection and retry mechanism functions correctly.

---

### 9. Sprint Planning

**Deadline:** April 24

| Milestone | Functionality Achieved | Distribution of Work |
|-----------|------------------------|----------------------|
| **Sprint 1** | Set up the development environment; assign roles; gather hardware; implement a basic translation pipeline using a pretrained model (Whisper or Hugging Face). | Software lead: model setup and testing. Hardware lead: component sourcing and verification. Full team: initial pipeline integration. |
| **Sprint 2** | Integrate microphone and speaker with the Raspberry Pi; connect the translation pipeline so spoken input produces translated audio output. | Audio lead: microphone and speaker integration. Software lead: pipeline connection. Full team: end-to-end testing. |
| **MVP Demo** | Integrate the full hardware system (STM32, LEDs, buttons); improve reliability with noise handling, medical dictionary, and stable real-time performance. | Full team across hardware (STM32, LEDs, buttons), software (noise filtering, dictionary, real-time tuning), and integration testing. |
| **Final Demo** | Debug and polish the system; prepare demo scenarios; finalize documentation; demonstrate simple translation, continuous conversation, and failure recovery. | One member: translation model and software. One member: audio processing and integration. One member: hardware and system control. Regular syncs to ensure all components work together. |

---

> **The sections below will be completed according to the milestone schedule.**

---

## Sprint Review #1

### Last Week's Progress

*(To be filled in)*

### Current State of Project

*(To be filled in)*

### Next Week's Plan

*(To be filled in)*

---

## Sprint Review #2

### Last Week's Progress

*(To be filled in)*

### Current State of Project

*(To be filled in)*

### Next Week's Plan

*(To be filled in)*

---

## MVP Demo

*(To be filled in)*

---

## Final Report

### 1. Video

*(To be added)*

### 2. Images

*(To be added)*

### 3. Results

#### 3.1 SRS Validation

| ID | Description | Validation Outcome |
|----|-------------|--------------------|
| `SRS-01` | STT accuracy ≥ 90% under low-noise conditions. | TBD — transcription logs vs. ground truth in `validation/`. |
| `SRS-02` | Translation semantic accuracy ≥ 85% on medical phrase set. | TBD — evaluation protocol and results in `validation/`. |
| `SRS-03` | Medical dictionary correctness ≥ 95% on critical terms. | TBD — dictionary test results in `validation/`. |
| `SRS-04` | End-to-end latency ≤ 15 seconds from speech input to playback. | TBD — timing measurements in `validation/`. |
| `SRS-05` | Context preserved across ≥ 2 consecutive sentences. | TBD — scenario test results in `validation/`. |
| `SRS-06` | Error/retry triggered in ≥ 90% of low-confidence cases. | TBD — failure-injection test logs in `validation/`. |

#### 3.2 HRS Validation

| ID | Description | Validation Outcome |
|----|-------------|--------------------|
| `HRS-01` | LEDs indicate all four system states correctly. | TBD — photo or video of each state in `validation/`. |
| `HRS-02` | Microphone captures intelligible speech under minimal noise. | TBD — audio samples or analyzer captures in `validation/`. |
| `HRS-03` | Real-time or near-real-time operation with minimal delay. | TBD — latency notes or logs in `validation/`. |
| `HRS-04` | Push buttons control pipeline via GPIO on the STM32. | TBD — demo or GPIO scope trace in `validation/`. |
| `HRS-05` | Speaker produces intelligible translated audio via I2S amplifier. | TBD — playback demo or measurements in `validation/`. |

### 4. Conclusion

*(To be filled in)*

---

## References

*(To be added)*
