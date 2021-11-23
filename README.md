# Choosing an AWS EC2 Instance for OpenFOAM - Test Cases

These cases are supporting material for an article that looked at [how to choose an EC2 instance to run OpenFOAM](https://www.cfdengine.com/blog/how-to-choose-an-ec2-instance-for-openfoam/) (computational fluid dynamics) simulations.

The article summarises which instances were tested and how it was done, along with a rough guide to choosing an EC2 instance for your own OpenFOAM simulations.

This repository contains the test cases used to prepare that article. It contains 6 versions of the [windAroundBuildings tutorial from OpenFOAM&reg; v6](https://github.com/OpenFOAM/OpenFOAM-6/tree/master/tutorials/incompressible/simpleFoam/windAroundBuildings). Each with a different `blockMesh` resolution to produce a different model size.

| Test Case | Model Size            | blockMesh         |
|:--        |:--                    |:--                |
| x1 (STD)  | ~185K Cells           | 25 - 20 - 10      |
| x2        | ~1.1 Million Cells    | 50 - 40 - 20      |
| x3        | ~3.3 Million Cells    | 75 - 60 -30       |
| x4        | ~7.0 Million Cells    | 100 - 80 - 40     |
| x5        | ~13.3 Million Cells   | 125 - 100 - 50    |
| x6        | ~22.5 Million Cells   | 150 - 120 - 60    |

Grabbing these cases & running them locally will give you some additional data points to compare the performance of your local machines against some current EC2 instances. It should also give you an idea of how your usual simulations might perform on AWS.

## Prerequisites
The test cases in the article were run in OpenFOAM v6 on Ubuntu 16.04 - they will (most likely) run in earlier versions of OpenFOAM, but may require modification. They *should* work out-of-the-box in v6.

## Instructions for use
1. Download the cases to your test machine with git **OR** download & extract the zip archive
	- `git clone https://github.com/CFDEngine/of-on-aws-tests.git`
	- `wget -O of-on-aws-tests.zip https://github.com/CFDEngine/of-on-aws-tests/archive/master.zip`
2. Choose which test case is closest to your usual model size & change into that directory
3. Change the `numSubDomains` in `decomposeParDict` to suit the number of physical cores you have available
4. Run & time the case using the `Allrun` file - on Ubuntu this can be done with the following command
	- `/usr/bin/time -o log.time ./Allrun`

The `time` command will save it's output into a `log.time` file &ndash; the contents of which should look something like this:

```
61.85user 0.60system 1:03.92elapsed 97%CPU (0avgtext+0avgdata 362380maxresident)k
10416inputs+0outputs (4major+103628minor)pagefaults 0swaps
```

In this example `1:03.92elapsed` is the interesting bit, showing  the time taken was 1m 3.92s.

See [the full article](https://www.cfdengine.com/blog/how-to-choose-an-ec2-instance-for-openfoam/) for links to the complete data set of equivalent timings on AWS.
