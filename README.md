# AlgoLab 2024 @ ETHZ

## Structure

```bash
algo-lab-2024
├── cmake                 # .cmake files for build system
├── CMakeLists.txt
├── README.md
└── problems              # codes for the weekly problems 
    ├── pow-XX            
    └── week01-XX
```

## Getting Started

```bash
git clone git@github.com:youwuyou/algo-lab-2024.git
```

This will create a subdirectory `algo-lab-2024` containing codes and other data needed for the algorithmic problems.

Now we create a build directory and initialize the build system. By default, we compile the repository with tests, you could also switch it off by using the flag `-DBUILD_TESTS=OFF` if you just want to build the examples themselves without testing.

```bash
cd algo-lab-2024
mkdir build
cd build
cmake .. && make
```

Now let us run a demonstration code. For running this code, make sure you locate at `algo-lab-2024/build` directory.

```bash
cd problems/hello-world
./hello-world
```

## Testing

If you have compiled the repository with `-DBUILD_TESTS=ON`, you can run the tests using `make test`.