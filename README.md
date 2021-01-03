# Shibboleth IdP Demo Docker Image
This is a Shibboleth IdP docker image that contains a read-to-run configuration that allows tester to start working with a Shibboleth IdP as quick as possible.

## Running the image

This Demo Shibboleth IdP is configured to access a LDAP server from docker host using the following connection:

-   Port: 389
-   Base OU: dc=example,dc=org
-   LDAP Service Account DN: cn=admin,dc=example,dc=org
-   LDAP Service Account Password: admin

A starter openldap docker is readily usable from [osixia/openldap](https://hub.docker.com/r/osixia/openldap). Just issue the following command to start the openldap docker container before running this Shibboleth IdP:

```docker run -p 389:389 -p 636:636 --name my-openldap-container --detach osixia/openldap:1.4.0```

Then, this demo IdP can be run in foreground by this command:

```docker run -p 443:443 itbill/shib-idp-demo```

Alternatively, you may run this demo IdP in background as follows:

```docker run -d -p 443:443 itbill/shib-idp-demo```

# About This Image

This demo Shibboleth IdP is configured by the following steps: 

1.  Pull the latest tier/shibbidp_configbuilder_container from TIER

    ```docker pull tier/shibbidp_configbuilder_container```

2.  Run the configuration builder from the container pulled above

    ```docker run -it -v ${PWD}:/output -e "BUILD_ENV=LINUX" tier/shibbidp_configbuilder_container```

    and supply the following information to the configuration builder

    -   FQDN: localhost
    -   Scope: localhost
    -   LDAP: ldap://host.docker.internal:389
    -   LDAP Base OU: dc=example,dc=org
    -   LDAP Service DN: cn=admin,dc=example,dc=org
    -   LDAP Service DN Password: admin

3.  Modify the generated Dockerfile to refer to specific version of Shibboleth IdP image:

    ```
    FROM tier/shib-idp:4.0.1_20201130
    ```

4.  Build this docker image

    ```docker build --no-cache -t itbill/shib-idp-demo .```

# Reference
TIER Shibboleth-IdP Container Instructions

https://docs.google.com/document/d/17-0O3Tvty9PONL6wu4PiC6ZWramdyntXmOsq1UpD2tE/edit

InCommon Trusted Access Platform documentation:

https://spaces.at.internet2.edu/display/ITAP/InCommon+Trusted+Access+Platform+Release
