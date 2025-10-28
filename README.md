# MRI-to-sCT

In modern clinical workflows, particularly for radiotherapy planning of brain tumors,
Magnetic Resonance Imaging (MRI) and Computed Tomography (CT) scans serve
complementary roles. MRI offers superior soft-tissue contrast for accurate tumor
delineation, while CT is essential for radiation dose calculation. However, the
necessity of acquiring both scans imposes significant logistical burdens, including
increased costs, extended patient scan times, and exposure to ionizing radiation
from the CT scan. This project addresses these challenges by developing a deep
learning framework to generate synthetic CT (sCT) images directly from T-weighted
MRI scans, enabling a more efficient "MRI-only" clinical pathway.
Utilizing the SynthRad dataset of 180 preprocessed and aligned 3D paired brain
scans, this study aims to optimize the Pix2Pix architecture for high-fidelity, 2D
slice-level sCT generation. While Pix2Pix provides a strong baseline, its performance
is highly dependent on its training configuration. Therefore, this project
systematically evaluates two key modifications against an 81-epoch baseline to
enhance image quality and training efficiency:
1. Altering the Generator-to-Discriminator (G:D) training ratio.
2. Replacing the standard L1 reconstruction loss with a more robust
Charbonnier loss function.
The primary objective is to identify an optimal configuration that yields superior sCT
image quality in significantly fewer training epochs than the standard approach.
