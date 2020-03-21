# conda
use conda to manage development environment, benefits:

1. easily re-create development environment
2. easily switch between projects and commits
	git checkout xxx
	conda env update -f environment.yml
	make
3. easier git bisect
	conda env update -f environment.yml
	make && flash
	test ...
	git bisect [good|bad]
4. distribute a build or test environment to others, even without internet and conda 

# install conda
$wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
$bash Miniconda3-latest-Linux-x86_64.sh

The final step in the installer is to make sure the $HOME/miniconda3/etc/profile.d/conda.sh script is loaded in the shell(.bashrc).

close and re-open the terminal to activate conda, then:
$conda config --set auto_activate_base false

# create the environment
$conda create -n stm32
$conda activate stm32

# config the environment
copy the environment.yml to project as template

# populate the environment
in your project root:
$conda env update -f environment.yml

# add new dependency
edit environment.yml, then:
$conda env update -f environment.yml

# create your own conda packages
$codna create -n build
$conda activate build
$conda install conda-build anaconda-client
$mkdir ~/tmp/arm-none-eabi-gcc && cd ~/tmp/arm-none-eabi-gcc
$touch meta.yaml:
package:
  name: gcc-arm-none-eabi
  version: 9.2019.q4.major

source:
  url: https://developer.arm.com/...
  sha256: 

build:
  string: "0"

test:
  commands:
    - arm-none-eabi-gcc --version

$touch build.sh:
#!/usr/bin/env bash
cp -R $SRC_DIR/\* $PREFIX

$conda build .

// test the build result
$conda deactivate
$conda create -n test
$conda activate test
$conda install -c $HOME/miniconda3/envs/build/conda-bld/ gcc-arm-none-eabi
$which arm-none-eabi-gcc  --> verify path
$arm-none-eabi-gcc --version  --> verify version

$anaconda upload ...

# uninstall conda
rm -rf $HOME/miniconda3
