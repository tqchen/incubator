# Apache Incubator Website

This is the content and build scripts for http://incubator.apache.org/

## Contributing to the website content

You can fork from https://github.com/apache/incubator, test your changes as described below
and raise a pull request.

Use the [general@incubator.a.o](https://lists.apache.org/list.html?general@incubator.apache.org) mailing list to contact
the Incubator PMC which manages this website.

## Prerequisites for building the website locally

The website is built using [JBake](https://jbake.org/) and Groovy templates.
The builds for the website do require internet access.

- Install JBake from http://jbake.org/download.html
  - Currently it looks like version 2.6.0 or greater is required.
- Create an environment variable `JBAKE_HOME` pointing to your JBake installation.
  - `export JBAKE_HOME=/home/jenkins/tools/jbake/jbake-2.6.3`
  - `export JBAKE_HOME=/usr/local/Cellar/jbake/2.6.4`
- Ensure that you have a JVM locally, e.g. [OpenJDK](http://openjdk.java.net/install/)

## Building & testing the site

To test the site locally, use 

    ./build_local.sh -b -s
    
This builds the site, serves it locally at  http://localhost:8820/ and rebuilds the content fairly
quickly if any changes are made.

That script can be called with any of the [arguments you would pass to jbake](https://jbake.org/docs/2.6.4/#bake_command).

The `build_clutch.sh` script can be used to build the Clutch data, but that's updated automatically by the Jenkins builds
mentioned below so it's not required unless you want to test that.

### Local URLs
> TODO: not sure
> if this is still valid, JBake 2.6.4 replaces the _site.host_ variable automatically when running locally 
> so everything should by fine provided all URLs are computed based on the `site.host` variable.

By default the site URLs redirect to `incubator.apache.org`;
to change this edit `jbake.properties` and uncomment the line referencing `localhost`.
Alternatively you can rename `jbake-local.properties` , but do not commit unwanted changes to that file!

## Automated builds - website and Clutch data

Commits to the `master` branch are automatically checked out and built using `build_site.sh` by the 
[Incubator GIT Site - part 2](https://builds.apache.org/view/H-L/view/Incubator/job/Incubator%20GIT%20Site%20-%20part%202/)
Jenkins job. The results are pushed to the [`content` folder of the `asf-site` branch](https://github.com/apache/incubator/tree/asf-site/content)
which is in turn published automatically to http://incubator.apache.org/ by the ASF's `gitwcsub` mechanism.

The data for http://incubator.apache.org/clutch/ takes longer to build so it is handled by a separate
[SVN Clutch Analysis - part 1](https://builds.apache.org/view/H-L/view/Incubator/job/Incubator%20SVN%20Clutch%20Analysis%20-%20part%201/)
Jenkins job that runs the `build_clutch.sh` script that's scheduled to run regularly. The results are stored in the 
[`reserve` folder of the `asf-site` branch](https://github.com/apache/incubator/tree/asf-site/reserve)

Any build failures are reported to *[cvs@incubator.a.o](https://lists.apache.org/list.html?cvs@incubator.apache.org)*
mailing list.

## Asciidoctor

Most of the pages in the site are written using Asciidoctor.
While it is a form of asciidoc it does have some [syntax differences that are worth reviewing](http://asciidoctor.org/docs/asciidoc-syntax-quick-reference/)

## Groovy Templates

The site templates are written in groovy scripts.
Even though the files end with `.gsp` they are not GSP files and do not have access to tag libraries.
You can run custom code in them, similar to what is done in [homepage.gsp](templates/homepage.gsp) and [projectspage.gsp](templates/projectspage.gsp).