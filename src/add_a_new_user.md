# Add a new user

```bash
#
# Create user own group (for assigning the home folder with `700` permission)
#
doas groupadd mike

#
# Create user home folder
#
doas mkdir /home/mike

#
# Create user with the different user group and default shell
#
doas useradd -g mike -s /usr/local/bin/fish mike

#
# Add alternative group to created user if needed
#
doas usermod -G wheel mike

#
# Set new passwd
#
doas passwd mike

#
# Change user home folder permission
#
doas chown -R mike:mike /home/mike
doas chmod -R 700 /home/mike

#
# Optional, show the user info
#
userinfo mike
```

</br>
