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
resulting net ionic charges are **±0.785 e** for [EMIM][BF4] and **±0.753 e** for
[EMIM][EtSO4]. No empirical charge scaling is applied — the reduction emerges
from the quantum chemical calculation itself.

## Repository layout

```
.
├── EMIM_BF4/
│   ├── EMIM_BF4_Neat_Liquid/
│   │   ├── EMIM.itp             # cation parameters (bonded + charges)
│   │   ├── EMIM_types.itp       # cation atom types and Lennard-Jones parameters
│   │   ├── BF4.itp              # anion parameters (bonded + charges)
│   │   ├── BF4_types.itp        # anion atom types and Lennard-Jones parameters
│   │   ├── EMIM_BF4.top         # system topology, 1000 ion pairs
│   │   ├── EMIM_BF4.gro         # equilibrated box, 298.15 K / 1 bar
│   │   ├── topol.top            # same topology, default GROMACS name
│   │   ├── conf.gro             # same configuration, default GROMACS name
│   │   └── grompp.mdp           # run parameters (production)
│   └── EMIM_BF4_Mixtures/       # binary mixtures with acetonitrile and
│                                # ethanol (files to be added)
└── EMIM_EtSO4/
    ├── EMIM_EtSO4_Neat_Liquid/
    │   ├── EMIM.itp
    │   ├── EMIM_types.itp
    │   ├── EtSO4.itp
    │   ├── EtSO4_types.itp
    │   ├── EMIM_EtSO4.top
    │   ├── EMIM_EtSO4.gro
    │   ├── EMIM_EtSO4.mdp
    │   ├── topol.top
    │   ├── conf.gro
    │   └── grompp.mdp
    └── EMIM_EtSO4_Mixtures/
```

Each ionic liquid directory is split into a `*_Neat_Liquid` folder, containing
everything needed to reproduce the pure-liquid simulations, and a `*_Mixtures`
folder for the corresponding binary systems with molecular solvents. Within the
neat-liquid folders, `topol.top` and `conf.gro` are copies of the named topology
and configuration files under the default GROMACS names, so that `gmx grompp` can
be called without arguments.

> **Note.** The mixture topologies and configurations are being added over the
> coming days. They introduce no new parameters: the ionic liquid parameters are
> exactly those in the `*_Neat_Liquid` folders, combined with standard published
> models for the molecular solvents.

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
cd EMIM_BF4/EMIM_BF4_Neat_Liquid
gmx grompp -f grompp.mdp -c EMIM_BF4.gro -p EMIM_BF4.top -o run.tpr
gmx mdrun -deffnm run
```

The supplied configurations are already equilibrated at 298.15 K and 1 bar. For
other state points, run an energy minimization followed by NVT and NPT
equilibration before production, adjusting the temperature and pressure coupling
settings in `grompp.mdp` accordingly.

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
