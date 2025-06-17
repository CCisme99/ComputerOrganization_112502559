# 1102-ComputerOrganization-FinalProject

## Questions

### Q1. GEM5 + NVMAIN BUILD-UP (40%)
Follow PPT

### Q2. Enable L3 last level cache in GEM5 + NVMAIN (15%)
1. modify
    - `GEM5/configs/common/Caches.py`
    - `GEM5/configs/common/Options.py`
    - `GEM5/src/cpu/BaseCPU.py`
    - `GEM5/src/mem/XBar.py`
    - `GEM5/configs/common/CacheConfig.py`
<!-- ![](https://i.imgur.com/Znt0Vpq.png =300x) -->
2. recompile
```
scons EXTRAS=../NVmain build/X86/gem5.opt
```
3. run hello world
```
./build/X86/gem5.opt configs/example/se.py \
-c tests/test-progs/hello/bin/x86/linux/hello \
--cpu-type=TimingSimpleCPU --caches --l2cache --l3cache --mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config
```

### Q3. Config last level cache to  2-way and full-way associative cache and test performance (15%)
1. compile quicksort.c
```
gcc --static quicksort.c -o quicksort
```
2. recompile
```
scons EXTRAS=../NVmain build/X86/gem5.opt
```
3. run
```
./build/X86/gem5.opt configs/example/se.py \
-c ../benchmark/quicksort --cpu-type=TimingSimpleCPU \
--caches --l1i_size=32kB --l1d_size=32kB --l2cache --l2_size=128kB \
--l3cache --l3_size=1MB --l3_assoc=16384 --mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config > cmdlog_full-way.txt
```
```
./build/X86/gem5.opt configs/example/se.py \
-c ../benchmark/quicksort --cpu-type=TimingSimpleCPU \
--caches --l1i_size=32kB --l1d_size=32kB --l2cache --l2_size=128kB \
--l3cache --l3_size=1MB --l3_assoc=2 --mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config > cmdlog_2-way.txt
```

### Q4. Modify last level cache policy based on frequency based replacement policy (15%)
1. add
    - `GEM5/src/mem/cache/replacement_policies/fb_rp.cc`
    - `GEM5/src/mem/cache/replacement_policies/fb_rp.hh`
2. modifiy
    - `GEM5/src/mem/cache/replacement_policies/ReplacementPolicies.py`
    - `GEM5/src/mem/cache/replacement_policies/SConscript`
    - `GEM5/configs/common/Caches.py`
3. recompile
```
scons EXTRAS=../NVmain build/X86/gem5.opt
```
4. run 
```
./build/X86/gem5.opt configs/example/se.py \
-c ../benchmark/quicksort --cpu-type=TimingSimpleCPU \
--caches --l1i_size=32kB --l1d_size=32kB --l2cache --l2_size=128kB \
--l3cache --l3_size=1MB --l3_assoc=2 --mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config > cmdlog_FB.txt
```
```
./build/X86/gem5.opt configs/example/se.py \
-c ../benchmark/quicksort --cpu-type=TimingSimpleCPU \
--caches --l1i_size=32kB --l1d_size=32kB --l2cache --l2_size=128kB \
--l3cache --l3_size=1MB --l3_assoc=2 --mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config > cmdlog_LRU.txt
```

### Q5. Test the performance of write back and write through policy based on 4-way associative cache with isscc_pcm(15%) 
1. modifiy
     - `GEM5/src/mem/cache/base.cc`
2. recompile
```
scons EXTRAS=../NVmain build/X86/gem5.opt
```
3. run
```
./build/X86/gem5.opt configs/example/se.py \
-c ../benchmark/multiply --cpu-type=TimingSimpleCPU \
--caches --l1i_size=32kB --l1i_assoc=4 --l1d_size=32kB --l1d_assoc=4 --l2cache --l2_size=128kB --l2_assoc=4 \
--l3cache --l3_size=1MB --l3_assoc=4 --mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config > cmdlog_WB.txt
```
```
./build/X86/gem5.opt configs/example/se.py \
-c ../benchmark/multiply --cpu-type=TimingSimpleCPU \
--caches --l1i_size=32kB --l1i_assoc=4 --l1d_size=32kB --l1d_assoc=4 --l2cache --l2_size=128kB --l2_assoc=4 \
--l3cache --l3_size=1MB --l3_assoc=4 --mem-type=NVMainMemory \
--nvmain-config=../NVmain/Config/PCM_ISSCC_2012_4GB.config > cmdlog_WT.txt
```

