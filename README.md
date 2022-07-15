# KETI-CSD simulator
> The KETI-CSD simulator uses qemu to provide an environment as if using csd, helping to select offload targets for queries
> ![simulator](https://user-images.githubusercontent.com/58501215/179137256-c190a97c-3412-4f3f-85d6-77e691448876.png)
## Dependency
> + General
>   + Linux 5.5 or higher
>   + compiler with c++17 support
>   + clang 10 or higher
>   + cmake 3.18 or higher
>   + python 3.x
>   + mesonbuild < 0.60 (pip3 install meson==0.59)
>   + pyelftools (pip3 install pyelftools)
>   + ninja
>   + cunit
> + Code Coverage
>   + ctest
>   + lcov
>   + gcov
>   + gcovr
> + Continuous Integration
>   + valgrind
> + Python scripts
>   + virtualenv
## Build
> ```bash
> $ git clone https://github.com/KETI-OpenCSD/simulator.git
> $ cd simulator
> $ git submodule update --init
> $ mkdir build
> $ cmake -DENABLE_DOCUMENTATION=off ..
> $ cmake --build .
> # Do not use make -j $(nproc), CMake is not able to solve concurrent dependency chain
> $ cd build/qemu-csd
> $ source activate
> $ qemu-img create -f raw znsssd.img 16777216 # 34359738368
> # By default qemu will use 4 CPU cores and 8GB of memory
> $ ./qemu-start.sh
> # Wait for QEMU VM to fully boot... (might take some time)
> $ git bundle create deploy.git HEAD
> $ rsync -avz -e "ssh -p 7777" deploy.git arch@localhost:~/
> # Type password (arch)
> $ ssh arch@localhost -p 7777
> # Type password (arch)
> $ git clone deploy.git qemu-csd
> $ rm deploy.git
> $ cd qemu-csd
> $ git -c submodule."dependencies/qemu".update=none submodule update --init
> $ mkdir build
> $ cd build
> $ cmake -DENABLE_DOCUMENTATION=off -DIS_DEPLOYED=on ..
> $ # Do not use make -j $(nproc), CMake is not able to solve concurrent dependency chain
> $ cmake --build .
> ```
## Getting Start
> ```bash
> # In QEMU
> $ mkdir /mnt/simul
> $ cd simulator/build/dir
> $ ./fuse-entry -- -d -o max_read=2147483647 /mnt/simul
> $ # Start another terminal(HOST) 
> $ mkdir /mnt/simul
> $ sshfs -p 7777 root@localhost:/mnt/simul /mnt/simul
> ```
> After running the course above, you can use the /mnt/simul directory on the host server as a csd mounted directory.
> https://github.com/KETI-OpenCSD/simulator
