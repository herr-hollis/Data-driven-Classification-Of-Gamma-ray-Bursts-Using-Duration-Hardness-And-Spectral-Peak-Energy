# Data-Driven Classification of Gamma-Ray Bursts Using Duration, Hardness and Spectral Peak Energy

**Hollis Francis<sup>1</sup>, Kiren Emmanuel Anbalagan<sup>1</sup>**
<sup>1</sup> Department of Physics, Faculty of Science, Universiti Teknologi Malaysia, Johor Bahru, Malaysia

Poster presentation, **IAU Early Career Researchers Working Group (ECRWG) Conference 2026**,
*Synergies between Observational and Theoretical Astronomy* (virtual, 17–19 November 2026).

---

## Abstract

Gamma-ray bursts (GRBs) are the most luminous explosions in the Universe. Their durations are bimodal, conventionally split at $T_{90} \approx 2$ s: short GRBs are associated with compact-object mergers and long GRBs with the core collapse of massive stars. Recent events challenge this dichotomy. GRB 211211A was long yet accompanied by a kilonova, while GRB 200826A was short yet accompanied by a supernova. Whether a third, intermediate class exists also remains debated.

We classify 3,759 bursts from the *Fermi* Gamma-ray Burst Monitor (GBM) catalogue **without labels**, using three prompt-emission observables: duration ($T_{90}$), spectral hardness ratio (HR) and spectral peak energy ($E_{\rm peak}$). We apply Gaussian mixture models (GMM) and density-based clustering (HDBSCAN, both directly and on a UMAP embedding). The number of classes is selected with the Bayesian information criterion (BIC), a parametric bootstrap likelihood-ratio test and seed-stability checks. The clusters are then validated against bursts with independently established progenitors.

Two dominant classes are recovered. They agree with the 2 s boundary for **97.5%** of bursts, and we find **no robust evidence for a third class**. The additional structure resides within the long population, which divides by spectral hardness. Including $E_{\rm peak}$ correctly reclassifies the collapsar GRB 200826A. However, merger-origin long bursts such as GRB 211211A remain indistinguishable from ordinary long GRBs in this feature space.

---

## 1. Data

| Item | Description |
|---|---|
| Source | *Fermi* GBM Burst Catalog (`fermigbrst`), NASA HEASARC |
| Raw sample | 4,415 GRBs |
| Sample A | 3,759 bursts with valid $T_{90}$ and HR |
| Sample B | 2,933 bursts with valid $T_{90}$, HR and $E_{\rm peak}$ |

$T_{90}$ is the interval containing 5–95% of the burst fluence (50–300 keV). Spectral quantities are taken from the best-fitting time-integrated (fluence) model: Band, Comptonised or power law.

## 2. Method

**Features.** The hardness ratio is defined as the ratio of energy flux in the 50–300 keV and 10–50 keV bands, integrated over the best-fit photon spectrum $N(E)$:

$$
\mathrm{HR}=\frac{\int_{50}^{300}E\,N(E)\,dE}{\int_{10}^{50}E\,N(E)\,dE}
\tag{1}
$$

$E_{\rm peak}$ is retained only for Band or Comptonised fits with $10 < E_{\rm peak} < 10^4$ keV and relative uncertainty below 100%. All features are log-transformed and standardised to zero mean and unit variance.

**Gaussian mixture model.** The standardised feature vector $\mathbf z$ is modelled as

```math
p(\mathbf z)=\sum_{k=1}^{K}\pi_k\,\mathcal N(\mathbf z\mid\boldsymbol\mu_k,\boldsymbol\Sigma_k)\qquad(2)
```
 
with full covariance matrices, fitted by expectation–maximisation.
 
**Model selection.** The number of components $K$ is chosen by minimising
 
```math
\mathrm{BIC}=-2\ln\hat L+q\ln n\qquad(3)
```

where $\hat L$ is the maximised likelihood, $q$ the number of free parameters and $n$ the sample size. Because BIC can favour extra components when clusters are non-Gaussian, the $K=2$ versus $K=3$ comparison is also tested with a parametric bootstrap of the likelihood-ratio statistic $\Lambda = 2(\ln\hat L_3 - \ln\hat L_2)$.

**Density clustering.** HDBSCAN, which makes no Gaussian assumption, is applied to the standardised features and to a two-dimensional UMAP embedding. Robustness is assessed across 10 independent UMAP random seeds.

**Validation.** Cluster assignments are compared with five bursts whose progenitors are known from kilonova or supernova associations.

## 3. Results

