LDAP
----

You can set-up a local ldap with a UI managment front in a few steps.

 * Install docker then follow the instructions here for creating an ldap instance https://github.com/Enalean/docker-ldap-dev
 * Download and install http://phpldapadmin.sourceforge.net/wiki/index.php/Installation
 * Modify config.php to your liking
 * Restart apache and go to [name of your localhost]/phpldapadmin
 * Hack one of files in phpldapadmin (known bug) http://stackoverflow.com/questions/20673186/getting-error-for-setting-password-feild-when-creating-generic-user-account-phpl
 * Log-in with the crediantials from the docker README: (currently) cn=Manager,dc=tuleap,dc=local / welcome0

Example config.php:

    .. code-block:: php

        $config->custom->appearance['friendly_attrs'] = array(
            'facsimileTelephoneNumber' => 'Fax',
            'gid'                      => 'Group',
            'mail'                     => 'Email',
            'telephoneNumber'          => 'Telephone',
            'uid'                      => 'User Name',
            'userPassword'             => 'Password'
        );

        ......

        /*********************************************
         * Define your LDAP servers in this section  *
         *********************************************/

        $servers = new Datastore();

        $servers->newServer('ldap_pla');
        $servers->setValue('server','name','My LDAP Server');
        $servers->setValue('server','host','ldap://localhost');
        $servers->setValue('login','auth_type','cookie');
        $servers->setValue('login','bind_id','cn=Manager,dc=tuleap,dc=local');
        $servers->setValue('login','bind_pass','welcome0');


Using your local LDAP with a local gerrit
`````````````````````````````````````````

Use this config in ``etc/gerrit.conf``:

    .. code-block:: bash

        [auth]
            type = LDAP
        [ldap]
            server = ldap://localhost
            accountBase = ou=people,dc=tuleap,dc=local
            groupBase = ou=groups,dc=tuleap,dc=local
            accountFullName = cn
            sslVerify = false


