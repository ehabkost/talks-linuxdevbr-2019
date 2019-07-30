# CPU Vulnerabilities & KVM Security

Eduardo Habkost &lt;ehabkost\@redhat.com&gt;<br>
<br>
<a href="https://linuxdev-br.net/">Linux Developer Conference Brazil 2019</a>

Note:




## Contents

* KVM CPU configuration abstractions
* CPU vulnerabilities
* When things fall apart
* New abstractions


Note:



# CPU configuration abstractions


## CPUID

<pre><code class="lang-shell" data-trim data-noescape>
eax in: 0x00000000, eax = 00000016 ebx = 756e6547 ecx = 6c65746e edx = 49656e69
eax in: 0x00000001, eax = 000406e3 ebx = 00100800 ecx = 7ffafbff edx = bfebfbff
eax in: 0x00000002, eax = 76036301 ebx = 00f0b5ff ecx = 00000000 edx = 00c30000
eax in: 0x00000003, eax = 00000000 ebx = 00000000 ecx = 00000000 edx = 00000000
eax in: 0x00000004, eax = 1c004121 ebx = 01c0003f ecx = 0000003f edx = 00000000
eax in: 0x00000005, eax = 00000040 ebx = 00000040 ecx = 00000003 edx = 11142120
eax in: 0x00000006, eax = 000027f7 ebx = 00000002 ecx = 00000009 edx = 00000000
eax in: 0x00000007, eax = 00000000 ebx = 00000000 ecx = 00000000 edx = 00000000
eax in: 0x00000008, eax = 00000000 ebx = 00000000 ecx = 00000000 edx = 00000000
<em>[...]</em>
</code></pre>


## CPU flags

<pre><code class="lang-shell" data-trim data-noescape>
$ cat /proc/cpuinfo
vendor_id       : GenuineIntel
cpu family      : 6
model           : 78
model name      : Intel(R) Core(TM) i7-6600U CPU @ 2.60GHz
stepping        : 3
<mark>flags</mark>           : <mark>fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf tsc_known_freq pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch cpuid_fault epb invpcid_single pti ssbd ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm mpx rdseed adx smap clflushopt intel_pt xsaveopt xsavec xgetbv1 xsaves dtherm ida arat pln pts hwp hwp_notify hwp_act_window hwp_epp md_clear flush_l1d</mark>
<em>[...]</em>
</code></pre>


## CPU models: QEMU

<pre><code class="lang-shell" data-trim data-noescape>
$ qemu-system-x86_64 -cpu help
Available CPUs:
x86 486
x86 Broadwell             Intel Core Processor (Broadwell)
x86 Cascadelake-Server    Intel Xeon Processor (Cascadelake)
x86 Conroe                Intel Celeron_4x0 (Conroe/Merom Class Core 2)
x86 EPYC                  AMD EPYC Processor
x86 Haswell               Intel Core Processor (Haswell)
x86 Icelake-Client        Intel Core Processor (Icelake)
<em>[...]</em>
</code></pre>


## CPU models: libvirt

<pre><code class="lang-xml" data-trim>
&lt;cpu match='exact'>
  &lt;model>Broadwell&lt;/model>
&lt;/cpu>
</code></pre>


## CPU models: virt-manager

<img src="virt-manager-cpu-config.png" style="border: none; box-shadow: none; width: 55%; height: 100%;">


## Special CPU mode: host-passthrough


## Special CPU model: host-model



# CPU vulnerabilities


## Spectre / Meltdown (Jan 2018)


## SSB (May 2018)


## L1TF (Aug 2018)


## MDS (May 2019)



# When everything falls apart


# Law of Leaky Abstractions

<em style="font-size: 200%">“All non-trivial abstractions, to some degree, are leaky.”</em>

&mdash; Joel Spolsky, 2002

Note:
TODO: "what breaks" on each slide below


## -noTSX

Unexpected facts:
* New CPUs may have less features than older CPUs
* Installing a software update will make features go away


## -IBRS / -IBPB

Unexpected facts:
* Not all predefined CPU models are safe
* Configuration update is required to make VM safe


## MSR features (arch-capabilities)

Unexpected fact:
* Some CPU capabilities are not reported through CPUID


## Negative features

Unexpected fact:
* CPU flag should <b>not</b> be 1 on guest if it's 1 on host


## Extra feature flags

Unexpected fact:
* There's always a "safe" CPU model to choose from

Note:
TODO: example CPU config
Things get messy.


## Leaky abstractions push complexity to upper layers



# New Abstractions



# References

* [Vulnerabilities Associated with CPU Speculative Execution](https://vuls.cert.org/confluence/display/Wiki/Vulnerabilities+Associated+with+CPU+Speculative+Execution)