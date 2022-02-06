# Configuration

### `List`

`{}` just like an array, you can hold a group of ip addreses or ports or protocols
in the `{}` which can be applied to the rules

```bash
# Ip address list, `!` works as well
{192.168.1.0/24 !192.168.1.5}

# Port list
{22 25 80}

# Protocol list
{tcp udp}

# Host list
{'www.google.com' 'www.google.co.nz' 'www.google.com.au'}
```

If you put hostname into list, then when the hostname is resolved to an IP
address, all resulting IPv4 and IPv6 addresses are placed into the table. That's
why sometimes you will see the dynamic talbe name like `<__automatic_95f8422e_0>`
when you print the rules!!!

</br>


The commas between list items are optional, so the following lists are the same:
```bash
{22,25,80}
{22 25 80}
```

</br>


### `Table`

Lookups against a table are very fast and consume less memory and processor
time than lists. For this reason, a table is ideal for holding a large group
of addresses as the lookup time on a table holding 50,000 addresses is only
slightly more than for one holding 50 addresses. Table only can hold addresses
but not ports.

- Define table

    Syntax: `table <TALBE_NAME_HERE> {}`

    ```bash
    table <ssh_white_list> {192.168.1.10 192.168.1.11}
    ```

    </br>

- How to show table

    Syntax: `doas pfctl -t {TABLE_NAME_HERE} -T{COMMAND_HERE}`

    ```bash
    doas pfctl -t ssh_white_list -Tshow
    ```

    </br>

- How to test a IP address to see whether math your table or not

    ```bash
    # Define ssh_white_list table
    table <ssh_white_list> persist {192.168.1.0/24 !192.168.1.1 !192.168.1.254 !192.168.1.255}
    ```

    `persist` is saying keep the table in the memory even there is no rule use it,
    Otherwise, you can't show it or test ip:)

    ```bash
    # Should match
    doas pfctl -t ssh_white_list -Ttest 192.168.1.2

    # Should NOT match
    doas pfctl -t ssh_white_list -Ttest 192.168.1.1 192.168.1.254 192.168.1.255
    ```

