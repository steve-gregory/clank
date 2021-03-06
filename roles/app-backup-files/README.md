app-backup-files
===================

Using a nested list data structure, dump the files in said data structure to a remote directory as a backup.

Requirements
------------

None.

Role Variables
--------------
- `BACKUP_PATH`     - Path of where you want the backedup files dumped to    
- `LIST_OF_BACKUPS` - List of lists that contains files along with a name of the project they belong to.

    An example of the variable:
  
    LIST_OF_BACKUPS:
      # Copy out Atmosphere specific confs
      - NAME: atmosphere
        PROJECT_NAME: atmo
        FILES:
            - /opt/dev/atmosphere/variables.ini
            - /opt/dev/atmosphere/atmosphere/settings/local.py
            - /opt/dev/atmosphere/atmosphere/settings/secrets.py

        # Copy out troposphere specific conf
      - NAME: troposphere
        PROJECT_NAME: tropo
        FILES:
          - /opt/dev/troposphere/variables.ini
          - /opt/dev/troposphere/package.json
          - /opt/dev/troposphere/troposphere/settings/local.py

        # Copy out nginx/uswgi confs
      - NAME: web_confs
        PROJECT_NAME: web_confs
        FILES:
          - /etc/nginx/sites-enabled
          - /etc/nginx/locations
          - /etc/uwsgi/apps-enabled
          - /etc/default/celeryd  

Dependencies
------------

None.

Example Playbook
----------------

Example using default variables:

    - hosts: all
      roles:
         - { role: app-backup-files,
             tags: ['data-backup', 'backup', 'files'] }

Example defining an alternative back up location:

    - hosts: all
      roles:
         - { role: app-backup-files,
             BACKUP_PATH: /root/backups_are_important,
             tags: ['data-backup', 'backup', 'files'] }

Example defining a collection of files to be backed up:

    - hosts: all
      vars:
        MY_BACKUPS:
            # Copy out Atmosphere specific confs
            - NAME: atmosphere
            PROJECT_NAME: atmo
            FILES:
              - /opt/dev/atmosphere/variables.ini
              - /opt/dev/atmosphere/atmosphere/settings/local.py

            # Copy out troposphere specific conf
            - NAME: troposphere
            PROJECT_NAME: tropo
            FILES:
              - /opt/dev/troposphere/variables.ini

      roles:
         - { role: app-backup-files,
             LIST_OF_BACKUPS : MY_BACKUPS, 
             tags: ['data-backup', 'backup', 'files'] }

This will create a backup directory containing two dirs (atmo, tropo). Each of those dirs will contain their respective files for safe keeping.  

License
-------

BSD
