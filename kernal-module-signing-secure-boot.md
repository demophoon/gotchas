Loading unsigned kernel modules when booting with uefi secure boot enabled
==========================================================================

So... Secure boot is rough if you need to make changes to any kernel modules
that have not explicitly been signed by a trusted party. I didn't actually
realize that the modules that I was attempting to install (Specifically the
vboxdrv modules for running virtualbox) were not signed and I kept getting the
`modprobe vboxdrv failed` message when attempting to set up virtualbox.

Good news though, it is fairly easy to work around by installing your own
signing key and signing the kernel modules with that key. So lets walk through
that.

Create your 'Machine Owner Key'
-------------------------------

`openssl req -new -x509 -newkey rsa:2048 -keyout MOK.prv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=Owner Name"`

Also to be safe make sure that this key is not world/group
read/write/executable.

`chmod 600 MOK.prv`

Import the public key onto the machine
--------------------------------------

`mokutil --import /path/to/MOK.der`

When prompted for a password just pick anything memorable. It will be used
during enrolling the MOK we have just imported.

We will need to reboot the machine and the bootloader will warn you about a new
key which you will need to enroll. Find the options for enrollment and type in
the password you previously assigned during the import.

Verify our public key is loaded after booting into your OS.

`dmesg | grep 'EFI:'`

You should see `Owner Name` somewhere in the output. As well as all of the other
loaded trusted kernel keys.

Sign any kernel modules with your private key
---------------------------------------------

You are now ready to start signing kernel modules with your private key.

Sign kernel modules with this command:

```sh
/usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 \
    /path/to/MOK.prv \
    /path/to/MOK.der /path/to/kernel.ko
```

**Note** This process will need to be repeated every time you update the kernel module.
