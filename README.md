# Hamcrest 2.2 for JUnit 4 in RDi 9.9

*Repairs the build path entry "JUnit 4" of Java projects in RDi 9.9.*

## Description

RDi 9.9 ships the bundle `org.hamcrest` in version 3.0.0 only, but the JUnit 4 classpath container
of RDi 9.9 needs version 2.2. Without it, every Java project with the build path entry *JUnit 4*
shows the error "Invalid classpath container: 'JUnit 4' in project ..." and cannot be built.

This feature adds `org.hamcrest` 2.2.0 and its source bundle to RDi 9.9. Version 3.0.0 stays
installed.

## Requirements

* RDi 9.9.
* For building the update site: Ant 1.9 or later, e.g. the Ant of RDi, and an installation of
  Eclipse 4.31 (2024-03), that contains the bundles `org.hamcrest` 2.2.0 and
  `org.hamcrest.source` 2.2.0.

## Installation

1. Build the archived update site: run `build.xml` with *Run As > Ant Build* in RDi, or `ant` in
   the folder of the project. If Eclipse 4.31 is not installed in `C:/IBM/Eclipse_4.31`, set its
   folder in the file `local.properties` next to `build.xml`:

   ```properties
   eclipse.home=D:/eclipse-4.31
   ```

   The build writes `target/Hamcrest-2.2-for-JUnit-4-RDi 9.9_1.0.0.r.zip`. The property
   `site.dir` in `local.properties` sets another folder.
2. Choose *Help > Install New Software...* in RDi 9.9, click *Add... > Archive...* and select the
   ZIP file.
3. Select *Hamcrest 2.2 for the JUnit 4 container*, finish the wizard and restart RDi.
4. If the error is still shown, choose *Project > Clean...* for the affected projects.

## Liability

This software is free and provided without warranty. You use it at your own risk: test it with
non-critical data first, and back up your data before you use it. The [license](LICENSE) applies in
addition.
