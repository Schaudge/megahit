MEGAHIT
=========

MEGAHIT is a single node assembler for large and complex metagenomics NGS reads, such as soil. It makes use of succinct *de Bruijn* graph (SdBG) to achieve low memory assembly. The graph construction algorithm can self-adjust to use all available or moderate memory, and can be accelerated if a CUDA-enable GPU is provided. The GPU-accelerated version of MEGAHIT has been tested on NVIDIA GTX680 (4G memory) and Tesla K40c (12G memory) with CUDA 5.5, 6.0 and 6.5.

[![Build Status](https://travis-ci.org/voutcn/megahit.svg)](https://travis-ci.org/voutcn/megahit)
[![Build Status](https://drone.io/github.com/voutcn/megahit/status.png)](https://drone.io/github.com/voutcn/megahit/latest)

Getting Started
----------------

### Dependency
MEGAHIT is suitable for 64-bit Linux and MAC OS X. It requires [zlib](http://www.zlib.net/), python 2.6 or greater and G++ 4.4 or greater (with `-std=c++0x` and [OpenMP](http://openmp.org) support). The GPU version further requires [CUDA](https://developer.nvidia.com/cuda-toolkit) 5.5 or greater.

### Compiling from Source Codes
```
git clone https://github.com/voutcn/megahit.git
cd megahit
make
```

Notably, for MAC OS X, the `g++` in the path is probably the sym-link of `clang`, which do not support OpenMP. Users should have the "real" G++ installed and use `make CXX=/PATH/TO/G++` to specify the compiler.

### Stable releases
Please refer to the [release page](https://github.com/voutcn/megahit/releases).

### Running MEGAHIT
If MEGAHIT is successfully compiled, it can be run by the following command:

```
./megahit [options] {-r <se.fq> | -1 <pe1.fq> -2 <pe2.fq> | --12 <pe12.fq> }
```

or type `make test` in MEGAHIT's source directory for a quick test and `./megahit -h` for usage message.

If you need install MEGAHIT to another directory after compilation, please copy `megahit`, `megahit_asm_core` and `megahit_sdbg_build` (and `megahit_sdbg_build_gpu`) to the destination.

### Using GPU Version
To use the GPU version, run `make use_gpu=1` to compile MEGAHIT, and run MEGAHIT with `--use-gpu`. GPU version has only been tested in Linux.

Assembly Tips
------------------------
###Choosing *k*
MEGAHIT uses multiple *k*-mer strategy. Minimum *k*, maximum *k* and the step for iteration can be set by options `--k-min`, `--k-max` and `--k-step` respectively. *k* must be odd numbers while the step must be an even number. 
* For ultra complex metagenomics data such as soil, a larger *k<sub>min</sub>*, say 27, is recommended to reduce the complexity of the *de Bruijn* graph. Quality trimming is also recommended
* For high-depth generic data, large `--k-min` (25 to 31) is recommended
* Smaller `--k-step`, say 10, is more friendly to low-coverage datasets

###Filtering (*k<sub>min</sub>*+1)-mer
(*k<sub>min</sub>*+1)-mer with multiplicity lower than *d* (default 2, specified by `--min-count` option) will be discarded. You should be cautious to set *d* less than 2, which will lead to a much larger and noisy graph. We recommend using the default value 2 for metagenomics assembly. If you want to use MEGAHIT to do generic assembly, please change this value according to the sequencing depth. (recommend `--min-count 3` for >40x).

###Mercy *k*-mer
This is specially designed for metagenomics assembly to recover low coverage sequence. For generic dataset >= 30x,  MEGAHIT may generate better results with `--no-mercy` option.

###*k*-min 1pass mode
This mode can be activated by option `--kmin-1pass`. It is more memory efficient for ultra low-depth dataset, such as soil metagenomics data.

FAQ
-----------------------
Please refer to [our wiki](https://github.com/voutcn/megahit/wiki).

Reporting Issues
-----------------------
If you have any questions or suggestions, please [report an issue](https://github.com/voutcn/megahit/issues) on Github.

Citing MEGAHIT
-----------------------
* Li, D., Liu, C-M., Luo, R., Sadakane, K., and Lam, T-W., (2015) MEGAHIT: An ultra-fast single-node solution for large and complex metagenomics assembly via succinct de Bruijn graph. *Bioinformatics*, doi: 10.1093/bioinformatics/btv033 [PMID: [25609793](http://www.ncbi.nlm.nih.gov/pubmed/25609793)].

License
-----------------------
MEGAHIT is released under GPLv3. Several third-party codes are used, including:

* [CUB](https://github.com/NVlabs/cub) under "New BSD" license
* [klib](https://github.com/attractivechaos/klib) under MIT license
* [IDBA package](http://i.cs.hku.hk/~alse/hkubrg/projects/idba/) under GPLv2
* [CityHash](https://code.google.com/p/cityhash/) under MIT license