| Quantity | Value |
|---|---|
| Short component (1D GMM) | median $T_{90}$ = 0.83 s, 20% of bursts |
| Long component (1D GMM) | median $T_{90}$ = 25.9 s, 80% of bursts |
| ΔBIC ($K=3$ vs $K=2$, duration only) | +14.5 (two classes preferred) |
| Bootstrap LRT, $K=2$ vs $K=3$ (duration only) | $p \approx 0.005$: extra structure, but no distinct third class |
| Agreement of 2-class GMM ($T_{90}$, HR) with 2 s cut | **97.5%** |
| Hardness of short vs long bursts (median HR) | short bursts ≈ **2.5×** harder |
| 3-class GMM ($T_{90}$, HR, $E_{\rm peak}$) | long bursts split into soft (104 keV) and hard (235 keV) groups |
| UMAP + HDBSCAN | **two clusters** in all 10 random seeds |

**Known progenitors (Table 1).** "GMM" denotes the 2-class GMM on ($T_{90}$, HR); "UMAP" denotes UMAP + HDBSCAN on ($T_{90}$, HR, $E_{\rm peak}$).

| GRB | Origin | GMM | UMAP |
|---|---|---|---|
| 090510 | merger | short ✓ | short ✓ |
| 170817A | merger | short ✓ | – |
| 200826A | collapsar | short ✗ | long ✓ |
| 211211A | merger | long ✗ | long ✗ |
| 130427A | collapsar | long ✓ | long ✓ |

GRB 170817A has no usable $E_{\rm peak}$ and is therefore absent from sample B.

| Duration distribution | How many classes? |
|---|---|
| ![Duration distribution](Duration%20distribution.png) | ![How many classes](How%20many%20classes.png) |
| **Hardness vs duration** | **Peak energy vs duration** |
| ![Hardness vs duration](Hardness%20vs%20duration.png) | ![Peak energy vs duration](Peak%20energy%20vs%20duration.png) |

## 4. Conclusions

Unsupervised learning on *Fermi* GBM prompt-emission data recovers the classical short/long dichotomy with high fidelity. It does not support a statistically robust intermediate class: the additional structure reflects spectral diversity within long bursts. Peak energy carries progenitor information beyond duration and hardness, as shown by the correct reclassification of GRB 200826A. Merger-driven long bursts, however, require information beyond these three observables.

## 5. Repository contents

| File / folder | Description |
|---|---|
| `Data_Driven_Classification_of_Gamma_Ray_Bur…ipynb` | Complete analysis notebook (Google Colab); reproduces all numbers and figures |
| `browse_results.fits` | *Fermi* GBM Burst Catalog extract from HEASARC (input data) |
| `Gamma Ray Burst cleaned data.xlsx` | Cleaned feature table with cluster labels |
| `grb_outputs/` | Notebook outputs: figures, CSV/Excel data and `results.json` |
| `Analysed and Plotted Graph.opju` | OriginPro project used for the poster figures |
| `*.png` | Poster figures (Figures 1–4) |
| `Hollis Francis_Poster_IAU ECRWG Conference 2…` | Final conference poster |

**Dependencies:** `numpy`, `pandas`, `scipy`, `scikit-learn`, `astropy`, `hdbscan`, `umap-learn`, `matplotlib`, `openpyxl`. 

## References

[1] C. Kouveliotou *et al.*, "Identification of two classes of gamma-ray bursts," *Astrophys. J.*, vol. 413, pp. L101–L104, 1993, doi: [10.1086/186969](https://doi.org/10.1086/186969).
[2] T. Ahumada *et al.*, "Discovery and confirmation of the shortest gamma-ray burst from a collapsar," *Nat. Astron.*, vol. 5, pp. 917–927, 2021, doi: [10.1038/s41550-021-01428-7](https://doi.org/10.1038/s41550-021-01428-7).
[3] J. C. Rastinejad *et al.*, "A kilonova following a long-duration gamma-ray burst at 350 Mpc," *Nature*, vol. 612, pp. 223–227, 2022, doi: [10.1038/s41586-022-05390-w](https://doi.org/10.1038/s41586-022-05390-w).
[4] C. L. Steinhardt, W. J. Mann, V. Rusakov, and C. K. Jespersen, "Classification of BATSE, Swift, and Fermi gamma-ray bursts from prompt emission alone," *Astrophys. J.*, vol. 945, Art. no. 67, 2023, doi: [10.3847/1538-4357/acb999](https://doi.org/10.3847/1538-4357/acb999).
[5] S. Poolakkil *et al.*, "The Fermi-GBM gamma-ray burst spectral catalog: 10 yr of data," *Astrophys. J.*, vol. 913, Art. no. 60, 2021, doi: [10.3847/1538-4357/abf24d](https://doi.org/10.3847/1538-4357/abf24d).

## Acknowledgements

This research uses data from the *Fermi* GBM team, obtained through the High Energy Astrophysics Science Archive Research Center (HEASARC) at NASA Goddard Space Flight Center. Analysis used Astropy, scikit-learn, hdbscan and umap-learn.

## Contact

Hollis Francis: hollis@graduate.utm.my
