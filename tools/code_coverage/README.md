# Code Coverage Tool for Pytorch

## Overview

This tool is designed for calculating code coverage for Pytorch project.
It’s an integrated tool. You can use this tool to run and generate both file-level and line-level report for C++ and Python tests. It will also be the tool we use in *CircleCI* to generate report for each master commit.

### Simple
* *Simple command to run:*
    * `python oss_coverage.py  `
* *Argument `--clean` will do all the messy clean up things for you*

### But Powerful

* *Choose your own interested folder*:
    * Default folder will be good enough in most times
    * Flexible: you can specify one or more folder(s) that you are interested in
* *Run only the test you want:*
    * By default it will run all the c++ and python tests
    * Flexible: you can specify one or more test(s) that you want to run
* *Final report:*
    * File-Level: The coverage percentage for each file you are interested in
    * Line-Level: The coverage details for each line in each file you are interested in
* *More complex but flexible options:*
    * Use different stages like *--run, --export, --summary* to achieve more flexible functionality

## How to use
This part will introduce about the arguments you can use when run this tool. The arguments are powerful, giving you full flexibility to do different work.
We have two different compilers, `gcc` and `clang`, and this tool supports both. But it is recommended to use `gcc` because it's much faster and use less disk place. The examples will also be divided to two parts, for `gcc` and `clang`.

## Preparation
The first step is to [build *Pytorch* from source](https://github.com/pytorch/pytorch#from-source) with `CODE_COVERAGE` option `ON`. Besides, you may also want to set `BUILD_TEST` option `ON` to get the test binaries.
See: [how to adjust build options](https://github.com/pytorch/pytorch#adjust-build-options-optional) for reference. Following is one way to adjust build option:
```
# in build/ folder (all build artifacts must in `build/` folder)
cmake .. -DCODE_COVERAGE=ON -DBUILD_TEST=ON
```


## Examples
The default setting is for `gcc`. If you are using `clang`, the first step is to set some environment value if needed:
```bash
# set compiler type, the default is "GCC"
export COMPILER_TYPE="CLANG"
# set llvm path, by default is /usr/local/opt/llvm/bin
export LLVM_TOOL_PATH=...
```

Great, you are ready to run the code coverage tool for the first time! Start from the simple command:
```
python oss_coverage.py --run-only=atest
```
This command will run `atest` binary in `build/bin/` folder and generate reoports over the entire *Pytorch* folder. But you may only be interested in the `aten` folder, in this case, try:
```
python oss_coverage.py --run-only=atest --interested-only=aten
```
In *Pytorch*, `c++` tests located in `build/bin/` and `python` tests located in `test/`. If you want to run `python` test, try:
```
python oss_coverage.py --run-only=test_complex.py
```

You may also want to specify more than one test or interested folder, in this case, try:
```
python oss_coverage.py --run-only=atest c10_logging_test --interested-only aten/src/Aten c10/core
```
That it is! With these two simple options, you can customize many different functionality according to your need.
By default, the tool will run all tests in `build/bin` folder (by running all executable binaries in it) and `test/` folder (by running `run_test.py`), and then collect coverage over the entire *Pytorch* folder. If this is what you want, try:
```bash
python oss_coverage.py
```

### For more complex arguments and functionality
*To Be Done*

## Reference

For `gcc`
* See about how to invoke `gcov`, read [Invoking gcov](https://gcc.gnu.org/onlinedocs/gcc/Invoking-Gcov.html#Invoking-Gcov) will be helpful

For `clang`
* If you are not familiar with the procedure of generating code coverage report by using `clang`, read [Source-based Code Coverage](https://clang.llvm.org/docs/SourceBasedCodeCoverage.html) will be helpful.
