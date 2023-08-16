# singularity-minerl
A Singularity container for HPC headless GPU rendering, developed specially for MineRL executions. Inspiried by the [docker repo](https://github.com/jeasinema/egl-docker) by [Xiaojian Ma.](https://github.com/jeasinema)

## Build
Since Singularity `build` command needs `sudo` to be run, if your are executing this on a HPC cluster, you probably don't have root permissions, therefore you will need to build the .sif file in other computer and then move it (you can use _scp_ command) to your cluster.

```bash
git clone https://github.com/Sanfee18/singularity-minerl && cd singularity-minerl
sudo singularity build <container name>.sif vgldocker.def
```

