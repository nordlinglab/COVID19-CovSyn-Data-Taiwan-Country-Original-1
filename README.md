# COVID19-CovSyn-Data-Taiwan-Country-Original-1

This repository contains **synthetic COVID-19 contact data for Taiwan**, generated using **CovSyn** with *original (unweighted) transmission parameters*.  
The dataset consists of **100 Monte Carlo simulations (seeds)**.

The data were generated using:

> **Wu, Yu-Heng, and Torbjörn E. M. Nordling (2025)**  
> *CovSyn: an agent-based model for synthesizing COVID-19 course of disease and contact tracing data.*  
> medRxiv, 2025-05  
> https://github.com/nordlinglab/COVID19-CovSyn.git

---

## 📌 Dataset Description

This repository specifically contains **contact-layer data for every infected individual**, organized by simulation seed.

Each file captures:
- Contact structure across multiple social layers
- Effective (infection-causing) contacts
- Timing of secondary infections
- Reinfection tracking indices
- Age attributes of secondary contacts

---

## 📂 File Naming Convention
contact_data_<seed>.npy


- `<seed>` ∈ {0, 1, …, 99}
- Each file corresponds to **one complete simulated outbreak**

---

## 🧠 Data Structure

Each `contact_data_<seed>.npy` file is a NumPy array with:

- **dtype:** `object`
- **shape:** `(N,)`, where `N` = number of infected individuals in that simulation

Each element in the array corresponds to **one infected individual** and is represented as a Python dictionary.

---

## 🧬 Contact Layers Included

Each dictionary contains contact information for the following **five social layers**:

- Household  
- School  
- Workplace  
- Healthcare  
- Municipality (community contacts)

Each layer includes the same set of fields.

---

## 📑 Dictionary Fields (per contact layer)

### Example: Household Layer
- `household_contacts_matrix`  
  Boolean contact matrix indicating potential contacts

- `household_effective_contacts`  
  Binary list indicating which contacts resulted in infection (1 = infection occurred)

- `household_effective_contacts_infection_time`  
  Time (days since index infection) when secondary infection occurred

- `household_secondary_contact_ages`  
  Age of each contacted individual

- `household_previously_infected_index_list`  
  Index of previously infected individual if reinfection occurred (`NaN` otherwise)

### Other Layers
The **school**, **workplace**, **healthcare**, and **municipality** layers contain **identical field names**, prefixed by their respective layer identifiers.

---

## 📌 Definition: Effective Contact

An **effective contact** is defined as:

> A social-layer encounter occurring during the infectious period that **successfully transmitted infection** to a secondary individual.

These effective contacts correspond directly to transmission edges stored in the companion `case_edge_list` dataset.

---

## 🧪 Example Inspection (Python)

```python
import numpy as np

data = np.load("contact_data_23.npy", allow_pickle=True)

print(type(data), data.dtype, data.shape)
print(data[0].keys())

