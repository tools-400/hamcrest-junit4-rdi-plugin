# Hamcrest 2.2 for JUnit 4 – Developer notes

How to build the archived update site and how to publish a release. For what the feature is for and
how to install it, see [README.md](README.md).

## Build

Requirements:

* Ant 1.9.1 or later, e.g. the Ant of RDi.
* An installation of Eclipse 4.31 (2024-03), that contains the bundles `org.hamcrest` 2.2.0 and
  `org.hamcrest.source` 2.2.0. Its `eclipsec.exe` runs the p2 publisher.

Run `build.xml` with *Run As > Ant Build* in RDi, or `ant` in the folder of the project. The build
writes `target/Hamcrest-2.2-for-JUnit-4-RDi-9.9_<version>.zip`. The version is the attribute
`version` of `feature/feature.xml`; the build reads it from there.

Local values go into the file `local.properties` next to `build.xml`, which git ignores:

| Property | Default | Meaning |
|---|---|---|
| `eclipse.home` | `C:/IBM/Eclipse_4.31` | the Eclipse 4.31 installation |
| `site.dir` | `target` | the folder of the ZIP file |
| `work.parent` | the temp folder | the parent of the work folder of the p2 publisher; its path must contain only the letters A-Z, digits and `. _ - ~` |
| `gh`, `git` | `gh`, `git` | the GitHub CLI and git, if they are not on the `PATH` |

```properties
eclipse.home=D:/eclipse-4.31
```

## Release

Requirements: git and the GitHub CLI (`gh`), logged in with write access to
`tools-400/hamcrest-junit4-rdi-plugin`.

1. Set the release version `x.y.z.r` in `feature/feature.xml`.
2. Commit and push to `main`.
3. Run the target `release`: *Run As > Ant Build...* in RDi with the target `release`, or
   `ant release`.

   The target checks that the version is a release version, that neither the release nor a draft
   of it exists, that an existing tag `v<version>` is on `HEAD`, that the clone has no changes and
   that `HEAD` is `main` on GitHub; for a new tag also that `user.name` and `user.email` are set in
   the git configuration. Then it builds the ZIP, shows the commit, the tag, the texts and the file
   (the tag message and the tagger only for a new tag), and asks for confirmation. It creates the
   annotated tag `v<version>` on `HEAD` with the GitHub API and the release
   `Hamcrest 2.2 for JUnit 4 v<version>` with the ZIP file.

   A tagger with characters beyond ASCII needs Ant in a JVM with `-Dfile.encoding=UTF-8`, e.g. on
   the console with `ANT_OPTS=-Dfile.encoding=UTF-8`; another `-Dfile.encoding`, e.g. `Cp1252` in
   the `RDi.ini` of an RDi, makes the target stop before it creates the tag.
4. Fetch the tag into the clone: `git fetch origin tag v<version>`.

`ant release -Drelease.dry.run=true` checks and builds only and shows what the target would
create. `-Drelease.confirmed=true` answers the confirmation, e.g. on a console without input.

If the target stops halfway:

* The tag exists on `HEAD`, the release is missing: run `release` again; it creates the release
  only.
* A draft release exists, e.g. of an aborted upload: delete it with
  `gh release delete v<version> -R tools-400/hamcrest-junit4-rdi-plugin` and run `release` again.
