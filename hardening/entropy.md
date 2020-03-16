## Entropy
**Important:** There are many online tutorials/answers to increase entropy in headless environments. Many are not reliable in cryptographic terms, make sure that the source is trusted and validate the method from different sources. Arch wiki is an excellent source for this topic.

Needed for any sort of encryption, SSL/TLS... In virtualized headless environments, good-quality high entropy is problematic. `haveged` and/or `rng-tools` can be used to create entropy in headless environments. However, `haveged` should be used very carefully as it is not suitable for virtualized environments. Arch wiki recommends that:
> Unless you have a specific reason to not trust any hardware random number generator on your system, you should try to use them with the rng-tools first and if it turns out not to be enough (or if you do not have a hardware random number generator available), then use Haveged.

So try `rng-tools` first. Before doing anything, check available entropy to get an idea: `cat /proc/sys/kernel/random/entropy_avail`. At least in Debian Buster (10), note that there are three packages with names `rng-tools`, `rng-tools-debian` and `rng-tools5`. The upstream version is `rng-tools5`, I prefer installing that version.
Then:
```shell
# In a None-Debian OS, you might need to replace rng-tools5 with rng-tools
sudo apt-get install rng-tools5
# might be called rng-tools.service depending on distro/package
# e.g. rng-tools-debian
sudo systemctl start rngd.service
sudo systemctl enable rngd.service
# Check status to see if rng-tools is complaining about sth
sudo systemctl status rngd.service
```
Check if `rngd` has a source of entropy: `sudo rngd -v`. If the cluster does not have any external TPM chip, it is normal to see `Unable to open file: /dev/tpm0`. If you see `DRNG` entropy source, it is an Intel ‘hardware approach to high-quality, high-performance entropy and random number generation’ using the RDRAND processor instruction, which is good. Check if your processor has RDRAND instruction by `cat /proc/cpuinfo | grep rdrand`. RDRAND presence can also be checked by `sudo dmesg | grep -i crng`. If the output has something like `random: crng done (trusting CPU's manufacturer)`, then it is present. Another type of random number generator that you might see is VirtIO RNG, which is exposed by QEMU. To check VirtIO RNG, issue `cat /sys/devices/virtual/misc/hw_random/rng_available`, it will output something like `virtio_rng`. This is usually not applicable to cloud servers, but sometimes (especially in modern laptops and desktops) you have other hardware-backed RNGs, which are called Trusted Platform Modules (TPMs). You can check if one exists in your system via `cat /sys/devices/virtual/misc/hw_random/rng_available`, the output will be like `tpm-rng-0` or `tpm-rng`. Another indication of HWRNG is the presence of `/dev/hwrng` and/or `/dev/tpm0`. If everything is fine and you have at least one hardware RNG source (i.e. RDRAND, VirtIO RNG, TPM...) move on.

Check if it is working correctly. The first command should give instantaneous output when `rng-tools` working correctly. Without it, outputted `dd` speed would be extremely low (<10KB/s). The second command executes a test of 1000 passes, you should get a maximum of 1-2 failures.
```
dd if=/dev/random of=/dev/null bs=1024 count=1 iflag=fullblock
rngtest -c 1000 </dev/random
```

**Note**: If you have access to the BIOS settings of the platform, you might want to check if TPM or the other RNG generation-related settings are disabled.
