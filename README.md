# Pentaho Reporting Plugin for Apache Fineract

This is a [_Plugin_ for Apache Fineract](https://github.com/apache/fineract/blob/maintenance/1.6/fineract-doc/src/docs/en/deployment.adoc). The original work is this one https://github.com/vorburger/fineract-pentaho.

See [TODO](TODO.md) for possible future follow-up enhancement work.

The Pentaho reports has been updated to the version 9.3, please use the [`Pentaho Report Designed version 9.3`](https://sourceforge.net/projects/pentaho/files/Pentaho-9.3/client-tools/prd-ce-9.3.0.0-428.zip/download) 

## For Developers

Add the repository https://mifos.jfrog.io/artifactory/libs-snapshot-local/

Maven
```bash
    <dependency>
        <groupId>community.mifos</groupId>
        <artifactId>pentaho-plugin</artifactId>
        <version>1.8.3-SNAPSHOT</version>
    </dependency>
```
Gradle
```bash
    compile(group: 'community.mifos', name: 'pentaho-plugin', version: '1.8.3-SNAPSHOT')
```

## Download, Unzip and Use the binaries 

```bash
    mkdir fineract-pentaho  && cd fineract-pentaho
    wget https://mifos.jfrog.io/artifactory/libs-snapshot-local/org/mifos/fineract-pentaho/0.0.1-SNAPSHOT/fineract-pentaho-0.0.1-SNAPSHOT.tar
    tar -xvf fineract-pentaho-0.0.1-SNAPSHOT.tar
```

## Build & Use For Linux Users

This project is currently only tested against the very latest and greatest
bleeding edge Fineract `develop` branch on Linux Ubuntu 20.04LTS. Building and using it against
other versions may be possible, but is not tested or documented here.

1. Download and compile

```bash
    git clone https://github.com/openMF/fineract-pentaho.git
    cd fineract-pentaho && ./gradlew -x test distZip && cd ..
```
2. Export the Location of Pentaho Reports (prpt files) in a variable required by the Plugin

```bash
    export FINERACT_PENTAHO_REPORTS_PATH="$PWD/fineract-pentaho/pentahoReports/"
```    

3. Execute Apache Fineract with the location of the Mifos Pentaho Plugin library for Apache Fineract

```bash
java -Dloader.path=$MIFOS_PENTAHO_PLUGIN_HOME/libs/ -jar $APACHE_FINERACT_HOME/fineract-provider.jar
```

4. Test the Pentaho Reports Execution using the following curl example or through the Mifos Web App in the Reports Menu

```bash
    curl --location --request GET 'https://localhost:8443/fineract-provider/api/v1/runreports/Expected%20Payments%20By%20Date%20-%20Formatted?tenantIdentifier=default&locale=en&dateFormat=dd%20MMMM%20yyyy&R_startDate=01%20January%202022&R_endDate=02%20January%202023&R_officeId=1&output-type=PDF&R_loanOfficerId=-1' \
--header 'Fineract-Platform-TenantId: default' \
--header 'Authorization: Basic bWlmb3M6cGFzc3dvcmQ='
```

5. The output must be a PDF with the Expected Payment By Date Formated information in it (maybe it could have blank or zeroes if it is a fresh Fineract Setup)

![alt text](https://github.com/openMF/fineract-pentaho/blob/1.8/img/screenshot_pentaho_report.png?raw=true)

The API call (above) should not fail if you follow the steps as shown, and all conditions met for the version of Apache Fineract

If the API call (above) [fails with](https://issues.apache.org/jira/browse/FINERACT-1173) 
_`"There is no ReportingProcessService registered in the ReportingProcessServiceProvider for this report type: Pentaho"`_, 
then this Fineract Pentaho Plugin has not been correctly registered & loaded by Apache Fineract.

## What script does

The script basically just creates the following directory structure and download the required files by the Pentaho Report Engine and the Database Connection (In this case MySQL):

```bash
    fineract-provider.jar
    lib/
        fineract-pentaho-0.0.1-SNAPSHOT-plain.jar
        mysql-connector-j-8.0.31.jar
        libswing-9.3.0.0-428.jar
        libformat-9.3.0.0-428.jar
        libpixie-9.3.0.0-428.jar
        classic-core-9.3.0.0-428.jar
        flute-9.3.0.0-428.jar
        classic-extensions-scripting-9.3.0.0-428.jar
        libserializer-9.3.0.0-428.jar
        libdocbundle-9.3.0.0-428.jar
        librepository-9.3.0.0-428.jar
        wizard-core-9.3.0.0-428.jar
        libformula-9.3.0.0-428.jar
        libxml-9.3.0.0-428.jar
        libbase-9.3.0.0-428.jar
        classic-extensions-9.3.0.0-428.jar
        libfonts-9.3.0.0-428.jar
        commons-database-model-9.3.0.0-428.jar
        libsparkline-9.3.0.0-428.jar
        libloader-9.3.0.0-428.jar
        js-scriptengine-22.2.0.jar
        commons-vfs2-2.7.0.jar
        groovy-all-2.4.8.jar
        ehcache-core-2.5.1.jar
        barcode4j-2.0.jar
        js-1.7R3.jar
        bsf-2.4.0.jar
        barbecue-1.5-beta1.jar
        graal-sdk-22.2.0.jar
        js-22.2.0.jar
```

and then it launches Apache Fineract with the Pentaho Plugin and all its dependencias.

See also [`PentahoReportsTest`](src/test/java/org/mifos/fineract/pentaho/PentahoReportsTest.java) and the [`test`](test) script.


## License

This code used to be part of the Mifos codebase before it became Apache Fineract.
During that move, the Pentaho related code had to be removed, because Pentaho's license
prevents code using it from being part of an Apache Software Foundation hosted project.

The correct technical solution to resolve such conundrums is to use a plugin architecture - which is what this is.

Note that the code and report templates in this git repo itself are
[licensed to you under the Mozilla Public License 2.0 (MPL)](https://github.com/openMF/fineract-pentaho/blob/develop/LICENSE).
This is a separate question than the license that Pentaho itself (i.e. the JAR/s of Pentaho) are made available under.


## Contribute

If this Fineract plugin project is useful to you, please contribute back to it (and
Fineract) by raising Pull Requests yourself with any enhancements you make, and by helping
to maintain this project by helping other users on Issues and reviewing PR from others
(you will be promoted to committer on this project when you contribute).  We recommend
that you _Watch_ and _Star_ this project on GitHub to make it easy to get notified.
