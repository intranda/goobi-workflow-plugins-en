# Installation for productive use

To install the plugin for productive use on a web server, proceed as follows:

## Install the BaseX XML database

First, the XML database BaseX must be downloaded from the BaseX website. The download can be done from here:

```text
https://basex.org/download/
```

![BaseX website](../../.gitbook/assets/intranda_administration_archive_management_install_01%20%281%29%20%281%29.png)

The easiest way to download from there is as a `ZIP package`, for example in version 9.4.4:

```text
http://files.basex.org/releases/9.4.4/BaseX944.zip
```

The downloaded zip file can then be unpacked. The database is usually installed under the following path:

```bash
/opt/digiverso/basex/
```

From a Linux or Mac terminal, downloading and unpacking would be done as follows:

```bash
cd /opt/digiverso/
wget http://files.basex.org/releases/9.4.4/BaseX944.zip
unzip BaseX944.zip
chown -R tomcat. basex/
```

After downloading and unpacking, the Jetty configuration must be adjusted so that the application is only accessible on `localhost`. To do this, ensure in the configuration file `/opt/digiverso/basex/webapp/WEB-INF/jetty.xml` that the `host` is set to `127.0.0.1`:

```markup
<?xml version="1.0"?>
<!DOCTYPE Configure PUBLIC "-//Jetty//Configure//EN"
  "http://www.eclipse.org/jetty/configure_9_3.dtd">

<Configure id="Server" class="org.eclipse.jetty.server.Server">
  <!-- Default connector. The Jetty stop port can be specified
       in the .basex or pom.xml configuration file.  -->
  <Call name="addConnector">
    <Arg>
      <New id="httpConnector" class="org.eclipse.jetty.server.ServerConnector">
        <Arg name="server"><Ref refid="Server"/></Arg>
        <Set name="host">127.0.0.1</Set>
        <Set name="port">8984</Set>
        <Set name="idleTimeout">60000</Set>
        <Set name="reuseAddress">true</Set>
      </New>
    </Arg>
  </Call>
</Configure>
```

Then the `Systemd Unit File` is installed to this path:

```bash
/etc/systemd/system/basexhttp.service
```

This has the following structure:

```bash
[Unit]
Description=BaseX HTTP server

[Service]
User=tomcat
Group=tomcat
ProtectSystem=full
ExecStart=/opt/digiverso/basex/bin/basexhttp
ExecStop=/opt/digiverso/basex/bin/basexhttp stop

[Install]
WantedBy=multi-user.target
```

The daemon must then be reloaded, the unit file activated and the database restarted.:

```bash
systemctl daemon-reload
systemctl enable basexhttp.service
systemctl start basexhttp.service
```

To ensure that the administration interface of BaseX can also be accessed externally, this can be configured in the Apache web server, for example, with the following section:

```bash
    redirect 301 /basex http://example.com/basex/
    <Location /basex/>
            Require ip 188.40.71.142
            ProxyPass http://localhost:8984/ retry=0
            ProxyPassReverse http://localhost:8984/
    </Location>
```

Afterwards, the Apache module `proxy_http` has to be activated and Apache has to be restarted for the adjustments to take effect.:

```bash
a2enmod proxy_http
systemctl restart apache2
```

The XML database can be accessed after installation under the following URL:

