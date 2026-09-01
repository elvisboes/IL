# IL
Force field parameters for ionic liquids

# Charge-transfer-informed force field for [EMIM][BF4] and [EMIM][EtSO4]

GROMACS-format force field parameters, topologies and equilibrated configurations
accompanying the article:

> E. S. Böes, C. L. Firme, J. de Andrade, H. Stassen,
> *Thermophysical properties of [EMIM][BF4] and [EMIM][EtSO4] mixtures with
> molecular solvents from a charge-transfer-informed force field*,
> **Journal of Chemical & Engineering Data** (2026).
> DOI: `10.1021/acs.jced.XXXXXXX`

---

## What this is

A classical, non-polarizable force field for two imidazolium ionic liquids, built
within the AMBER functional form. Atomic charges were obtained from RESP fits to
electrostatic potentials computed at the MP2/6-311G(d,p) level on ion-pair
conformers, Boltzmann-weighted over the conformational ensemble; Lennard-Jones
parameters were subsequently refined against experimental densities and
vaporization enthalpies of the pure liquids.

Because the charges are derived from ion pairs rather than isolated ions, the
resulting net ionic charges are **±0.785e** for [EMIM][BF4] and **±0.753e** for
[EMIM][EtSO4]. No empirical charge scaling is applied — the reduction emerges
from the quantum chemical calculation itself.

## Repository layout

```
.
├── EMIM_BF4/
│   ├── emim_bf4.top          # system topology
│   ├── emim.itp              # cation parameters
│   ├── bf4.itp               # anion parameters
│   ├── conf_equilibrated.gro # equilibrated box, 1000 ion pairs
│   └── mdp/                  # run-parameter files (em, nvt, npt, production)
├── EMIM_EtSO4/
│   ├── emim_etso4.top
│   ├── emim.itp
│   ├── etso4.itp
│   ├── conf_equilibrated.gro
│   └── mdp/
└── mixtures/                 # topologies for the acetonitrile and ethanol mixtures
```

## Simulation details

| | |
|---|---|
| Engine | GROMACS 2019.3 |
| System size | 1000 ion pairs |
| Equilibrium box length | 6.346 nm ([EMIM][BF4]), 6.819 nm ([EMIM][EtSO4]) |
| Electrostatics | Particle-mesh Ewald |
| 1–4 scaling | 0.83 (electrostatic), 0.5 (Lennard-Jones), as in AMBER |
| Combination rules | Lorentz–Berthelot |

## Quick start

```bash
cd EMIM_BF4
gmx grompp -f mdp/production.mdp -c conf_equilibrated.gro -p emim_bf4.top -o run.tpr
gmx mdrun -deffnm run
```

The supplied configurations are already equilibrated at 298.15 K and 1 bar; for
other state points, run the `nvt` and `npt` stages in `mdp/` first.

## Scope

These parameters are specific to the two ion pairs for which they were derived.
They are **not** intended to be transferred to other cations or anions: the
atomic charges reflect the electronic structure of each particular ion pair,
including the asymmetry of the [EMIM]+ ring. What is intended to be reusable is
the *protocol* — described in full in the article — which was designed to be
inexpensive enough to be reapplied to a new ionic liquid as a matter of routine.

## Citing

If you use these files, please cite the article above.

## License

Released under the [MIT License](LICENSE). The parameters may be used and
redistributed freely, with attribution.

## Contact

Elvis S. Böes — elvis.boes@ifb.edu.br
Instituto Federal de Brasília, Campus Gama, Brasília — DF, Brazil
ORCID: [0000-0002-6319-8929](https://orcid.org/0000-0002-6319-8929)

