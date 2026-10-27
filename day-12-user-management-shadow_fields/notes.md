## To create a customize users:

`useradd -u 3000 -g 2000 -c "dev" -m -d /home/dev -s /bin/sh Denver`

Note:

    - group can we edited only if a group exists before

    - `-m` --> to create a home directory with the name of user

    - `-d` --> to give a personalized name to the directory


## To modify any existing user:

`usermod <flags> <arguments> <username>`

eg. `usermod -u 3000 kali`

# Fields of `/etc/shadow`

`kali:$y$j9T$aAqVqCXElakDBUvcM7oyv/$l7PnexCC6Y3c8UrUjQuTy/lBPxTzggCZe43/YOhaSX3:20753:0:99999:7:::`

- username --> kali

- encrypted password --> $y$j9T$aAqVqCXElakDBUvcM7oyv/$l7PnexCC6Y3c8UrUjQuTy/lBPxTzggCZe43/YOhaSX3

- days count from 1st 1970 till the last password change -->20753  --> `chage -d`

- min days --> 0 i.e. for minimum 2 days the user cannot change the password --> `chage -m`

- max days --> maximum days for password expiry --> `chage -M`

- warning days --> days from when the change password warning start comming --> `chage -W`

- password inactive --> extra days or time given to expiry of passwords --> `chage -I`

- account expiry --> days after which the account would expire --> `chage -E`

- future use --> ? (for future use)

`chage -d 0 -m 0 -M 0 kali` --> force user to change their password

`usermod -s /sbin/nologin kali` --> to remove the login shell of any user