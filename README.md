# High-Efficiency Silicon Nitride Grating Coupler (1030 nm) 

![Photonics](https://img.shields.io/badge/Photonics-Silicon_Nitride-blue?style=flat-square)
![Simulation](https://img.shields.io/badge/Simulation-Ansys_Lumerical_FDTD-orange?style=flat-square)
![Efficiency](https://img.shields.io/badge/Peak_Efficiency-38%25-success?style=flat-square)
![Wavelength](https://img.shields.io/badge/Target_Wavelength-1030_nm_(Yb_Laser)-purple?style=flat-square)

This repository contains the 3D FDTD simulation models, Python (`gdspy`) layout generation scripts, and extracted physical parameters for a highly optimized, focusing grating coupler. It is designed to couple light between a standard optical fiber and a Silicon Nitride ($Si_3N_4$) photonic integrated circuit (PIC) at the **1030 nm** wavelength, specifically targeted for Ytterbium (Yb) laser applications.

## 📖 What is a Grating Coupler?
In silicon photonics, light traveling through an optical fiber is far too large (mode diameter ~10 µm) to directly enter a sub-micron on-chip waveguide. A grating coupler solves this mode-mismatch by acting as a microscopic diffraction grating. It intercepts the light shining down from a tilted fiber and scatters it perfectly horizontally at a 90-degree angle, shrinking and guiding the light into the planar waveguide mode. 

## ⚠️ The Engineering Challenge: Fixed Substrate Constraints
Typically, grating couplers are optimized by adjusting the thickness of the waveguide and the underlying Buried Oxide (BOX) layer to force constructive interference between upward-scattering and downward-scattering light. 

This design achieved a remarkable **~38% coupling efficiency (~4.2 dB Insertion Loss)** under extreme, locked boundary conditions:

1. **Fixed 1 µm $Si_3N_4$ Thickness:** At 1 µm thick, Silicon Nitride is highly multi-mode at the 1030 nm wavelength. Injecting light into such a thick slab causes massive modal scattering, where optical power fractures into higher-order modes instead of the desired fundamental TE0 mode.
2. **Fixed 2 µm BOX Layer:** Downward-diffracted light bounces off the silicon substrate and travels back up through the BOX layer. Ideally, the BOX thickness is perfectly tuned to ensure this reflected wave constructively interferes with the upward-diffracted wave. A fixed 2 µm BOX destroys this geometric tuning capability, forcing all optimization to occur purely on the surface geometry of the grating teeth.

## 🔬 Final Optimized Geometry
* **Target Wavelength:** 1030 nm
* **Pitch (Grating Period):** 0.76 µm
* **Duty Cycle:** 0.51
* **Etch Depth:** 0.366 µm (Shallow etch into the 1 µm base)
* **Focusing Radius:** 40.0 µm
* **Waveguide Width:** 0.45 µm

## 📊 Tuning Parameters & Their Effects on Transmission
Optimizing this design required balancing the Bragg phase-matching condition:

$$\beta - k_0 n_{c} \sin(\theta) = \frac{2\pi}{\Lambda}$$

Where $\beta$ is the propagation constant, $n_c$ is the cladding index, $\theta$ is the fiber tilt angle, and $\Lambda$ is the grating pitch. Below is a breakdown of how altering individual physical parameters affects the transmission graph.

| Parameter | What it is | Effect on Transmission Graph |
| :--- | :--- | :--- |
| **Grating Period (Pitch)** | The length of one full cycle of the grating (one tooth + one trench). | **Shifts the Peak $\lambda$.** Increasing the pitch shifts the peak efficiency strictly to the right (longer wavelengths/red-shift). Decreasing it shifts the peak to the left (blue-shift). |
| **Duty Cycle (Fill Factor)** | The ratio of the unetched tooth width to the total pitch. | **Shifts Peak & Alters Efficiency.** Changing the duty cycle alters the effective refractive index ($n_{eff}$) of the grating region. It gently shifts the center wavelength while also changing the peak amplitude. |
| **Etch Depth** | How deep the trenches are carved into the $Si_3N_4$ slab. | **Controls Bandwidth & Scattering.** A deeper etch increases the index contrast, resulting in stronger scattering, a broader optical bandwidth, and a shorter coupling length. However, going too deep causes massive back-reflection into the fiber. |
| **Fiber Tilt Angle** | The injection angle of the optical fiber (typically 8° to 12°). | **Massive Wavelength Shift.** Changing the angle drastically alters the Bragg phase-matching condition. An increased angle pushes the peak efficiency to significantly shorter wavelengths. We tilt the fiber to avoid 2nd-order Bragg reflections that would shoot light straight back up the fiber. |
| **Focusing Radius** | The initial sweeping curve radius of the grating arcs. | **Mode Field Matching.** Adjusting this ensures the curved wavefronts perfectly converge into the narrow width of the single-mode output waveguide. If the radius is misaligned with the focal length, the peak efficiency drops instantly without shifting the wavelength. |

## 🛠️ Repository Contents
* `simulation_files/`: Native Ansys Lumerical 3D FDTD project files (`.fsp`).
* `scripts/`: Custom Lumerical script (`.lsf`) to extract simulation parameters and generate Native GDSII blueprints.
* `python_gds/`: Python script utilizing `gdspy` to algorithmically generate the mathematically perfect lithography mask for foundry fabrication.
* `exports/`: The final parameterized `.gds` file and 3D `.step` file.

---
*Simulated using Ansys Lumerical FDTD. Designed for foundry-compatible standard lithography processes.*
