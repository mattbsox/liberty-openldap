# liberty-openldap
Simple Open Liberty App with Open LDAP


## Setting up OpenLDAP

Run this command to start your [OpenLDAP Docker container](https://github.com/osixia/docker-openldap). It will bind ports 389 and 689.

```
docker run -p 389:389 -p 689:689 --name ldaptest --detach osixia/openldap:1.2.2
```

Then start and configure the [phpLDAPadmin](https://github.com/osixia/docker-phpLDAPadmin) to connect to your OpenLDAP container.

```
docker run -p 6443:443 --env PHPLDAPADMIN_LDAP_HOSTS=${docker inspect -f "{{ .NetworkSettings.IPAddress }}" testldap} --detach osixia/phpldapadmin:0.7.2
```

Once the container is running it can be accessed at `https://localhost:6443`. You'll need to login with the admin credentials. By default these are `cn=damin,dc=example,dc=org:admin`.

You'll need to add a new user that you can use to log into the application. This app is configured to use the inetOrgPerson type. 
In the phpLDAPAdmin click the drop down menu for the DN you have set up. It will be `dc=example,dc=org` by default.
Go to `Create new entry here` and select `Default`. Once in `Default` go to the drop down menu and select inetOrgPerson.
Set the `RDN` to `cn` and add an entry for the cn, sn, and password. Click create and commit the entry.
You should now have a new user in your OpenLDAP registry.

## Running and accessing the application

To run the application use:

```
mvn clean install liberty:start
```

Connect to the application at [localhost:9080/ldap-test/](localhost:9080/ldap-test/).

Enter your user's name and password into the login box. You'll need to use the whole name you entered earlier, e.g. `cn=user,dc=example,dc=org`.

If all goes well you should see `Hello, from a Servlet!`

To stop the server use:

```
mvn liberty:stop
```
