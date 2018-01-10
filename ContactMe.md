---
title: Contact Me
layout: default
---

# [Contact Me](#contact-me)
Email ID - `abhisheknaik22296 [at] gmail [dot] com`


# RAIL : Risk-Averse Imitation Learning #

Codebase for [RAIL : Risk Averse Imitation Learning](https://arxiv.org/abs/1707.06658), presented at the [NIPS 2017 DRL Symposium](https://sites.google.com/view/deeprl-symposium-nips2017).   

---

## Setting up

* Set up MuJoCo on your machine.
* Install Anaconda Python 2.7 and set it as the default python: 
```bash
$ export PATH="/home/username/anaconda3/bin:$PATH"
$ which python
/home/username/anaconda3/bin/python
```
Make changes in the `.bashrc` for permanent results.

* Install the required packages.
```bash
pip install mujoco-py
pip install theano
pip install gym
```
* Clone the OpenAI-imitation (GAIL) repository.
`git clone https://github.com/openai/imitation.git`

* Clone the RAIL bitbucket repository.
`git clone https://abhisheknaik96@bitbucket.org/intelpclfad/rail.git`

* Add the path to the RAIL repository to `$PYTHONPATH`
* Copy the `expert_policies` directory from GAIL to RAIL.
* Make a new directory called trajectories in the RAIL repository.
* Generate expert trajectories from an expert policy using `scripts/vis_mj.py`   
Example usage : 
```
username@machine:/path/to/RAIL$ python scripts/vis_mj.py expert_policies/modern/log_Hopper-v1_4.h5/snapshots/iter0000500 --count 25 --out trajectories/expert/hopper_25.h5
```

* Run `scripts/imitate_mj.py` with flags set for GAIL/RAIL
Example usage of RAIL : 
```
username@machine:/path/to/RAIL$ python scripts/imitate_mj.py --mode ga --data trajectories/expert/hopper_25.h5 --limit_trajs 25 --data_subsamp_freq 20 --env_name Hopper-v1 --log training_logs/Hopper/ga_501-iter_Hopper_CVaR_lambda_0.5.h5 --max_iter 501 --useCVaR --CVaR_Lambda_not_trainable --CVaR_Lambda_val_if_not_trainable 0.5`
```

* Evaluate using `scripts/evaluate.py`. It has various functions to calculate metrics like VaR, CVaR as well for plotting different types histograms.   
Example usage :
```
python scripts/evaluate.py --trajectories_GAIL trajectories/Hopper/hopper_GAIL_25.h5 --trajectories_CVaR trajectories/Hopper/hopper_RAIL_25.h5 --env_name Hopper --percentile 10 --expert_mean 3666.88 --expert_std 392.908 --trajectories_expert trajectories/Hopper/hopper_expert_25.h5
````

---

In case of any issues, feel free to contact the authors/developers :      
- Anirban Santara (nrbnsntr@gmail.com)
- Abhishek Naik (abhisheknaik22296@gmail.com)
