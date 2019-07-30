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


## CPU flags


## CPU models: QEMU


## CPU models: libvirt


## CPU models: virt-manager


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