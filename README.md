# Fragment Forwarding in Lossy Networks (IEEE Access)

[![Paper on IEEE Xplore][paper-badge]][paper-ieeexplore]
[![DOI][software-badge]][software-doi]

This repository contains code and documentation to reproduce experimental
results of the paper **"[Fragment Forwarding in Lossy Networks][paper-ieeexplore]"**
published in IEEE Access.

* Martine S. Lenders, Thomas C. Schmidt, Matthias Wählisch, "**Fragment
  Forwarding in Lossy Networks**", in *IEEE Access*, vol. 9, pp. 143969–143987, October 2021,
  doi: [10.1109/ACCESS.2021.3121557][paper-doi]

##### Abstract

> This paper evaluates four forwarding strategies for fragmented datagrams in the Internet of Things (IoT).
> We focus on classic end-to-end fragmentation, hop-wise reassembly, a minimal approach to direct forwarding of fragments, and direct forwarding utilizing selective fragment recovery.
> To fully analyze the potentials of selective fragment recovery, we include four common congestion control mechanisms.
> We compare all fragmentation strategies comprehensively in extensive experiments to assess reliability, end-to-end latency, and memory consumption on top of IEEE&nbsp;802.15.4 and its common CSMA/CA&nbsp;MAC&nbsp;implementation.
> Our key findings include three takeaways.
> First, direct fragment forwarding should be deployed with care since higher packet transmission rates on the link layer can significantly reduce reliability, which can even further increase end-to-end latency because of highly increased link layer retransmissions.
> Second, selective fragment recovery can mitigate the problems underneath.
> Third, congestion control for selective fragment recovery should be chosen such that small congestion windows grow together with fragment pacing.
> In case of fewer fragments per datagram, pacing is less of a concern but the congestion window is limited by an upper bound.

[paper-doi]: https://doi.org/10.1109/ACCESS.2021.3121557
[paper-ieeexplore]: https://ieeexplore.ieee.org/document/9580895
[paper-badge]: https://img.shields.io/badge/Paper-IEEE%20Xplore-green
[software-badge]: https://zenodo.org/badge/DOI/10.5281/zenodo.5575035.svg
[software-doi]: https://doi.org/10.5281/zenodo.5575035

## Repository structure

### [RIOT/][RIOT]
The explicit RIOT version is included as a sub-module in this repository
([RIOT]). It is based on the [2021.04 release][2021.04] of RIOT but also
contains all relevant changes to conduct the experiments. The PRs these changes
came from are documented within the git history and the history can be recreated
using the [`cherry-pick-prs.sh`](./cherry-pick-prs.sh) (merge conflicts might
need to be resolved by hand). For more information use

```sh
cd RIOT
git log
```

### [apps/](./apps)
The `apps` directory contains the RIOT applications required for the
experiments, one for the data [sink](./apps/sink) and one for the sources
[sources and forwarders](./apps/source). Please refer to their `README`s for
their usage.

### [scripts/](./scripts)
The `scripts` directory contains scripts for [measuring the testbed as
described in Section IV-A.1 of the paper](./scripts/testbed_measure), to
[conduct the experiments](./scripts/experiment_ctrl), and to plot the results of
both [Section IV. COMPARISON OF FRAGMENT FORWARDING METHODS](./scripts/plots-ff)
and [Section V. EVALUATION OF CONGESTION CONTROL WITH SFR](./scripts/plots-cc).
Please also refer to their respective `README`s for their usage.

To handle the rather specific dependencies of the scripts, we recommend using
[virtualenv]:

```sh
virtualenv -p python3 env
source env/bin/activate
```

[virtualenv]: https://virtualenv.pypa.io/en/latest/

### [results/](./results)
In their default configuration, the scripts will put their result files into the
`results` directory. There, we also  provided the
[NetworkX](./scripts/plots-ff#requirements) [edge
list file](./results/m3-57x9938589e.edgelist.gz) and the two graphical
representations (logical and geographic topology) of that network, for your
convenience.

Usage
-----
For detailed usage, have a look into all the code and its documentation.
The quickest way to start the experiments, given the [provided network for
Section IV. in results/](./results/m3-57x9938589e.edgelist.gz) is bookable in
the IoT-LAB and all requirements on the OS-side are fulfilled, see [scripts
README's](./scripts/experiment_ctrl/README.md), is to just run:

```sh
rm -rf env
virtualenv -p python3 env
source env/bin/activate
pip install -r ./scripts/experiment_ctrl/requirements.txt
# only one of the following two, the `descs.yaml` is newly generated for either
./scripts/experiment_ctrl/create_ff_descs.py    # for Section IV experiments
./scripts/experiment_ctrl/create_cc_descs.py    # for Section V experiments
./scripts/experiment_ctrl/setup_exp.sh
```

[RIOT]: https://github.com/anr-bmbf-pivot/RIOT/tree/ieee-access-2021
[2021.04]: https://github.com/RIOT-OS/RIOT/releases/tag/2021.04
