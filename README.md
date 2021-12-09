# ACTS-samples

## How to Compile 

The easiest way to do this is using Docker image the Acts team provides. 


``` bash
docker pull ghcr.io/acts-project/ubuntu2004:master

docker run --volume=<source>:/acts:ro --interactive --tty <image> /bin/bash
```
Download the [source code](https://github.com/hrzhao76/acts/tree/Add_Truth_and_Reco_Writer) which is modified to output input tracks for vertexing.  

Inside a Docker container, below is the code to compile Acts. 

```bash 
cmake -B build -S /acts -DACTS_BUILD_EXAMPLES_PYTHIA8=ON
```

Otherwise you'll need to meet the prerequisites if you want to compile one a local machine, `a C++17 compiler`, `CMake`, `Boost`, `Eigen` and `Pythia8`.

## How to run Vertex Reconstruction 

The content of the build directory: 

```
|-- ActsConfig.cmake
|-- ActsConfigVersion.cmake
|-- CMakeCache.txt
|-- CMakeFiles
|-- CTestTestfile.cmake
|-- Core
|-- Examples
|-- Fatras
|-- Makefile
|-- Plugins
|-- Tests
|-- bin
|-- cmake_install.cmake
|-- lib
|-- thirdparty
`-- this_acts.sh
```

Navigate to the folder `<cmake_build>/bin`, create a script named `gen_script.sh` containing all the steps, do change the source_dir accordingly. 
``` bash
source_dir=<source>
pu=$1
n_events=$2
mkdir -p logs/log_pu$pu

./ActsExamplePythia8  --rnd-seed=42 --events=$n_events  --output-dir=data/gen/ttbar_mu$pu --output-root --output-csv --gen-cms-energy-gev=14000 --gen-hard-process=Top:qqbar2ttbar=on --gen-npileup=$pu -l 2 | tee logs/log_pu$pu/log.ActsExamplePythia8_pu$pu

./ActsExampleFatrasGeneric --rnd-seed=42 --input-dir=data/gen/ttbar_mu$pu   --output-dir=data/sim_generic/ttbar_mu$pu  --output-csv  --select-eta=-2.5:2.5 --select-pt-gev=0.4: --fatras-pmin-gev 0.4 --remove-neutral  --bf-constant-tesla=0:0:2 -l 2 | tee logs/log_pu$pu/log.ActsExampleFatrasGeneric_pu$pu

./ActsExampleTruthTracksGeneric --rnd-seed=42 --input-dir=data/sim_generic/ttbar_mu$pu --output-dir=data/reco_generic/ttbar_mu$pu --bf-constant-tesla=0:0:2 --digi-config-file $source_dir/Examples/Algorithms/Digitization/share/default-smearing-config-generic.json -l 2 | tee logs/log_pu$pu/log.ActsExampleTruthTracksGeneric_pu$pu


cp data/gen/ttbar_mu$pu/particles.root data/reco_generic/ttbar_mu$pu
./ActsExampleVertexFinderTrackReaderPerformanceWriter  --rnd-seed=42 --bf-constant-tesla=0:0:2 --input-dir=data/reco_generic/ttbar_mu$pu --output-dir=data/vertexing/ttbar_mu$pu -l 2 | tee logs/log_pu$pu/log.ActsExampleVertexFinderTrackReaderPerformanceWriter_pu$pu
```

Then run the code with 
```
./gen_script.sh <n_pu> <n_events>
```

Once you run this and want to re-run the vertexing, only the last command is needed. 

```bash
./ActsExampleVertexFinderTrackReaderPerformanceWriter  --rnd-seed=42 --bf-constant-tesla=0:0:2 --input-dir=data/reco_generic/ttbar_mu$pu --output-dir=data/vertexing/ttbar_mu$pu -l 2 | tee logs/log_pu$pu/log.ActsExampleVertexFinderTrackReaderPerformanceWriter_pu$pu
```