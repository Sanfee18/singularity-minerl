# singularity-minerl
A Singularity container for HPC headless GPU rendering, developed specially for MineRL executions. Inspiried by this [docker repo](https://github.com/jeasinema/egl-docker) by [Xiaojian Ma.](https://github.com/jeasinema)

## Definition file
The container has three libraries installed: _MineRL_, _Stable-baselines3_ (for RL algorithms implementations) and _Wandb_ (for monitoring purposes). Feel free to uninstall any library that you may not need, this are the three libraries that I've used to do my executions.

Also, make sure to change the `mkdir -p` at the end of the _%post_ section to match the folder that contains the python script you're going to run.

## Build
Since Singularity `build` command needs `sudo` to be run, if your are executing this on a HPC cluster, you probably don't have root permissions, therefore you will need to build the .sif file in other computer and then move it to your cluster (you can use `scp` command for it).

```bash
git clone https://github.com/Sanfee18/singularity-minerl && cd singularity-minerl
sudo singularity build <container name>.sif vglsingularity.def
```

## Run
The command to run your MineRL program should be:
```bash
singularity exec --fakeroot --nv -w --bind /path/to/code:/path/to/code <container name>.sif ./setupvgl.sh vglrun /opt/conda/envs/minerl/bin/python /path/to/code/<script name>.py
```
- `--fakeroot`: essential flag, since MineRL will need to write some files.
- `--nv`: nvidia flag to enable GPU capabilities. (some warnings will raise but ignore them)
- `-w`: enable writting inside the container. (.sif is read only)
- `--bind`: path to the folder where your python scripts are.
- If you are using Wandb, you will need to specify the Wandb API key for remote monitoring. This can be done adding the following flag to the above command: `--env WANDB_API_KEY=XXXXXXXXXXXXXXXXX`. You can check your key [here.](https://wandb.ai/authorize)

> A work around that can be done if the `--fakeroot` can't be enbled is to transform the .sif file to _sandbox mode_. This can be archived with the following command:
> `sudo singularity build --sandbox <sandbox name> <container name>.sif`
> This way, you will be able to use the `-w` flag to make the container writable.

Make sure to have the _setupvgl.sh_ inside the folder from where you are executing the program, so that the instruction `./setupvgl.sh` works, and the `vglrun` right after, before your python call.
