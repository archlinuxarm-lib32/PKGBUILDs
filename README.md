# ArchLinux ARM lib32 repo
This is a repo of lib32 multilib packages for ArchLinux ARM.

## Why it exists?
Well though many people claimed that armv7l to aarch64 are not like i386 to amd64, for end users they're very similiar. You can run armv7l bianries on aarch64 platforms(except for some ARMv9 platforms), and why not?

## Current status
It's very experimental now and should be used in production environment.
Due to the header difference between arm and aarch64(gcc treated them like two different architecture), it's not possible to store arm headers under `/usr/include`.
So in every lib32 package, there are:

 - `/usr/include32` --- for armv7l headers(not a good choice and definitely not FHS compatible, may change later)
 - `/usr/lib32` --- for armv7l libs

Worth to note that in lib32 packages every binary is armv7l architecture, which means the lib32-gcc is not a cross compiler. That's decided for best multilib experience.
The glibc package should be installed instead of system's glibc. It provided libc for both aarch64 and armv7l. Using different version of glibc for 32-bit and 64-bit architecture may cause unwanted affects.

## Packaging rule for lib32
If you want to make or submit your own lib32 package, make sure:
 - Use the compiler from lib32-gcc instead of some cross compilers.
 - If the aarch64 version of the same package exists, then your package shouldn't install any config files under `/etc` and any files under `/usr/share`. Use the aarch64 packages' configs instead(mark them as dependency if needed)
 - Install all your libraries under `/usr/lib32`. They should all be in armv7l architecture.
 - Install all your headers under `/usr/include32`.
 - Install your binaries under `/usr/bin`. Keep in mind that they should be armv7l binaries. Remember to add a prefix to your binaries so they dont overlap on aarch64 ones.

