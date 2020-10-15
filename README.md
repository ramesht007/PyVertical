![om-logo](https://github.com/OpenMined/design-assets/blob/master/logos/OM/horizontal-primary-trans.png)

![Tests](https://github.com/OpenMined/PyVertical/workflows/Tests/badge.svg?branch=master)
![License](https://img.shields.io/github/license/OpenMined/PyVertical)
![OpenCollective](https://img.shields.io/opencollective/all/openmined)

# PyVertical

A project developing privacy-preserving, vertically-distributed learning.

- :link: Links vertically partitioned data
         without exposing membership
         using Private Set Intersection (PSI).
- :lock: Trains a model on vertically partitioned data
        using SplitNNs,
        so only data holders can access data.

Vertically-partitioned data is data
in which
fields relating to a single record
are distributed across multiple datasets.
For example,
multiple hospitals may have admissions data on the same individuals.
Vertically-partitioned data could be applied to solve vital problems,
but data holders can't combine their datasets
by simply comparing notes with other data holders
unless they want to break user privacy.
`PyVertical` uses [PSI](https://www.github.com/OpenMined/PSI)
to link datasets in a privacy-preserving way.
We train SplitNNs on the partitioned data
to ensure the data remains separate throughout the entire process.

See the [changelog](./CHANGELOG.md)
for information
on the current status of `PyVertical`.


## The Process

![PyVertical diagram](./images/diagram_white_background.png)

PyVertical process:
1. Create partitioned dataset
    - Simulate real-world partitioned dataset by splitting MNIST into a dataset of images and a dataset of labels
    - Give each data point (image + label) a unique ID
    - Randomly shuffle each dataset
    - Randomly remove some elements from each dataset
1. Link datasets using PSI
    - Use **PSI** to link indices in each dataset using unique IDs
    - Reorder datasets using linked indices
1. Train a split neural network
    - Hold both datasets in a dataloader
    - Send images to first part of split network
    - Send labels to second part of split network
    - Train the network

## Requirements
This project is written in Python.
The work is displayed in jupyter notebooks.

### Environment

To install the dependencies,
we recommend using [Conda]:
1. Clone this repository
1. In the command line, navigate to your local copy of the repository
1. Run `conda env create -f environment.yml`
    - This creates an environment `pyvertical-dev`
    - Comes with most dependencies you will need
1. Activate the environment with `conda activate pyvertical-dev`
1. Run `pip install syft[udacity]`
1. Run `conda install notebook`

N.b. Installing the dependencies takes several steps to circumvent versioning incompatibility between
`syft` and `jupyter`.
In the future,
all packages will be moved into the `environment.yml`.

### PSI

In order to use [PSI](https://github.com/OpenMined/PSI) with PyVertical,
you need to install [bazel](https://www.bazel.build/) to build the necessary Python bindings for the C++ core.
After you have installed bazel, run the build script with `.github/workflows/scripts/build-psi.sh`.

This should generate a `_psi_bindings.so` file
and place it in `src/psi/`.

### Docker

You can instead opt to use Docker.

To run:

1. Build the image with `docker build -t pyvertical:latest .`
1. Launch a container with `docker run -it -p 8888:8888 pyvertical:latest`
  - Defaults to launching jupyter lab

## Usage

Check out
[`examples/PyVertical Example.ipynb`](examples/PyVertical%20Example.ipynb)
to see `PyVertical` in action.

## Goals

- [X] MVP
    - Simple example on MNIST dataset
    - One data holder has images, the other has labels
- [ ] Extension demonstration
    - Apply process to electronic health records (EHR) dataset
    - Dual-headed SplitNN: input data is split amongst several data holders
- [ ] Integrate with [`syft`](https://www.github.com/OpenMined/PySyft)

## Contributing
Pull requests are welcome.
For major changes,
please open an issue first to discuss what you would like to change.

Read the OpenMined
[contributing guidelines](https://github.com/OpenMined/.github/blob/master/CONTRIBUTING.md)
and [styleguide](https://github.com/OpenMined/.github/blob/master/STYLEGUIDE.md)
for more information.

## Contributors
|  [![TTitcombe](https://github.com/TTitcombe.png?size=150)][ttitcombe] | [![Pavlos-P](https://github.com/pavlos-p.png?size=150)][pavlos-p]  | [![H4ll](https://github.com/h4ll.png?size=150)][h4ll] | [![rsandmann](https://github.com/rsandmann.png?size=150)][rsandmann]
| :--:|:--: |:--:|:--:|
|  [TTitcombe] | [Pavlos-p]  | [H4LL] | [rsandmann]

## Testing
We use [`pytest`][pytest] to test the source code.
To run the tests manually:
1. In the command line, navigate to the root of this repository
1. Run `python -m pytest`

CI also checks the code conforms to [`flake8`][flake8] standards
and [`black`][black] formatting

## License
[Apache License 2.0](https://choosealicense.com/licenses/apache-2.0/)

[black]: https://black.readthedocs.io/en/stable/
[conda]: https://docs.conda.io/en/latest/
[flake8]: https://flake8.pycqa.org/en/latest/index.html#quickstart
[pytest]: https://docs.pytest.org/en/latest/contents.html

[ttitcombe]: https://github.com/ttitcombe
[pavlos-p]: https://github.com/pavlos-p
[h4ll]: https://github.com/h4ll
[rsandmann]: https://github.com/rsandmann
