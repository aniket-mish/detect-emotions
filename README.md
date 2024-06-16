# Detect Emotions

I'm building an ML service that detects emotions. My motivation behind this project is to learn about distributed ways of training and serving ml models. I'm going to use [ray](https://www.ray.io) for distributing workloads across nodes. Ray is being used by top tech companies to train large language models. It abstracts a lot of complexity for us.

## Setup

Let's create a cluster of machines to scale the workloads effortlessly. This cluster has a head node that manages the cluster and several worker nodes that will execute workloads. We can then implement auto-scaling based on our application's computing needs.

I'm going to create our cluster by defining a computing configuration and an environment.

I'm using a macbook air for this project but you can use any os including cloud. I'm using `pyenv` to create the virtual environments and switch between python versions easily. To create a cluster on the cloud you'll need a yaml with all the configurations with a base image, env variables, etc.

```bash
pyenv install 3.10.11 # install 
pyenv global 3.10.11 # set default
```

Once the pyenv is installed, create a virtual environment to install the dependencies.

```bash
mkdir detect-emotions 
cd detect-emotions 
python3 -m venv venv # create virtual environment 
source venv/bin/activate
python3 -m pip install --upgrade pip setuptools wheel
```

### Compute

Now define the compute configuration in the `cluster_compute.yaml` that specify the hardware dependencies for workload execution. If you're using cloud computing platforms like aws, define configurations such as region, instance_type, min_workers, max_workers, etc. 

I'm doing this on my laptop. there's one cpu(head node) and some of the remaining cpus are worker nodes.

Create a github repo and clone it.

```bash
export GITHUB_USERNAME="aniket-mish"
git clone https://github.com/aniket-mish/detect-emotions.git . 
git remote set-url origin https://github.com/$GITHUB_USERNAME/detect-emotions.git 
git checkout -b dev 
export PYTHONPATH=$PYTHONPATH:$PWD
```

Next, install the necessary packages by `requirements.txt` file.

```bash
python3 -m pip install -r requirements.txt
```

Every library uses `pre-commit` that keeps your syntax's/jsons/yamls/credentials in check.

```bash
pre-commit install
pre-commit autoupdate
```

Start experimenting in a jupyter notebook.

```bash
jupyter lab notebooks/emotions.ipynb
```

To check if ray is installed properly.

```python
import ray

# initialize Ray
if ray.is_initialized():
	ray.shutdown()
ray.init()
```

You can view cluster resources.

```python
ray.cluster_resources()
```
