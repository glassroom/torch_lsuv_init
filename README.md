# torch_lsuv_init

PyTorch implementation of layer sequential unit variance (LSUV), as described in ["All you need is a good init" by Mishkin, D. et al (2015)](https://arxiv.org/abs/1511.06422):

![Algorithm as published](algorithm_as_published.png)

## Why?

We were unable to find a short, simple implementation online.

Excluding comments, this one is under 30 lines of code.

It's meant to be easy to understand/modify.

## Installation

Copy the file `torch_lsuv_init.py` to your application directory.

## Requirements

The only requirement is a working installation of [PyTorch](https://pytorch.org/) on Python 3.

Note: Tested only with PyTorch versions 1.0 and 1.1, on Ubuntu Linux 18.04 and Python 3.6+.

## Usage

```python
from torch_lsuv_init import LSUV_

LSUV_(model, data)
```

Where: `model` is the model to be initialized and `data` is a sample batch of input data drawn from the training dataset.

The complete list of parameters and default values are:

```python
LSUV_(model, data, apply_only_to=['Conv', 'Linear', 'Bilinear'],
      tol=0.1, max_iters=10, do_ortho_init=True, logging_FN=print):
```

Where:

`model` is a torch.nn.Module object containing the model on which to apply LSUV.

`data` is a sample batch of input data drawn from training dataset.

`apply_only_to` is a list of strings indicating target children modules. For example, \['Conv'\] results in LSUV being applied to children of type containing the substring 'Conv', which includes all convolutional layers defined in the PyTorch API ('Conv1d', 'Conv2d', ..., 'ConvTranspose3d').

`tol` is a positive number lower than 1.0, below which differences between actual and unit standard deviation are acceptable.

`max_iters` is the number of times to try scaling standard deviation of each children module's output activations.

`do_ortho_init` is a boolean indicating whether to apply orthogonal init to parameters of at least 2 dimensions (or zero init if dimension is lower than 2, as in bias parameters).

`logging_FN` is a function for outputting progress information.

## Notes

This implementation uses PyTorch's built-in orthogonal init, which at the time of writing (mid-2019) relies on QR decomposition, instead of singular value decomposition (SVD) as in other implementations.

## Licence

torch_lsuv_init is released by GlassRoom Software LLC under the [MIT License](LICENSE).
