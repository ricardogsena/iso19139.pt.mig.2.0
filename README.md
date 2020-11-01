# Implementação do Schema para o Perfil Nacional de Metadados de Informação Geográfica (MIG) schema plugin

Implementação do Schema para o Perfil Nacional de Metadados de Informação Geográfica (MIG)

## Installing the plugin

### GeoNetwork version to use with this plugin

Use GeoNetwork 3.4+. It's not supported in older versions so don't plug it into it!

### Adding the plugin to the source code

The best approach is to add the plugin as a submodule into GeoNetwork schema module.

```
cd schemas
git submodule add -b 3.4.x https://github.com/metadata101/iso19139.pt.mig.2.0 iso19139.pt.mig.2.0
```

Add the new module to the schema/pom.xml:

```
  <module>iso19139</module>
  <module>iso19139.pt.mig.2.0</module>
</modules>
```

Add the dependency in the web module in web/pom.xml:

```
<dependency>
  <groupId>${project.groupId}</groupId>
  <artifactId>schema-iso19139.pt.mig.2.0</artifactId>
  <version>${gn.schemas.version}</version>
</dependency>
```

Add the module to the webapp in web/pom.xml:

```
<execution>
  <id>copy-schemas</id>
  <phase>process-resources</phase>
  ...
  <resource>
    <directory>${project.basedir}/../schemas/iso19139.pt.mig.2.0/src/main/plugin</directory>
    <targetPath>${basedir}/src/main/webapp/WEB-INF/data/config/schema_plugins</targetPath>
  </resource>
```

### Adding editor configuration

Editor configuration in GeoNetwork 3.4.x is done in `schemas/iso19139.pt.mig.2.0/src/main/plugin/iso19139.pt.mig.2.0/layout/config-editor.xml` inside each view. Default values are the following:

      <sidePanel>
        <directive data-gn-onlinesrc-list=""/>
        <directive gn-geo-publisher=""
                   data-ng-if="gnCurrentEdit.geoPublisherConfig"
                   data-config="{{gnCurrentEdit.geoPublisherConfig}}"
                   data-lang="lang"/>
        <directive data-gn-validation-report=""/>
        <directive data-gn-suggestion-list=""/>
        <directive data-gn-need-help="user-guide/describing-information/creating-metadata.html"/>
      </sidePanel>

### Build the application 

Once the application is build, the war file contains the schema plugin:

```
$ mvn clean install -Penv-prod
```

### Deploy the profile in an existing installation

After building the application, it's possible to deploy the schema plugin manually in an existing GeoNetwork installation:

- Copy the content of the folder schemas/iso19139.pt.mig.2.0/src/main/plugin to INSTALL_DIR/geonetwork/WEB-INF/data/config/schema_plugins/iso19139.pt.mig.2.0

- Copy the jar file schemas/iso19139.pt.mig.2.0/target/schema-iso19139.pt.mig.2.0-3.4.jar to INSTALL_DIR/geonetwork/WEB-INF/lib.

If there's no changes to the profile Java code or the configuration (config-spring-geonetwork.xml), the jar file is not required to be deployed each time.
