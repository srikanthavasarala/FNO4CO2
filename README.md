# FNO4CO2

This repository contains the implementation of learned coupled inversion framework and the numerical experiments in [Learned coupled inversion for carbon sequestration monitoring and forecasting with Fourier neural operators](https://arxiv.org/abs/2203.14396), accepted by the International Meeting for Applied Geoscience & Energy 2022.

The aforementioned framework entails a re-implementation of Fourier neural operators from [Fourier Neural Operator for Parameter Partial Differential Equations](https://arxiv.org/abs/2010.08895) authored by Zongyi Li et al. The [original repository](https://github.com/zongyi-li/fourier_neural_operator) is in python.

This code is based on the Julia Language and the package [DrWatson](https://juliadynamics.github.io/DrWatson.jl/stable/) to make a reproducible scientific project named
> FNO4CO2

To (locally) reproduce this project, do the following:

1. Download this code base. Notice that raw data are typically not included in the
   git-history and may need to be downloaded independently.
2. Download [python](https://www.python.org/) and [Julia](https://julialang.org/). The numerical experiments are reproducible by python 3.7 and Julia 1.7.
3. Install [Devito](https://www.devitoproject.org/), a python package used for wave simulation.
4. Open a Julia console and do:
   ```
   julia> using Pkg
   julia> Pkg.activate("path/to/this/project")
   julia> Pkg.instantiate()
   ```

This will install all necessary Julia packages for you to be able to run the scripts and
everything should work out of the box.

## Examples

The repository currently includes several scripts.

`gen_perm.jl` generates random permeability samples and `gen_conc.jl` generates time-varying CO2 concentration for each of them using the numerical simulator from [FwiFlow.jl](https://github.com/lidongzh/FwiFlow.jl). To save you some time to reproduce our examples, we've also provided the dataset through a dropbox link -- when you run the network training script, the dataset will be downloaded automatically.

`fourier_3d.jl` trains a 3D FNO which maps the permeability to time-varying CO2 concentration governed by two-phase flow equations, with the dataset generated by `gen_perm.jl` and `gen_conc.jl`. You can train the FNO on GPU if available by setting the env variable ``export FNO4CO2GPU=1`` in your environment (e.g. in `~/.zshrc` on my mac, or just do `FNO4CO2GPU=1 julia`). The trained 3D network for the two-phase flow example is provided in the repository under `data/3D-FNO`.

`fourier_3d_grad.jl` script shows how to conduct a learned inversion to estimate the permeability (input of FNO) from the CO2 concentration snapshots. It uses gradient descent with back-tracking line search to iteratively invert the FNO.

`learned_coupled_inversion.jl` script shows how to conduct a learned coupled inversion, i.e. we invert for the permeability from time-lapse seismic datasets. The process involves inverting multiple physics as shown in [Coupled Time-Lapse Full-Waveform Inversion for Subsurface Flow Problems Using Intrusive Automatic Differentiation](https://agupubs.onlinelibrary.wiley.com/doi/abs/10.1029/2019WR027032), while it uses a pre-trained FNO as a surrogate for the fluid-flow solver.

## Citation

If you find this software useful in your research, we would appreciate it if you cite:

```bibtex
@article{yin2022learned,
  title={Learned coupled inversion for carbon sequestration monitoring and forecasting with Fourier neural operators},
  author={Yin, Ziyi and Siahkoohi, Ali and Louboutin, Mathias and Herrmann, Felix J},
  journal={arXiv preprint arXiv:2203.14396},
  year={2022}
}
```

## Acknowledgements

We thank the developers from several software packages in the open-source software community, which we based our implementation on. The FNO is re-implemented following Zongyi Li et al's work in [https://github.com/zongyi-li/fourier_neural_operator](https://github.com/zongyi-li/fourier_neural_operator). The two-phase flow dataset is generated by [FwiFlow.jl](https://github.com/lidongzh/FwiFlow.jl). We use [Devito](https://www.devitoproject.org/) and [JUDI.jl](https://github.com/slimgroup/JUDI.jl) for wave simulations. We use [SetIntersectionProjection](https://github.com/slimgroup/SetIntersectionProjection.jl) for constrained optimization.

This research was carried out with the support of Georgia Research Alliance and partners of the ML4Seismic Center.

## Author

Ziyi (Francis) Yin, [ziyi.yin@gatech.edu](mailto:ziyi.yin@gatech.edu)