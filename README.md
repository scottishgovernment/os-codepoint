# Ordnance Survey Code Point

## Introduction

This project provides a Debian package containing an extract of the
Ordnance Survey Code Point data set. It is used by the mygov.scot site
for address lookup, and includes only files containing Scottish postcodes.

Code-Point provides a precise geographical location for 1.6 million
postcode units in Great Britain and Northern Ireland. Each one contains
an average of 15 adjoining addresses.

For more information on the data set, please refer to:
http://www.ordnancesurvey.co.uk/business-and-government/products/code-point.html

## Updating the data set

The Code Point data set is updated quarterly; updates are published in
February, May, August and November.

The version number of this project reflects the release date of the data set
that it includes.

The Ordnance Survey Code Point data contains CRLF line endings.
These having been converted to LF line endings when being imported
into this repository.

To update the data set:

* Run `git config core.eol lf`
* Run `git config core.autocrlf input`
* Remove the Doc and Data directories.
* Unzip the latest Code Point data from Ordnance Survey.
* Check the LICENSE.md still matches Doc/license.txt.
* Add files containing Scottish postcodes:
  `git add $(for d in Data/CSV/*.csv ; do awk -F, '$9 ~ /"S/ {exit 1}' $d || echo $d; done)`
* Commit.

To deploy the new release using Maven:

* Use a clean checkout to ensure files have expected LF line endings.
* Determine the release version, based on the release date of the Code Point
  data set. This should be in the form `YYYY.MM` where MM is one of 02, 05, 08, 11.
* Use the release plugin to prepare (tag and verify) the release. For example, to release
  the August 2015 data set, use:
  `mvn -B release:prepare -DreleaseVersion=2015.08 -DdevelopmentVersion=2015.11-SNAPSHOT`
* Use the release plugin to build and deploy the release:
  `mvn -B release:perform`
