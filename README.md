# Notes on GNS3

### vSRX resource utilization

After many years of running vSRX 12.1X47-D10.4, i decided to look at more recent versions
* 20.1R1.11
* vSRX19.2R1.8

wowsers they use a lot more a lot more resource intensive that 12.1

typically for 12.1 1024MB RAM that idles @8.3%, for the other images they idle at 4G and 103% cpu

so investigation will be done to see what can be done about that

|vSRX    | RAM|cpu% idle|
|:-------|:--:|----------|
|12.1|1G|9%|
|19.2|4G|103%|
|20.1|4G|103%|

```
 4090 moz-uk      20   0 4838856   4.0g  20896 S 103.9  13.2   4:20.23 qemu-system-x86
 3844 moz-uk      20   0 4830632   4.0g  21172 S 102.7  13.2  19:55.57 qemu-system-x86
 3794 moz-uk      20   0 1522744   1.0g  21068 S   8.9   3.4   3:08.35 qemu-system-x86
```
###GNS SIMSERVER speciifcation
* AMD A8-5600K
* 32GB DDR3 RAM
* debian 10 x64
* QEMU KVM


cat /proc/cpuinfo
```
processor       : 0
vendor_id       : AuthenticAMD
cpu family      : 21
model           : 16
model name      : AMD A8-5600K APU with Radeon(tm) HD Graphics
stepping        : 1
microcode       : 0x6001119
cpu MHz         : 3791.736
cache size      : 2048 KB
physical id     : 0
siblings        : 4
core id         : 0
cpu cores       : 2
apicid          : 16
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc rep_good nopl nonstop_tsc cpuid extd_apicid aperfmperf pni pclmulqdq monitor ssse3 fma cx16 sse4_1 sse4_2 popcnt aes xsave avx f16c lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs xop skinit wdt lwp fma4 tce nodeid_msr tbm topoext perfctr_core perfctr_nb cpb hw_pstate ssbd vmmcall bmi1 arat npt lbrv svm_lock nrip_save tsc_scale vmcb_clean flushbyasid decodeassists pausefilter pfthreshold
bugs            : fxsave_leak sysret_ss_attrs null_seg spectre_v1 spectre_v2 spec_store_bypass
bogomips        : 7184.33
TLB size        : 1536 4K pages
clflush size    : 64
cache_alignment : 64
address sizes   : 48 bits physical, 48 bits virtual
power management: ts ttp tm 100mhzsteps hwpstate cpb eff_freq_ro

processor       : 1
vendor_id       : AuthenticAMD
cpu family      : 21
model           : 16
model name      : AMD A8-5600K APU with Radeon(tm) HD Graphics
stepping        : 1
microcode       : 0x6001119
cpu MHz         : 3791.733
cache size      : 2048 KB
physical id     : 0
siblings        : 4
core id         : 1
cpu cores       : 2
apicid          : 17
initial apicid  : 1
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc rep_good nopl nonstop_tsc cpuid extd_apicid aperfmperf pni pclmulqdq monitor ssse3 fma cx16 sse4_1 sse4_2 popcnt aes xsave avx f16c lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs xop skinit wdt lwp fma4 tce nodeid_msr tbm topoext perfctr_core perfctr_nb cpb hw_pstate ssbd vmmcall bmi1 arat npt lbrv svm_lock nrip_save tsc_scale vmcb_clean flushbyasid decodeassists pausefilter pfthreshold
bugs            : fxsave_leak sysret_ss_attrs null_seg spectre_v1 spectre_v2 spec_store_bypass
bogomips        : 7184.33
TLB size        : 1536 4K pages
clflush size    : 64
cache_alignment : 64
address sizes   : 48 bits physical, 48 bits virtual
power management: ts ttp tm 100mhzsteps hwpstate cpb eff_freq_ro

processor       : 2
vendor_id       : AuthenticAMD
cpu family      : 21
model           : 16
model name      : AMD A8-5600K APU with Radeon(tm) HD Graphics
stepping        : 1
microcode       : 0x6001119
cpu MHz         : 3791.562
cache size      : 2048 KB
physical id     : 0
siblings        : 4
core id         : 2
cpu cores       : 2
apicid          : 18
initial apicid  : 2
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc rep_good nopl nonstop_tsc cpuid extd_apicid aperfmperf pni pclmulqdq monitor ssse3 fma cx16 sse4_1 sse4_2 popcnt aes xsave avx f16c lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs xop skinit wdt lwp fma4 tce nodeid_msr tbm topoext perfctr_core perfctr_nb cpb hw_pstate ssbd vmmcall bmi1 arat npt lbrv svm_lock nrip_save tsc_scale vmcb_clean flushbyasid decodeassists pausefilter pfthreshold
bugs            : fxsave_leak sysret_ss_attrs null_seg spectre_v1 spectre_v2 spec_store_bypass
bogomips        : 7184.33
TLB size        : 1536 4K pages
clflush size    : 64
cache_alignment : 64
address sizes   : 48 bits physical, 48 bits virtual
power management: ts ttp tm 100mhzsteps hwpstate cpb eff_freq_ro

processor       : 3
vendor_id       : AuthenticAMD
cpu family      : 21
model           : 16
model name      : AMD A8-5600K APU with Radeon(tm) HD Graphics
stepping        : 1
microcode       : 0x6001119
cpu MHz         : 3791.736
cache size      : 2048 KB
physical id     : 0
siblings        : 4
core id         : 3
cpu cores       : 2
apicid          : 19
initial apicid  : 3
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc rep_good nopl nonstop_tsc cpuid extd_apicid aperfmperf pni pclmulqdq monitor ssse3 fma cx16 sse4_1 sse4_2 popcnt aes xsave avx f16c lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs xop skinit wdt lwp fma4 tce nodeid_msr tbm topoext perfctr_core perfctr_nb cpb hw_pstate ssbd vmmcall bmi1 arat npt lbrv svm_lock nrip_save tsc_scale vmcb_clean flushbyasid decodeassists pausefilter pfthreshold
bugs            : fxsave_leak sysret_ss_attrs null_seg spectre_v1 spectre_v2 spec_store_bypass
bogomips        : 7184.33
TLB size        : 1536 4K pages
clflush size    : 64
cache_alignment : 64
address sizes   : 48 bits physical, 48 bits virtual
power management: ts ttp tm 100mhzsteps hwpstate cpb eff_freq_ro
```
