# ChangeLog

* **0.11.5-M1**
  - Fix problem when creating intermediate archive for collecting assembly files introduced with #139. The 
    container can be now set with "mode" in the assembly configuration with the possible values `dir`, `tar`, `tgz`
    and `zip` (#171)
  - Workaround Docker problem when using an implicit registry `index.docker.io` when no registry is explicitly given. 
  - Fixed references to docker hub in documentation (#169)
  - Fixed registry authentication lookup (#146)
  
* **0.11.4**
  - Fixed documentation for available properties
  - Changed property `docker.assembly.exportBase` to `docker.assembly.exportBaseDir` (#164)
  - Changed default behaviour of `exportBaseDir` (true if no base image used with `from`, false otherwise)
  - Fix log messages getting cut off in the build (#163)
  - Allow system properties to overwrite dynamic port mapping (#161)
  - Fix for empty authentication when pushing to registries (#102)
  - Added watch mode for images with `-Ddocker.watch` (#141)
  - Added support for inline assemblies (#157, #158)
  - Add support for variable substitution is environment declarations (#137)
  - Use Tar archive as intermediate container when creating image (#139)  
  - Better error handling for Docker errors wrapped in JSON response only (#167) 
  
* **0.11.3**
  - Add support for removeVolumes in `docker:stop` configuration (#120)
  - Add support for setting a custom maintainer in images (#117)
  - Allow containers to be named using `<namingStrategy>alias</namingStrategy>` when started (#48)
  - Add new global property 'docker.verbose' for switching verbose image build output (#36)
  - Add support for environment variables specified in a property file (#128)
  - Documentation improvements (#107, #121)
  - Allow to use a dockerFileDir without any assembly
  
* **0.11.2**
  - Fix maven parse error when specifying restart policy (#99)
  - Allow host names to be used in port bindings (#101)
  - Add support for tagging at build and push time (#104)
  - Use correct output dir during multi-project builds (#97)
  - `descriptor` and `descriptorRef` in the assembly configuration are now optional (#66)
  - Fix NPE when filtering enabled during assembly creation (#82)
  - Allow `${project.build.finalName}` to be overridden when using a pre-packaged assembly descriptor
    for artifacts (#111)

* **0.11.1**
  - Add support for binding UDP ports (#83)
  - "Entrypoint" supports now arguments (#84)
  - Fix basedir for multi module projects (#89)
  - Pull base images before building when "autoPull" is switched on (#76, #77, #88)
  - Fix for stopping containers without tag (#86)
  
* **0.11.0**
  - Add support for binding/exporting containers during startup (#55)
  - Provide better control of the build assembly configuration. In addition, the plugin will now search
    for assembly descriptors in `src/main/docker`. This default can be overridden via the global
    configuration option `sourceDirectory`.
  - An external `Dockerfile` can now be specified to build an image.
  - When "creating" containers they get now all host configuration instead of during "start". This is
    the default behaviour since v1.15 while the older variant where the host configuration is fed into
    the "start" call is deprecated and will go away.
  - Allow selecting the API version with the configuration "apiVersion".
    Default and minimum API version is now "v1.15"
  - A registry can be specified as system property `docker.registry` or
    environment variable `DOCKER_REGISTRY` (#26)
  - Add new wait parameter `shutdown` which allows to specify the amount of time to wait between stopping
    a container and removing it (#54)

Please note, that the syntax for binding volumes from another container has changed slightly in 0.10.6.
See "[Volume binding](manual.md#volume-binding)" for details but in short:

````xml
<run>
  <volumes>
    <from>data</from>
    <from>jolokia/demo</from>
  </volumes>
  ....
</run>
````

becomes

````xml
<run>
  <volumes>
    <from>
      <image>data</image>
      <image>jolokia/demo</image>
    </from>
  </volumes>
  ....
</run>
````

The syntax for specifying the build assembly configuration has also changed. See "[Build Assembly]
(manual.md#build-assembly)" for details but in short:

`````xml
<build>
  ...
  <exportDir>/export</exportDir>
  <assemblyDescriptor>src/main/docker/assembly.xml</assemblyDescriptor>  
</build>  
````

becomes

`````xml
<build>
  ...
  <assembly>
    <basedir>/export</basedir>
    <descriptor>assembly.xml</descriptor>
  </assembly>
</build>           
````

* **0.10.5**
  - Add hooks for external configurations
  - Add property based configuration for images (#42)
  - Add new goal `docker:logs` for showing logs of configured containers (#49)
  - Support for showing logs during `docker:start` (#8)
  - Use `COPY` instead of `ADD` when putting a Maven assembly into the container (#53)
  - If `exportDir` is `/` then do not actually export (since it doesn't make much sense) (see #62)

* **0.10.4**
  - Restructured and updated documentation
  - Fixed push issue when using a private registry (#40)
  - Add support for binding to an arbitrary host IP (#39)

* **0.10.3**
  - Added "remove" goal for cleaning up images
  - Allow "stop" also as standalone goal for stopping all managed builds

* **0.10.2**
  - Support for SSL Authentication with Docker 1.3. Plugin will respect `DOCKER_CERT_PATH` with fallback to `~/.docker/`. 
    The plugin configuration `certPath` can be used, too and has the highest priority.
  - Getting rid of UniRest, using [Apache HttpComponents](http://hc.apache.org/) exclusively for contacting the Docker host.
  - Support for linking of containers (see the configuration in the [shootout-docker-maven](https://github.com/rhuss/shootout-docker-maven/blob/master/pom.xml) POM)
    Images can be specified in any order, the plugin takes care of the right startup order when running containers.
  - Support for waiting on a container's log output before continuing 

## 0.9.x Series 

Original configuration syntax (as described in the old [README](readme-0.9.x.md))

* **0.9.12**
  - Fixed push issue when using a private registry (#40)

* **0.9.11**
  - Support for SSL Authentication with Docker 1.3. Plugin will respect `DOCKER_CERT_PATH` with fallback to `~/.docker/`. 
    The plugin configuration `certPath` can be used, too and has the highest priority.
