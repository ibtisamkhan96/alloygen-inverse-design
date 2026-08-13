# AlloyGEN: generative inverse design of alloys

A small, honest, **working** version of generative inverse design for alloys: instead of predicting
a property from a composition (forward), it **generates a composition for a target property**
(inverse). Built on **real data**, with honest evaluation of where it works and where it doesn't.

It follows the approach in the recent literature, a VAE that learns a latent space of alloys, then
generation in that space conditioned on a target property (e.g. [Abu-Mualla et al., 2026](https://advanced.onlinelibrary.wiley.com/doi/10.1002/aidi.202500069)).

## The pipeline

```
real alloys ->  surrogate   composition -> strength  (forward model, cross-validated)
            ->  CVAE         a conditional VAE that GENERATES alloys for a target strength
            ->  screen       generate for a high target, validate with the surrogate,
                             keep the novel, iron-dominant (plausible) candidates
```

## Results (all verified by running it)

- **Forward surrogate:** 5-fold CV **R² = 0.76** (random forest, composition -> yield strength).
- **The generator responds to the target:** ask for higher strength and the generated alloys get
  stronger (surrogate mean **1154 -> 1289 MPa** as the target rises from the 25th to 95th percentile).
- **The proposed alloys are chemically meaningful:** the top novel candidates (~**1830 MPa**) come
  out as **Fe-Ni-Co-Mo-Ti** compositions, the real **maraging-steel** family, discovered by the
  model just from being asked for strength.

## Honest limits (stated up front)

- A random-forest surrogate does not extrapolate, so generated alloys cluster below the extreme top
  of the range; the *trend* is right, the ceiling is the surrogate's.
- 312 alloys is small for generative modelling (the reference paper uses ~1,860).
- Yield strength is an accessible **stand-in** for the wear resistance a real programme targets; the
  method is identical, and real candidates would be validated by synthesis and testing.

## Data

**matbench_steels**: 312 real steels with experimentally measured yield strength, from the
[Matbench](https://matbench.materialsproject.org/) benchmark suite (Dunn et al., *npj Computational
Materials*, 2020), loaded via [matminer](https://hackingmaterials.lbl.gov/matminer/). Underlying
data curated from a Citrine Informatics steel dataset.

## Run it

```bash
pip install -r requirements.txt
```

Open `alloygen_inverse_design.ipynb` and run top to bottom (~1 minute on CPU). Every step is
commented and explained.

## About

Built by Ibtisam Ahmed Khan, a materials engineer working in data and AI.
[materialsdecoded.com](https://materialsdecoded.com) ·
[GitHub](https://github.com/ibtisamkhan96) ·
[LinkedIn](https://www.linkedin.com/in/ibtisam-ahmed-khan)
