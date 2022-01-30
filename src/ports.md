# Ports

- Fetch port source tree

    ```bash
    cd /tmp
    ftp https://mirror.fsmg.org.nz/pub/OpenBSD/$(uname -r)/{ports.tar.gz,SHA256.sig}
    signify -Cp /etc/signify/openbsd-$(uname -r | cut -c 1,3)-base.pub -x SHA256.sig ports.tar.gz

    cd /usr
    doas tar xzf /tmp/ports.tar.gz
    ```

    After that, you got an entire port tree in `/usr/ports`:)

    </br>

- How to compile and install from port source


    </br>
