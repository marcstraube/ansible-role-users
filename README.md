# users

Manage users and groups.


## Requirements

This role requires root access, so either run it in a playbook with a global ```become: true```, or invoke the role in
your playbook like:

    - hosts: 'all'
      roles:
        - role: 'marcstraube.users'
          become: true


## Role variables

    users_password_salt: 'changeme'

The salt with which user passwords will be salted during encrytion.

    users_groups_list: []

You can add or remove groups with a few additional directives, like the following example:

    users_groups_list:
      # group to add
      - name: 'mygroup'
	    gid: 2000
      # group to remove
      - name: 'removeme'
        state: 'absent'

```gid``` is optional and ```state``` only has to be set if you want to remove the group.

    users_user_list: []

Users can be added and removed with the following directives:

    users_user_list:
      - username: 'myuser'
        uid: 1500
        gid: 1500
        group: 'myusergroup'
        groups:
          - 'users'
          - 'wheel'
        shell: '/bin/bash'
        password: 'mypassword'
        update_password: 'always'
    

All directives, except the ```username``` are optional. If no ```group``` is provided, a default
group with the same name as the username will be created.

```update_password``` defaults to ```always```. If you only want the password to be set when the
user is created, use ```on_create```.

If you want to remove a user, set ```state: 'absent'```.
    

## Example Playbook

    - hosts: all
      vars:
        password_salt: 'mypasswordsalt'
        groups_list:
          - name: 'mygroup'
            gid: 2000
          - name: 'grouptoremove'
            state: 'absent'
        users_list:
          - username: 'myuser'
            uid: 1500
            group: 'myusergroup'
            gid: 1500
            groups:
              - 'users'
              - 'wheel'
            shell: '/bin/bash'
            password: 'mypassword'
          - username: 'usertoremove'
            state: 'absent'
          roles:
            - { role: marcstraube.users }

## License

GPLv3