​[http://localhost:8984/](http://localhost:8984/)​

![Started BaseX Server](../../.gitbook/assets/intranda_administration_archive_management_install_02%20%281%29%20%281%29.png)

## Administer database and import EAD file

After BaseX has been downloaded and started, XML files can be imported as new databases. To do this, first open the menu item `Database Administration`, where a login can be made with these access data:

```text
Login:      admin
Password:   admin
```

![Login for the database administration](../../.gitbook/assets/intranda_administration_archive_management_install_03.png)

Therefore, after logging in for the first time, a new password should be assigned. To do this, open the menu item Users. Here you can click on the account name and set the new password.

After the successful login, an overview of the installed databases, log files, etc. is displayed.

![Administrative overview](../../.gitbook/assets/intranda_administration_archive_management_install_04.png)

New databases for the EAD files can be created under the menu item 'Databases'.

![Create new database](../../.gitbook/assets/intranda_administration_archive_management_install_05.png)

There you can now enter a `name` for the new database. Then the button `Create` must be clicked.

![Definition of the name of the new database](../../.gitbook/assets/intranda_administration_archive_management_install_06.png)

After the new database has been created, an XML file can be imported as content. To do this, click on the `Add` button.

![Details of the newly created database](../../.gitbook/assets/intranda_administration_archive_management_install_07%20%281%29.png)

Here, an EAD file can be selected as an XML file and a path can be assigned under which this data inventory is to be accessible. Then click on the 'Add' button.

![Upload an XML file](../../.gitbook/assets/intranda_administration_archive_management_install_08%20%281%29%20%282%29.png)

After importing the EAD file, the content is already available for the Goobi archive management plugin.

![Details of the imported XML file](../../.gitbook/assets/intranda_administration_archive_management_install_09%20%281%29.png)

In the administration area of BaseX, files can also be removed. To do this, these must be marked by means of the associated checkbox and then deleted by clicking on `Delete`. Updating an EAD file is only possible by deleting it first and then adding it again.

### Definition of the queries

To set up BaseX for query from Goobi, the database must be made aware of what such a query looks like, what is to be done with the result of the query and what the result of the query is to look like. BaseX offers various options for this. We have chosen 'RESTXQ' because, in contrast to the REST interface, it does not require any additional authentication.

To configure the queries, several new files have to be created in the path `/opt/digiverso/basex/webapp/`. These are located within the plugin repository under the path `plugin/src/main/resources/` and can also be copied from there into the folder `/opt/digiverso/basex/webapp/`. Alternatively, they can also be created automatically with the following commands:

```bash
cd /opt/digiverso/basex/webapp/
sudo -u tomcat wget https://raw.githubusercontent.com/intranda/goobi-plugin-administration-archive-management/master/plugin/src/main/resources/importFile.xq
sudo -u tomcat wget https://raw.githubusercontent.com/intranda/goobi-plugin-administration-archive-management/master/plugin/src/main/resources/listDatabases.xq
sudo -u tomcat wget https://raw.githubusercontent.com/intranda/goobi-plugin-administration-archive-management/master/plugin/src/main/resources/openDatabase.xq
sudo -u tomcat wget https://raw.githubusercontent.com/intranda/goobi-plugin-administration-archive-management/master/plugin/src/main/resources/findDb.xq
chown tomcat. *.xq
```

![\*.xq-Files from the checked out plugin](../../.gitbook/assets/intranda_administration_archive_management_install_10%20%281%29%20%282%29.png)

![Copied \*.xq files within the webapp directory of BaseX](../../.gitbook/assets/intranda_administration_archive_management_install_11%20%281%29.png)

Content of the file `listDatabases.xq`:

```text
(: XQuery file to return the names of all available databases :)
module namespace page = 'http://basex.org/examples/web-pagepage';
(:declare default element namespace "urn:isbn:1-931666-22-9";:)

declare
  %rest:path("/databases")
  %rest:single
  %rest:GET

function page:getDatabases() {
  let $ead := db:list()

  return
    <databases>
    {
      for $c in $ead
      return
        <database>
          <name>
            {$c}
          </name>
          {
          let $files := db:list-details($c)
          return
            <details>
              {$files}
            </details>
          }
        </database>
    }
  </databases>
};
```

Content of the file `openDatabase.xq`:

```text
(: XQuery file to return a full ead record :)
module namespace page = 'http://basex.org/examples/web-page';
declare default element namespace "urn:isbn:1-931666-22-9";

declare
  %rest:path("/db/{$database}/{$filename}")
  %rest:single
  %rest:GET

function page:getDatbase($database, $filename) {
  let $ead := db:open($database, $filename)/ead
  return
  <collection>
    {$ead}
  </collection>
};
```

Content of the file `importFile.xq`:

```text
(: XQuery file to return a full ead record :)
module namespace page = 'http://basex.org/examples/web-page';
declare default element namespace "urn:isbn:1-931666-22-9";

declare
  %rest:GET
  %rest:path("/import/{$db}/{$filename}")

updating function page:import($db, $filename) {
  let $path := '/opt/digiverso/basex/import/' || $filename
  let $details := db:list-details($db, $filename)

  return
    if (fn:empty($details)) then
      db:add($db, doc($path), $filename)
    else
      db:replace($db, $filename, doc($path))
};
```

Depending on where the BaseX database was installed, two adjustments must still be made for writing EAD files in the file system. First, a folder must be created and given the appropriate rights so that EAD files can be saved in it. This folder could be, for example, as follows:

```bash
/opt/digiverso/basex/import/
```

In order to be able to access this specified directory, it must of course actually exist on the system and therefore needs to be created if necessary:

```bash
mkdir /opt/digiverso/basex/import
```

This directory must now be configured correctly within two configuration files. First of all, the adjustment is made in configuration file `plugin_intranda_administration_archive_management.xml` so that the path is defined there:

```markup
<eadExportFolder>/opt/digiverso/basex/import</eadExportFolder>
```

In addition, the previously created file `importFile.xq` must also be adapted so that the following line refers to the correct path in it:

```bash
let $path := '/opt/digiverso/basex/import/' || $filename
```
