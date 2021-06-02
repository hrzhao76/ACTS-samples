# ACTS-samples

To test if the associated truth particles match the ordering of the input fitted tracks, 

compile this branch first, https://github.com/hrzhao76/acts/tree/v8.3_add_TruthWriter   
then run

```bash
./ActsExampleVertexFinderTrackReaderPerformanceWriter -n 1 --rnd-seed=42 --bf-constant-tesla=0:0:2 --input-dir=data/reco_generic/ttbar_mu10 --output-dir=data/vertexing/ttbar_mu10 -l 1 | tee log.ActsExampleVertexFinderTrackReaderPerformanceWriter_custom
```

The log of this test can be found [log.ActsExampleVertexFinderTrackReaderPerformanceWriter_custom](./log.ActsExampleVertexFinderTrackReaderPerformanceWriter_custom) (with "custom" as suffix)







Samples are provides in this branch. See the folder `data`. 

Log files are also provided. See `log.*` .

Sample Generation Commands: 

```bash
./ActsExamplePythia8  --rnd-seed=42 --events=3  --output-dir=data/gen/ttbar_mu10 --output-root --output-csv --gen-cms-energy-gev=14000 --gen-hard-process=Top:qqbar2ttbar=on --gen-npileup=10 -l 1 | tee log.ActsExamplePythia8
```

```bash
./ActsExampleFatrasGeneric --rnd-seed=42 --input-dir=data/gen/ttbar_mu10   --output-dir=data/sim_generic/ttbar_mu10  --output-csv  --select-eta=-2.5:2.5 --select-pt-gev=0.4: --fatras-pmin-gev 0.4 --remove-neutral  --bf-constant-tesla=0:0:2 -l 1 | tee log.ActsExampleFatrasGeneric
```

```bash
./ActsExampleTruthTracksGeneric --rnd-seed=42 --input-dir=data/sim_generic/ttbar_mu10 --output-dir=data/reco_generic/ttbar_mu10 --bf-constant-tesla=0:0:2 --digi-config-file ~/sw/source/acts/Examples/Algorithms/Digitization/share/default-smearing-config-generic.json -l 1 | tee log.ActsExampleTruthTracksGeneric
```

```bash
cp data/gen/ttbar_mu10/particles.root data/reco_generic/ttbar_mu10
./ActsExampleVertexFinderTrackReaderPerformanceWriter  --rnd-seed=42 --bf-constant-tesla=0:0:2 --input-dir=data/reco_generic/ttbar_mu10 --output-dir=data/vertexing/ttbar_mu10 -l 1 | tee log.ActsExampleVertexFinderTrackReaderPerformanceWriter
```

