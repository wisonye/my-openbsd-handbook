# Erase install media partition

Before burning the install media file to the USB, make sure destroy the existing
partition on the USB by running the following command. Otherwise, some USB burning
tools not work:

```bash
#
# Use `diskutil list` to list the dev id and then replace the `/dev/disk2`
# to what you see from the output
#
sudo dd if=/dev/zero of=/dev/disk2 bs=512 count=1
```
