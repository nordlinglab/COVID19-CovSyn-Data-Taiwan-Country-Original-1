# COVID19-CovSyn-Data-Taiwan-Country-Original-1

This repository contains **synthetic COVID-19 epidemic data for Taiwan**, generated using **CovSyn** with *original (unweighted) transmission parameters*.  
The dataset consists of **100 Monte Carlo simulation runs (seeds)**, each representing a complete synthetic outbreak.

The data were generated using:

> **Wu, Yu-Heng, and Torbjörn E. M. Nordling (2025)**  
> *CovSyn: an agent-based model for synthesizing COVID-19 course of disease and contact tracing data.*  
> medRxiv, 2025-05  
> https://github.com/nordlinglab/COVID19-CovSyn.git

---

## 📌 Dataset Description

This repository provides the **full set of CovSyn simulation outputs** for the original Taiwan configuration, including:

- Demographic attributes of infected individuals  
- Social-structure assignments (household, school, workplace, healthcare, municipality)  
- Contact matrices and effective (infection-causing) contacts  
- Transmission edges linking infectors to infectees  
- Detailed course-of-disease timelines  

All data are stored in NumPy (`.npy`) format and organized by simulation seed.

---

## 📂 Contents and File Naming Convention

For each simulation seed `<seed> ∈ {0, 1, …, 99}`, the following files are provided:
contact_data_<seed>.npy
case_edge_list_<seed>.npy
demographic_data_<seed>.npy
social_data_<seed>.npy
course_of_disease_data_<seed>.npy


Each set of files corresponds to **one complete simulated outbreak**.

---

## 🧠 General Data Structure

All `.npy` files store Python objects using NumPy arrays with:

- **dtype:** `object`  
- **length:** equal to the number of infected individuals (or transmission events, for edge lists)

The data are designed to be directly loadable in Python using `numpy.load(..., allow_pickle=True)`.

---

## 🧬 Contact Data (`contact_data_<seed>.npy`)

Each `contact_data_<seed>.npy` file is a NumPy array where:

- Each entry corresponds to **one infected individual**
- Each entry is a dictionary containing contact information across **five social layers**:
  - Household  
  - School  
  - Workplace  
  - Healthcare  
  - Municipality (community contacts)

Each layer contains the same set of fields.

### 📑 Example: Household Layer Fields
- `household_contacts_matrix`  
  Boolean matrix indicating potential contacts

- `household_effective_contacts`  
  Binary list indicating which contacts resulted in infection (`1 = infection occurred`)

- `household_effective_contacts_infection_time`  
  Time (days since index infection) when secondary infection occurred

- `household_secondary_contact_ages`  
  Age of each contacted individual

- `household_previously_infected_index_list`  
  Index of previously infected individual if reinfection occurred (`NaN` otherwise)

The **school**, **workplace**, **healthcare**, and **municipality** layers contain identical field structures with layer-specific prefixes.

---

## 📌 Definition: Effective Contact

An **effective contact** is defined as:

> A social-layer encounter occurring during the infectious period that **successfully transmitted infection** to a secondary individual.

These effective contacts correspond directly to transmission edges stored in the companion `case_edge_list_<seed>.npy` files.

---

## 🔗 Transmission Edges (`case_edge_list_<seed>.npy`)

Each file contains a list of transmission events represented as tuples: (source_case_id, infectee_case_id, infection_day, contact_layer)


## 👤 Demographic Data (`demographic_data_<seed>.npy`)

Each entry corresponds to one infected individual and includes:

- Age  
- Gender  
- Job or role category  

Demographic attributes are sampled to reflect Taiwan’s population structure.

---

## 🏘️ Social Data (`social_data_<seed>.npy`)

This file stores social-structure assignments for each infected individual, including:

- Municipality  
- Household size  
- School class size (if applicable)  
- Workplace group size  
- Healthcare facility size (if applicable)  

These attributes determine potential contact opportunities in the simulation.

---

## 🧬 Course-of-Disease Data (`course_of_disease_data_<seed>.npy`)

Each entry contains the natural-history timeline for one infected individual, including:

- Infection day  
- Latent period  
- Incubation period  
- Infectious period  
- Symptom onset  
- Testing dates  
- Recovery or death dates  
- Natural immunity status  

Disease timelines are sampled using parametric distributions calibrated within CovSyn.

---

## 🧪 Example Inspection (Python)

```python
import numpy as np

contact = np.load("contact_data_23.npy", allow_pickle=True)
edges = np.load("case_edge_list_23.npy", allow_pickle=True)

print(type(contact), contact.dtype, contact.shape)
print(contact[0].keys())

