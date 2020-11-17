# Universe workshop: Grafting monorepos
Keep monorepos manageable using grafting


## Exercise 2

We will use different analysis tools to identify wrong practices in a repository. To do it we will use the following commands:
- **git-sizer**
- **git-find-dirs-many-files**
- **git-find-lfs-extensions**
- **git-find-dirs-unwanted**
- **git-filter-repo**

Before starting any analysis, **pick one repository of your preference** that you would like to analyze

> :warning: Make sure during all this exercise you don't post any private information that should not be shared publicly.

Clone this repository as we have added all the tools into the it for making the workshop more convenient:
```bash
# Clone the repository
git clone git@github.com:githubuniverseworkshops/grafting-monorepos.git


#  or use the GitHub CLI
gh repo clone githubuniverseworkshops/grafting-monorepos
```

### Stats of repo size: [`git-sizer`](https://github.com/github/git-sizer)

1. [Download](https://github.com/github/git-sizer/releases/latest) the corresponding compiled version of `git-sizer`.
> Optionally you can install git-sizer using Homebrew if you are on Mac
2. Run the tool from the root of the repository to analyze:
```bash
/path/to/git-sizer --verbose
```

<details><summary>See the result of running `git-sizer`</summary>

``` bash
» ../git-sizer-1.3.0-darwin-amd64/git-sizer --verbose
Processing blobs: 2173019                        
Processing trees: 4613547                        
Processing commits: 967035                        
Matching commits to trees: 967035                        
Processing annotated tags: 671                        
Processing references: 676                        
| Name                         | Value     | Level of concern               |
| ---------------------------- | --------- | ------------------------------ |
| Overall repository size      |           |                                |
| * Commits                    |           |                                |
|   * Count                    |   967 k   | *                              |
|   * Total size               |   752 MiB | ***                            |
| * Trees                      |           |                                |
|   * Count                    |  4.61 M   | ***                            |
|   * Total size               |  12.8 GiB | ******                         |
|   * Total tree entries       |   373 M   | *******                        |
| * Blobs                      |           |                                |
|   * Count                    |  2.17 M   | *                              |
|   * Total size               |  77.6 GiB | ********                       |
| * Annotated tags             |           |                                |
|   * Count                    |   671     |                                |
| * References                 |           |                                |
|   * Count                    |   676     |                                |
|                              |           |                                |
| Biggest objects              |           |                                |
| * Commits                    |           |                                |
|   * Maximum size         [1] |  72.7 KiB | *                              |
|   * Maximum parents      [2] |    66     | ******                         |
| * Trees                      |           |                                |
|   * Maximum entries      [3] |  2.19 k   | **                             |
| * Blobs                      |           |                                |
|   * Maximum size         [4] |  13.5 MiB | *                              |
|                              |           |                                |
| History structure            |           |                                |
| * Maximum history depth      |   162 k   |                                |
| * Maximum tag depth      [5] |     1     |                                |
|                              |           |                                |
| Biggest checkouts            |           |                                |
| * Number of directories  [6] |  4.74 k   | **                             |
| * Maximum path depth     [7] |    13     | *                              |
| * Maximum path length    [8] |   134 B   | *                              |
| * Number of files        [9] |  70.7 k   | *                              |
| * Total size of files    [9] |   918 MiB |                                |
| * Number of symlinks    [10] |    40     |                                |
| * Number of submodules       |     0     |                                |

[1]  91cc53b0c78596a73fa708cceb7313e7168bb146
[2]  2cde51fbd0f310c8a2c5f977e665c0ac3945b46d
[3]  648d5c2e00672dfe3c217d25b8c73802744f3d3b (refs/heads/master:arch/arm/boot/dts)
[4]  29af5167cd0057fcfbfab150378322008d3d2667 (refs/heads/master:drivers/gpu/drm/amd/include/asic_reg/nbio/nbio_6_1_sh_mask.h)
[5]  5dc01c595e6c6ec9ccda4f6f69c131c0dd945f8c (refs/tags/v2.6.11)
[6]  47bb244d15374c342ffe8ede1fcf15b46f08827f (8e4c309f9f33b76c09daa02b796ef87918eee494^{tree})
[7]  78a269635e76ed927e17d7883f2d90313570fdbc (dae09011115133666e47c35673c0564b0a702db7^{tree})
[8]  b0da5ce619daec8138cf92dfcf00e7a51ce856a9 (d8763340d2cb6262fb86424315a1f92cabc0e23c^{tree})
[9]  5bf53b0c610a5abf0cd874d0687f4e731009fda4 (9c75b68b91ff010d8d4c703b93954f605e2ef516^{tree})
[10] f29a5ea76884ac37e1197bef1941f62fda3f7b99 (f5308d1b83eba20e69df5e0926ba7257c8dd9074^{tree})
../git-sizer-1.3.0-darwin-amd64/git-sizer --verbose  248.41s user 28.56s system 136% cpu 3:22.24 total
```
</details>

### Find files that should be in LFS: [`git-find-lfs-extensions`](https://github.com/githubuniverseworkshops/grafting-monorepos/blob/main/scripts/git-find-lfs-extensions)

1. Checkout the `grafting-monorepos` repository
2. Run the tool from the root of the repository to analyze:
```bash
../grafting-monorepos/scripts/git-find-lfs-extensions
```

<details><summary>See the results of running `git-find-lfs-extensions`</summary>

```bash
time ../grafting-monorepos/scripts/git-find-lfs-extensions

Type           Extension              LShare    LCount     Count      Size       Min       Max
-------        ---------              -------   -------   -------   -------   -------   -------
all            *                        0 %        73     70569       917         0        13
text           h                        0 %        62     21370       304         0        13
text           c                        0 %         7     29461       523         0         0
text           json                     1 %         2       332         7         0         0
text           s                        0 %         1      1290         8         0         0
text w/o ext   maintainers             50 %         1         2         0         0         0

Add to .gitattributes:

../grafting-monorepos/scripts/git-find-lfs-extensions  3.18s user 5.41s system 80% cpu 10.619 total
```
</details>

### Print directories with the number of files contained: [`git-find-dirs-many-files`](https://github.com/githubuniverseworkshops/grafting-monorepos/blob/main/scripts/git-find-dirs-many-files)

1. Checkout the `grafting-monorepos` repository
2. Run the tool from the root of the repository to analyze:
```bash
../grafting-monorepos/scripts/git-find-dirs-many-files
```

<details><summary>See the results of running `git-find-dirs-many-files`</summary>

```bash
time ../grafting-monorepos/scripts/git-find-dirs-many-files | head -n 20                                                                    droidpl@javiers-mbp
   70602 .
   28698 ./drivers
   15946 ./arch
    7486 ./Documentation
    5416 ./include
    5085 ./drivers/gpu
    4996 ./drivers/gpu/drm
    4922 ./drivers/net
    4569 ./tools
    4492 ./arch/arm
    4071 ./Documentation/devicetree
    4064 ./Documentation/devicetree/bindings
    2453 ./include/linux
    2438 ./drivers/media
    2366 ./drivers/net/ethernet
    2279 ./sound
    2214 ./arch/arm/boot
    2204 ./drivers/staging
    2196 ./tools/testing
    2184 ./arch/arm/boot/dts
../grafting-monorepos/scripts/git-find-dirs-many-files  12.07s user 45.92s system 135% cpu 42.640 total
head -n 20  0.00s user 0.00s system 0% cpu 42.630 total
```
</details>

### Find dirs that should not be committed: [`git-find-dirs-unwanted`](https://github.com/githubuniverseworkshops/grafting-monorepos/blob/main/scripts/git-find-dirs-unwanted)

1. Checkout the `grafting-monorepos` repository
2. Run the tool from the root of the repository to analyze:
```bash
../grafting-monorepos/scripts/git-find-dirs-unwanted | head -n 15            
```

<details><summary>See the results of running `git-find-dirs-unwanted`</summary>

```bash
» time ../grafting-monorepos/scripts/git-find-dirs-unwanted | head -n 15                                                                      droidpl@javiers-mbp
    4569 tools/
    4064 Documentation/devicetree/bindings/
    2088 tools/testing/selftests/
    1385 arch/x86/
    1268 tools/perf/
     632 tools/testing/selftests/bpf/
     448 lib/
     368 arch/x86/include/asm/
     327 tools/perf/util/
     326 Documentation/devicetree/bindings/sound/
     325 tools/testing/selftests/bpf/progs/
     313 tools/perf/pmu-events/
     283 include/dt-bindings/clock/
     269 Documentation/devicetree/bindings/clock/
     240 arch/x86/kernel/
../grafting-monorepos/scripts/git-find-dirs-unwanted  81.27s user 13.35s system 114% cpu 1:22.32 total
head -n 15  0.00s user 0.00s system 0% cpu 1:22.31 total
```
</details>

### Analyze the repository: `git-filter-repo --analyze`

1. Clone the [`git-filter-repo` tool](https://github.com/newren/git-filter-repo)
2. Execute the tool from the linux repository
```bash
../git-filter-repo/git-filter-repo --analyze
```

<details><summary>See the results of running `git filter-repo --analyze`</summary>

```bash
Processed 7754272 blob sizes
Processed 967008 commitswarning: inexact rename detection was skipped due to too many files.
warning: you may want to set your diff.renameLimit variable to at least 817 and retry the command.
Processed 967035 commits
Writing reports to .git/filter-repo/analysis...done.
../git-filter-repo/git-filter-repo --analyze  1104.40s user 45.86s system 105% cpu 18:11.40 total
```
</details>
