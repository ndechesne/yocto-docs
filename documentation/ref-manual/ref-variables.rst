.. _ref-variables-glos:

******************
Variables Glossary
******************

This chapter lists common variables used in the OpenEmbedded build
system and gives an overview of their function and contents.

.. _ref-variables-glossary:

`A <#var-ABIEXTENSION>`__ `B <#var-B>`__ `C <#var-CACHE>`__
`D <#var-D>`__ `E <#var-EFI_PROVIDER>`__ `F <#var-FEATURE_PACKAGES>`__
`G <#var-GCCPIE>`__ `H <#var-HOMEPAGE>`__ `I <#var-ICECC_DISABLED>`__
`K <#var-KARCH>`__ `L <#var-LABELS>`__ `M <#var-MACHINE>`__
`N <#var-NATIVELSBSTRING>`__ `O <#var-OBJCOPY>`__ `P <#var-P>`__
`R <#var-RANLIB>`__ `S <#var-S>`__ `T <#var-T>`__
`U <#var-UBOOT_CONFIG>`__ `V <#var-VOLATILE_LOG_DIR>`__
`W <#var-WARN_QA>`__ `X <#var-XSERVER>`__

ABIEXTENSION
   Extension to the Application Binary Interface (ABI) field of the GNU
   canonical architecture name (e.g. "eabi").

   ABI extensions are set in the machine include files. For example, the
   ``meta/conf/machine/include/arm/arch-arm.inc`` file sets the
   following extension: ABIEXTENSION = "eabi"

ALLOW_EMPTY
   Specifies whether to produce an output package even if it is empty.
   By default, BitBake does not produce empty packages. This default
   behavior can cause issues when there is an
   ```RDEPENDS`` <#var-RDEPENDS>`__ or some other hard runtime
   requirement on the existence of the package.

   Like all package-controlling variables, you must always use them in
   conjunction with a package name override, as in: ALLOW_EMPTY_${PN} =
   "1" ALLOW_EMPTY_${PN}-dev = "1" ALLOW_EMPTY_${PN}-staticdev = "1"

ALTERNATIVE
   Lists commands in a package that need an alternative binary naming
   scheme. Sometimes the same command is provided in multiple packages.
   When this occurs, the OpenEmbedded build system needs to use the
   alternatives system to create a different binary naming scheme so the
   commands can co-exist.

   To use the variable, list out the package's commands that also exist
   as part of another package. For example, if the ``busybox`` package
   has four commands that also exist as part of another package, you
   identify them as follows: ALTERNATIVE_busybox = "sh sed test bracket"
   For more information on the alternatives system, see the
   "```update-alternatives.bbclass`` <#ref-classes-update-alternatives>`__"
   section.

ALTERNATIVE_LINK_NAME
   Used by the alternatives system to map duplicated commands to actual
   locations. For example, if the ``bracket`` command provided by the
   ``busybox`` package is duplicated through another package, you must
   use the ``ALTERNATIVE_LINK_NAME`` variable to specify the actual
   location: ALTERNATIVE_LINK_NAME[bracket] = "/usr/bin/["

   In this example, the binary for the ``bracket`` command (i.e. ``[``)
   from the ``busybox`` package resides in ``/usr/bin/``.

      **Note**

      If
      ALTERNATIVE_LINK_NAME
      is not defined, it defaults to
      ${bindir}/
      name
      .

   For more information on the alternatives system, see the
   "```update-alternatives.bbclass`` <#ref-classes-update-alternatives>`__"
   section.

ALTERNATIVE_PRIORITY
   Used by the alternatives system to create default priorities for
   duplicated commands. You can use the variable to create a single
   default regardless of the command name or package, a default for
   specific duplicated commands regardless of the package, or a default
   for specific commands tied to particular packages. Here are the
   available syntax forms: ALTERNATIVE_PRIORITY = "priority"
   ALTERNATIVE_PRIORITY[name] = "priority"
   ALTERNATIVE_PRIORITY_pkg[name] = "priority"

   For more information on the alternatives system, see the
   "```update-alternatives.bbclass`` <#ref-classes-update-alternatives>`__"
   section.

ALTERNATIVE_TARGET
   Used by the alternatives system to create default link locations for
   duplicated commands. You can use the variable to create a single
   default location for all duplicated commands regardless of the
   command name or package, a default for specific duplicated commands
   regardless of the package, or a default for specific commands tied to
   particular packages. Here are the available syntax forms:
   ALTERNATIVE_TARGET = "target" ALTERNATIVE_TARGET[name] = "target"
   ALTERNATIVE_TARGET_pkg[name] = "target"

      **Note**

      If ``ALTERNATIVE_TARGET`` is not defined, it inherits the value
      from the
      ```ALTERNATIVE_LINK_NAME`` <#var-ALTERNATIVE_LINK_NAME>`__
      variable.

      If ``ALTERNATIVE_LINK_NAME`` and ``ALTERNATIVE_TARGET`` are the
      same, the target for ``ALTERNATIVE_TARGET`` has "``.{BPN}``"
      appended to it.

      Finally, if the file referenced has not been renamed, the
      alternatives system will rename it to avoid the need to rename
      alternative files in the ```do_install`` <#ref-tasks-install>`__
      task while retaining support for the command if necessary.

   For more information on the alternatives system, see the
   "```update-alternatives.bbclass`` <#ref-classes-update-alternatives>`__"
   section.

APPEND
   An override list of append strings for each target specified with
   ```LABELS`` <#var-LABELS>`__.

   See the ```grub-efi`` <#ref-classes-grub-efi>`__ class for more
   information on how this variable is used.

AR
   The minimal command and arguments used to run ``ar``.

ARCHIVER_MODE
   When used with the ```archiver`` <#ref-classes-archiver>`__ class,
   determines the type of information used to create a released archive.
   You can use this variable to create archives of patched source,
   original source, configured source, and so forth by employing the
   following variable flags (varflags): ARCHIVER_MODE[src] = "original"
   # Uses original (unpacked) source # files. ARCHIVER_MODE[src] =
   "patched" # Uses patched source files. This is # the default.
   ARCHIVER_MODE[src] = "configured" # Uses configured source files.
   ARCHIVER_MODE[diff] = "1" # Uses patches between do_unpack and #
   do_patch. ARCHIVER_MODE[diff-exclude] ?= "file file ..." # Lists
   files and directories to # exclude from diff. ARCHIVER_MODE[dumpdata]
   = "1" # Uses environment data. ARCHIVER_MODE[recipe] = "1" # Uses
   recipe and include files. ARCHIVER_MODE[srpm] = "1" # Uses RPM
   package files. For information on how the variable works, see the
   ``meta/classes/archiver.bbclass`` file in the `Source
   Directory <#source-directory>`__.

AS
   Minimal command and arguments needed to run the assembler.

ASSUME_PROVIDED
   Lists recipe names (```PN`` <#var-PN>`__ values) BitBake does not
   attempt to build. Instead, BitBake assumes these recipes have already
   been built.

   In OpenEmbedded-Core, ``ASSUME_PROVIDED`` mostly specifies native
   tools that should not be built. An example is ``git-native``, which
   when specified, allows for the Git binary from the host to be used
   rather than building ``git-native``.

ASSUME_SHLIBS
   Provides additional ``shlibs`` provider mapping information, which
   adds to or overwrites the information provided automatically by the
   system. Separate multiple entries using spaces.

   As an example, use the following form to add an ``shlib`` provider of
   shlibname in packagename with the optional version:
   shlibname:packagename[_version]

   Here is an example that adds a shared library named ``libEGL.so.1``
   as being provided by the ``libegl-implementation`` package:
   ASSUME_SHLIBS = "libEGL.so.1:libegl-implementation"

AUTHOR
   The email address used to contact the original author or authors in
   order to send patches and forward bugs.

AUTO_LIBNAME_PKGS
   When the ```debian`` <#ref-classes-debian>`__ class is inherited,
   which is the default behavior, ``AUTO_LIBNAME_PKGS`` specifies which
   packages should be checked for libraries and renamed according to
   Debian library package naming.

   The default value is "${PACKAGES}", which causes the debian class to
   act on all packages that are explicitly generated by the recipe.

AUTO_SYSLINUXMENU
   Enables creating an automatic menu for the syslinux bootloader. You
   must set this variable in your recipe. The
   ```syslinux`` <#ref-classes-syslinux>`__ class checks this variable.

AUTOREV
   When ``SRCREV`` is set to the value of this variable, it specifies to
   use the latest source revision in the repository. Here is an example:
   SRCREV = "${AUTOREV}"

   If you use the previous statement to retrieve the latest version of
   software, you need to be sure ```PV`` <#var-PV>`__ contains
   ``${``\ ```SRCPV`` <#var-SRCPV>`__\ ``}``. For example, suppose you
   have a kernel recipe that inherits the
   `kernel <#ref-classes-kernel>`__ class and you use the previous
   statement. In this example, ``${SRCPV}`` does not automatically get
   into ``PV``. Consequently, you need to change ``PV`` in your recipe
   so that it does contain ``${SRCPV}``.

   For more information see the "`Automatically Incrementing a Binary
   Package Revision
   Number <&YOCTO_DOCS_DEV_URL;#automatically-incrementing-a-binary-package-revision-number>`__"
   section in the Yocto Project Development Tasks Manual.

AVAILABLE_LICENSES
   List of licenses found in the directories specified by
   ```COMMON_LICENSE_DIR`` <#var-COMMON_LICENSE_DIR>`__ and
   ```LICENSE_PATH`` <#var-LICENSE_PATH>`__.

      **Note**

      It is assumed that all changes to
      COMMON_LICENSE_DIR
      and
      LICENSE_PATH
      have been done before
      AVAILABLE_LICENSES
      is defined (in
      license.bbclass
      ).

AVAILTUNES
   The list of defined CPU and Application Binary Interface (ABI)
   tunings (i.e. "tunes") available for use by the OpenEmbedded build
   system.

   The list simply presents the tunes that are available. Not all tunes
   may be compatible with a particular machine configuration, or with
   each other in a
   `Multilib <&YOCTO_DOCS_DEV_URL;#combining-multiple-versions-library-files-into-one-image>`__
   configuration.

   To add a tune to the list, be sure to append it with spaces using the
   "+=" BitBake operator. Do not simply replace the list by using the
   "=" operator. See the "`Basic
   Syntax <&YOCTO_DOCS_BB_URL;#basic-syntax>`__" section in the BitBake
   User Manual for more information.

B
   The directory within the `Build Directory <#build-directory>`__ in
   which the OpenEmbedded build system places generated objects during a
   recipe's build process. By default, this directory is the same as the
   ```S`` <#var-S>`__ directory, which is defined as: S =
   "${WORKDIR}/${BP}"

   You can separate the (``S``) directory and the directory pointed to
   by the ``B`` variable. Most Autotools-based recipes support
   separating these directories. The build system defaults to using
   separate directories for ``gcc`` and some kernel recipes.

BAD_RECOMMENDATIONS
   Lists "recommended-only" packages to not install. Recommended-only
   packages are packages installed only through the
   ```RRECOMMENDS`` <#var-RRECOMMENDS>`__ variable. You can prevent any
   of these "recommended" packages from being installed by listing them
   with the ``BAD_RECOMMENDATIONS`` variable: BAD_RECOMMENDATIONS =
   "package_name package_name package_name ..."

   You can set this variable globally in your ``local.conf`` file or you
   can attach it to a specific image recipe by using the recipe name
   override: BAD_RECOMMENDATIONS_pn-target_image = "package_name"

   It is important to realize that if you choose to not install packages
   using this variable and some other packages are dependent on them
   (i.e. listed in a recipe's ```RDEPENDS`` <#var-RDEPENDS>`__
   variable), the OpenEmbedded build system ignores your request and
   will install the packages to avoid dependency errors.

   Support for this variable exists only when using the IPK and RPM
   packaging backend. Support does not exist for DEB.

   See the ```NO_RECOMMENDATIONS`` <#var-NO_RECOMMENDATIONS>`__ and the
   ```PACKAGE_EXCLUDE`` <#var-PACKAGE_EXCLUDE>`__ variables for related
   information.

BASE_LIB
   The library directory name for the CPU or Application Binary
   Interface (ABI) tune. The ``BASE_LIB`` applies only in the Multilib
   context. See the "`Combining Multiple Versions of Library Files into
   One
   Image <&YOCTO_DOCS_DEV_URL;#combining-multiple-versions-library-files-into-one-image>`__"
   section in the Yocto Project Development Tasks Manual for information
   on Multilib.

   The ``BASE_LIB`` variable is defined in the machine include files in
   the `Source Directory <#source-directory>`__. If Multilib is not
   being used, the value defaults to "lib".

BASE_WORKDIR
   Points to the base of the work directory for all recipes. The default
   value is "${TMPDIR}/work".

BB_ALLOWED_NETWORKS
   Specifies a space-delimited list of hosts that the fetcher is allowed
   to use to obtain the required source code. Following are
   considerations surrounding this variable:

   -  This host list is only used if ``BB_NO_NETWORK`` is either not set
      or set to "0".

   -  Limited support for wildcard matching against the beginning of
      host names exists. For example, the following setting matches
      ``git.gnu.org``, ``ftp.gnu.org``, and ``foo.git.gnu.org``.
      BB_ALLOWED_NETWORKS = "*.gnu.org"

         **Note**

         The use of the "``*``" character only works at the beginning of
         a host name and it must be isolated from the remainder of the
         host name. You cannot use the wildcard character in any other
         location of the name or combined with the front part of the
         name.

         For example, ``*.foo.bar`` is supported, while ``*aa.foo.bar``
         is not.

   -  Mirrors not in the host list are skipped and logged in debug.

   -  Attempts to access networks not in the host list cause a failure.

   Using ``BB_ALLOWED_NETWORKS`` in conjunction with
   ```PREMIRRORS`` <#var-PREMIRRORS>`__ is very useful. Adding the host
   you want to use to ``PREMIRRORS`` results in the source code being
   fetched from an allowed location and avoids raising an error when a
   host that is not allowed is in a ```SRC_URI`` <#var-SRC_URI>`__
   statement. This is because the fetcher does not attempt to use the
   host listed in ``SRC_URI`` after a successful fetch from the
   ``PREMIRRORS`` occurs.

BB_DANGLINGAPPENDS_WARNONLY
   Defines how BitBake handles situations where an append file
   (``.bbappend``) has no corresponding recipe file (``.bb``). This
   condition often occurs when layers get out of sync (e.g. ``oe-core``
   bumps a recipe version and the old recipe no longer exists and the
   other layer has not been updated to the new version of the recipe
   yet).

   The default fatal behavior is safest because it is the sane reaction
   given something is out of sync. It is important to realize when your
   changes are no longer being applied.

   You can change the default behavior by setting this variable to "1",
   "yes", or "true" in your ``local.conf`` file, which is located in the
   `Build Directory <#build-directory>`__: Here is an example:
   BB_DANGLINGAPPENDS_WARNONLY = "1"

BB_DISKMON_DIRS
   Monitors disk space and available inodes during the build and allows
   you to control the build based on these parameters.

   Disk space monitoring is disabled by default. To enable monitoring,
   add the ``BB_DISKMON_DIRS`` variable to your ``conf/local.conf`` file
   found in the `Build Directory <#build-directory>`__. Use the
   following form: BB_DISKMON_DIRS = "action,dir,threshold [...]" where:
   action is: ABORT: Immediately abort the build when a threshold is
   broken. STOPTASKS: Stop the build after the currently executing tasks
   have finished when a threshold is broken. WARN: Issue a warning but
   continue the build when a threshold is broken. Subsequent warnings
   are issued as defined by the BB_DISKMON_WARNINTERVAL variable, which
   must be defined in the conf/local.conf file. dir is: Any directory
   you choose. You can specify one or more directories to monitor by
   separating the groupings with a space. If two directories are on the
   same device, only the first directory is monitored. threshold is:
   Either the minimum available disk space, the minimum number of free
   inodes, or both. You must specify at least one. To omit one or the
   other, simply omit the value. Specify the threshold using G, M, K for
   Gbytes, Mbytes, and Kbytes, respectively. If you do not specify G, M,
   or K, Kbytes is assumed by default. Do not use GB, MB, or KB.

   Here are some examples: BB_DISKMON_DIRS = "ABORT,${TMPDIR},1G,100K
   WARN,${SSTATE_DIR},1G,100K" BB_DISKMON_DIRS =
   "STOPTASKS,${TMPDIR},1G" BB_DISKMON_DIRS = "ABORT,${TMPDIR},,100K"
   The first example works only if you also provide the
   ```BB_DISKMON_WARNINTERVAL`` <#var-BB_DISKMON_WARNINTERVAL>`__
   variable in the ``conf/local.conf``. This example causes the build
   system to immediately abort when either the disk space in
   ``${TMPDIR}`` drops below 1 Gbyte or the available free inodes drops
   below 100 Kbytes. Because two directories are provided with the
   variable, the build system also issue a warning when the disk space
   in the ``${SSTATE_DIR}`` directory drops below 1 Gbyte or the number
   of free inodes drops below 100 Kbytes. Subsequent warnings are issued
   during intervals as defined by the ``BB_DISKMON_WARNINTERVAL``
   variable.

   The second example stops the build after all currently executing
   tasks complete when the minimum disk space in the ``${TMPDIR}``
   directory drops below 1 Gbyte. No disk monitoring occurs for the free
   inodes in this case.

   The final example immediately aborts the build when the number of
   free inodes in the ``${TMPDIR}`` directory drops below 100 Kbytes. No
   disk space monitoring for the directory itself occurs in this case.

BB_DISKMON_WARNINTERVAL
   Defines the disk space and free inode warning intervals. To set these
   intervals, define the variable in your ``conf/local.conf`` file in
   the `Build Directory <#build-directory>`__.

   If you are going to use the ``BB_DISKMON_WARNINTERVAL`` variable, you
   must also use the ```BB_DISKMON_DIRS`` <#var-BB_DISKMON_DIRS>`__
   variable and define its action as "WARN". During the build,
   subsequent warnings are issued each time disk space or number of free
   inodes further reduces by the respective interval.

   If you do not provide a ``BB_DISKMON_WARNINTERVAL`` variable and you
   do use ``BB_DISKMON_DIRS`` with the "WARN" action, the disk
   monitoring interval defaults to the following:
   BB_DISKMON_WARNINTERVAL = "50M,5K"

   When specifying the variable in your configuration file, use the
   following form: BB_DISKMON_WARNINTERVAL =
   "disk_space_interval,disk_inode_interval" where: disk_space_interval
   is: An interval of memory expressed in either G, M, or K for Gbytes,
   Mbytes, or Kbytes, respectively. You cannot use GB, MB, or KB.
   disk_inode_interval is: An interval of free inodes expressed in
   either G, M, or K for Gbytes, Mbytes, or Kbytes, respectively. You
   cannot use GB, MB, or KB.

   Here is an example: BB_DISKMON_DIRS = "WARN,${SSTATE_DIR},1G,100K"
   BB_DISKMON_WARNINTERVAL = "50M,5K" These variables cause the
   OpenEmbedded build system to issue subsequent warnings each time the
   available disk space further reduces by 50 Mbytes or the number of
   free inodes further reduces by 5 Kbytes in the ``${SSTATE_DIR}``
   directory. Subsequent warnings based on the interval occur each time
   a respective interval is reached beyond the initial warning (i.e. 1
   Gbytes and 100 Kbytes).

BB_GENERATE_MIRROR_TARBALLS
   Causes tarballs of the source control repositories (e.g. Git
   repositories), including metadata, to be placed in the
   ```DL_DIR`` <#var-DL_DIR>`__ directory.

   For performance reasons, creating and placing tarballs of these
   repositories is not the default action by the OpenEmbedded build
   system. BB_GENERATE_MIRROR_TARBALLS = "1" Set this variable in your
   ``local.conf`` file in the `Build Directory <#build-directory>`__.

   Once you have the tarballs containing your source files, you can
   clean up your ``DL_DIR`` directory by deleting any Git or other
   source control work directories.

BB_NUMBER_THREADS
   The maximum number of tasks BitBake should run in parallel at any one
   time. The OpenEmbedded build system automatically configures this
   variable to be equal to the number of cores on the build system. For
   example, a system with a dual core processor that also uses
   hyper-threading causes the ``BB_NUMBER_THREADS`` variable to default
   to "4".

   For single socket systems (i.e. one CPU), you should not have to
   override this variable to gain optimal parallelism during builds.
   However, if you have very large systems that employ multiple physical
   CPUs, you might want to make sure the ``BB_NUMBER_THREADS`` variable
   is not set higher than "20".

   For more information on speeding up builds, see the "`Speeding Up a
   Build <&YOCTO_DOCS_DEV_URL;#speeding-up-a-build>`__" section in the
   Yocto Project Development Tasks Manual.

BB_SERVER_TIMEOUT
   Specifies the time (in seconds) after which to unload the BitBake
   server due to inactivity. Set ``BB_SERVER_TIMEOUT`` to determine how
   long the BitBake server stays resident between invocations.

   For example, the following statement in your ``local.conf`` file
   instructs the server to be unloaded after 20 seconds of inactivity:
   BB_SERVER_TIMEOUT = "20" If you want the server to never be unloaded,
   set ``BB_SERVER_TIMEOUT`` to "-1".

BBCLASSEXTEND
   Allows you to extend a recipe so that it builds variants of the
   software. Common variants for recipes exist such as "natives" like
   ``quilt-native``, which is a copy of Quilt built to run on the build
   system; "crosses" such as ``gcc-cross``, which is a compiler built to
   run on the build machine but produces binaries that run on the target
   ```MACHINE`` <#var-MACHINE>`__; "nativesdk", which targets the SDK
   machine instead of ``MACHINE``; and "mulitlibs" in the form
   "``multilib:``\ multilib_name".

   To build a different variant of the recipe with a minimal amount of
   code, it usually is as simple as adding the following to your recipe:
   BBCLASSEXTEND =+ "native nativesdk" BBCLASSEXTEND =+
   "multilib:multilib_name"

      **Note**

      Internally, the ``BBCLASSEXTEND`` mechanism generates recipe
      variants by rewriting variable values and applying overrides such
      as ``_class-native``. For example, to generate a native version of
      a recipe, a ```DEPENDS`` <#var-DEPENDS>`__ on "foo" is rewritten
      to a ``DEPENDS`` on "foo-native".

      Even when using ``BBCLASSEXTEND``, the recipe is only parsed once.
      Parsing once adds some limitations. For example, it is not
      possible to include a different file depending on the variant,
      since ``include`` statements are processed when the recipe is
      parsed.

BBFILE_COLLECTIONS
   Lists the names of configured layers. These names are used to find
   the other ``BBFILE_*`` variables. Typically, each layer will append
   its name to this variable in its ``conf/layer.conf`` file.

BBFILE_PATTERN
   Variable that expands to match files from
   ```BBFILES`` <#var-BBFILES>`__ in a particular layer. This variable
   is used in the ``conf/layer.conf`` file and must be suffixed with the
   name of the specific layer (e.g. ``BBFILE_PATTERN_emenlow``).

BBFILE_PRIORITY
   Assigns the priority for recipe files in each layer.

   This variable is useful in situations where the same recipe appears
   in more than one layer. Setting this variable allows you to
   prioritize a layer against other layers that contain the same recipe
   - effectively letting you control the precedence for the multiple
   layers. The precedence established through this variable stands
   regardless of a recipe's version (```PV`` <#var-PV>`__ variable). For
   example, a layer that has a recipe with a higher ``PV`` value but for
   which the ``BBFILE_PRIORITY`` is set to have a lower precedence still
   has a lower precedence.

   A larger value for the ``BBFILE_PRIORITY`` variable results in a
   higher precedence. For example, the value 6 has a higher precedence
   than the value 5. If not specified, the ``BBFILE_PRIORITY`` variable
   is set based on layer dependencies (see the ``LAYERDEPENDS`` variable
   for more information. The default priority, if unspecified for a
   layer with no dependencies, is the lowest defined priority + 1 (or 1
   if no priorities are defined).

      **Tip**

      You can use the command
      bitbake-layers show-layers
      to list all configured layers along with their priorities.

BBFILES
   A space-separated list of recipe files BitBake uses to build
   software.

   When specifying recipe files, you can pattern match using Python's
   ```glob`` <https://docs.python.org/3/library/glob.html>`__ syntax.
   For details on the syntax, see the documentation by following the
   previous link.

BBFILES_DYNAMIC
   Activates content when identified layers are present. You identify
   the layers by the collections that the layers define.

   Use the ``BBFILES_DYNAMIC`` variable to avoid ``.bbappend`` files
   whose corresponding ``.bb`` file is in a layer that attempts to
   modify other layers through ``.bbappend`` but does not want to
   introduce a hard dependency on those other layers.

   Use the following form for ``BBFILES_DYNAMIC``:
   collection_name:filename_pattern The following example identifies two
   collection names and two filename patterns: BBFILES_DYNAMIC += " \\
   clang-layer:${LAYERDIR}/bbappends/meta-clang/*/*/*.bbappend \\
   core:${LAYERDIR}/bbappends/openembedded-core/meta/*/*/*.bbappend \\ "
   This next example shows an error message that occurs because invalid
   entries are found, which cause parsing to abort: ERROR:
   BBFILES_DYNAMIC entries must be of the form <collection
   name>:<filename pattern>, not:
   /work/my-layer/bbappends/meta-security-isafw/*/*/*.bbappend
   /work/my-layer/bbappends/openembedded-core/meta/*/*/*.bbappend

BBINCLUDELOGS
   Variable that controls how BitBake displays logs on build failure.

BBINCLUDELOGS_LINES
   If ```BBINCLUDELOGS`` <#var-BBINCLUDELOGS>`__ is set, specifies the
   maximum number of lines from the task log file to print when
   reporting a failed task. If you do not set ``BBINCLUDELOGS_LINES``,
   the entire log is printed.

BBLAYERS
   Lists the layers to enable during the build. This variable is defined
   in the ``bblayers.conf`` configuration file in the `Build
   Directory <#build-directory>`__. Here is an example: BBLAYERS = " \\
   /home/scottrif/poky/meta \\ /home/scottrif/poky/meta-poky \\
   /home/scottrif/poky/meta-yocto-bsp \\
   /home/scottrif/poky/meta-mykernel \\ "

   This example enables four layers, one of which is a custom,
   user-defined layer named ``meta-mykernel``.

BBMASK
   Prevents BitBake from processing recipes and recipe append files.

   You can use the ``BBMASK`` variable to "hide" these ``.bb`` and
   ``.bbappend`` files. BitBake ignores any recipe or recipe append
   files that match any of the expressions. It is as if BitBake does not
   see them at all. Consequently, matching files are not parsed or
   otherwise used by BitBake.

   The values you provide are passed to Python's regular expression
   compiler. Consequently, the syntax follows Python's Regular
   Expression (re) syntax. The expressions are compared against the full
   paths to the files. For complete syntax information, see Python's
   documentation at ` <http://docs.python.org/3/library/re.html#re>`__.

   The following example uses a complete regular expression to tell
   BitBake to ignore all recipe and recipe append files in the
   ``meta-ti/recipes-misc/`` directory: BBMASK = "meta-ti/recipes-misc/"
   If you want to mask out multiple directories or recipes, you can
   specify multiple regular expression fragments. This next example
   masks out multiple directories and individual recipes: BBMASK +=
   "/meta-ti/recipes-misc/ meta-ti/recipes-ti/packagegroup/" BBMASK +=
   "/meta-oe/recipes-support/" BBMASK += "/meta-foo/.*/openldap" BBMASK
   += "opencv.*\.bbappend" BBMASK += "lzma"

      **Note**

      When specifying a directory name, use the trailing slash character
      to ensure you match just that directory name.

BBMULTICONFIG
   Specifies each additional separate configuration when you are
   building targets with multiple configurations. Use this variable in
   your ``conf/local.conf`` configuration file. Specify a
   multiconfigname for each configuration file you are using. For
   example, the following line specifies three configuration files:
   BBMULTICONFIG = "configA configB configC" Each configuration file you
   use must reside in the `Build Directory <#build-directory>`__
   ``conf/multiconfig`` directory (e.g.
   build_directory\ ``/conf/multiconfig/configA.conf``).

   For information on how to use ``BBMULTICONFIG`` in an environment
   that supports building targets with multiple configurations, see the
   "`Building Images for Multiple Targets Using Multiple
   Configurations <&YOCTO_DOCS_DEV_URL;#dev-building-images-for-multiple-targets-using-multiple-configurations>`__"
   section in the Yocto Project Development Tasks Manual.

BBPATH
   Used by BitBake to locate ``.bbclass`` and configuration files. This
   variable is analogous to the ``PATH`` variable.

      **Note**

      If you run BitBake from a directory outside of the
      Build Directory
      , you must be sure to set
      BBPATH
      to point to the Build Directory. Set the variable as you would any
      environment variable and then run BitBake:
      ::

              $ BBPATH = "build_directory"
              $ export BBPATH
              $ bitbake target
                                 

BBSERVER
   If defined in the BitBake environment, ``BBSERVER`` points to the
   BitBake remote server.

   Use the following format to export the variable to the BitBake
   environment: export BBSERVER=localhost:$port

   By default, ``BBSERVER`` also appears in
   ```BB_HASHBASE_WHITELIST`` <&YOCTO_DOCS_BB_URL;#var-BB_HASHBASE_WHITELIST>`__.
   Consequently, ``BBSERVER`` is excluded from checksum and dependency
   data.

BINCONFIG
   When inheriting the
   ```binconfig-disabled`` <#ref-classes-binconfig-disabled>`__ class,
   this variable specifies binary configuration scripts to disable in
   favor of using ``pkg-config`` to query the information. The
   ``binconfig-disabled`` class will modify the specified scripts to
   return an error so that calls to them can be easily found and
   replaced.

   To add multiple scripts, separate them by spaces. Here is an example
   from the ``libpng`` recipe: BINCONFIG = "${bindir}/libpng-config
   ${bindir}/libpng16-config"

BINCONFIG_GLOB
   When inheriting the ```binconfig`` <#ref-classes-binconfig>`__ class,
   this variable specifies a wildcard for configuration scripts that
   need editing. The scripts are edited to correct any paths that have
   been set up during compilation so that they are correct for use when
   installed into the sysroot and called by the build processes of other
   recipes.

      **Note**

      The
      BINCONFIG_GLOB
      variable uses
      shell globbing
      , which is recognition and expansion of wildcards during pattern
      matching. Shell globbing is very similar to
      fnmatch
      and
      glob
      .

   For more information on how this variable works, see
   ``meta/classes/binconfig.bbclass`` in the `Source
   Directory <#source-directory>`__. You can also find general
   information on the class in the
   "```binconfig.bbclass`` <#ref-classes-binconfig>`__" section.

BP
   The base recipe name and version but without any special recipe name
   suffix (i.e. ``-native``, ``lib64-``, and so forth). ``BP`` is
   comprised of the following: ${BPN}-${PV}

BPN
   This variable is a version of the ```PN`` <#var-PN>`__ variable with
   common prefixes and suffixes removed, such as ``nativesdk-``,
   ``-cross``, ``-native``, and multilib's ``lib64-`` and ``lib32-``.
   The exact lists of prefixes and suffixes removed are specified by the
   ```MLPREFIX`` <#var-MLPREFIX>`__ and
   ```SPECIAL_PKGSUFFIX`` <#var-SPECIAL_PKGSUFFIX>`__ variables,
   respectively.

BUGTRACKER
   Specifies a URL for an upstream bug tracking website for a recipe.
   The OpenEmbedded build system does not use this variable. Rather, the
   variable is a useful pointer in case a bug in the software being
   built needs to be manually reported.

BUILD_ARCH
   Specifies the architecture of the build host (e.g. ``i686``). The
   OpenEmbedded build system sets the value of ``BUILD_ARCH`` from the
   machine name reported by the ``uname`` command.

BUILD_AS_ARCH
   Specifies the architecture-specific assembler flags for the build
   host. By default, the value of ``BUILD_AS_ARCH`` is empty.

BUILD_CC_ARCH
   Specifies the architecture-specific C compiler flags for the build
   host. By default, the value of ``BUILD_CC_ARCH`` is empty.

BUILD_CCLD
   Specifies the linker command to be used for the build host when the C
   compiler is being used as the linker. By default, ``BUILD_CCLD``
   points to GCC and passes as arguments the value of
   ```BUILD_CC_ARCH`` <#var-BUILD_CC_ARCH>`__, assuming
   ``BUILD_CC_ARCH`` is set.

BUILD_CFLAGS
   Specifies the flags to pass to the C compiler when building for the
   build host. When building in the ``-native`` context,
   ```CFLAGS`` <#var-CFLAGS>`__ is set to the value of this variable by
   default.

BUILD_CPPFLAGS
   Specifies the flags to pass to the C preprocessor (i.e. to both the C
   and the C++ compilers) when building for the build host. When
   building in the ``-native`` context, ```CPPFLAGS`` <#var-CPPFLAGS>`__
   is set to the value of this variable by default.

BUILD_CXXFLAGS
   Specifies the flags to pass to the C++ compiler when building for the
   build host. When building in the ``-native`` context,
   ```CXXFLAGS`` <#var-CXXFLAGS>`__ is set to the value of this variable
   by default.

BUILD_FC
   Specifies the Fortran compiler command for the build host. By
   default, ``BUILD_FC`` points to Gfortran and passes as arguments the
   value of ```BUILD_CC_ARCH`` <#var-BUILD_CC_ARCH>`__, assuming
   ``BUILD_CC_ARCH`` is set.

BUILD_LD
   Specifies the linker command for the build host. By default,
   ``BUILD_LD`` points to the GNU linker (ld) and passes as arguments
   the value of ```BUILD_LD_ARCH`` <#var-BUILD_LD_ARCH>`__, assuming
   ``BUILD_LD_ARCH`` is set.

BUILD_LD_ARCH
   Specifies architecture-specific linker flags for the build host. By
   default, the value of ``BUILD_LD_ARCH`` is empty.

BUILD_LDFLAGS
   Specifies the flags to pass to the linker when building for the build
   host. When building in the ``-native`` context,
   ```LDFLAGS`` <#var-LDFLAGS>`__ is set to the value of this variable
   by default.

BUILD_OPTIMIZATION
   Specifies the optimization flags passed to the C compiler when
   building for the build host or the SDK. The flags are passed through
   the ```BUILD_CFLAGS`` <#var-BUILD_CFLAGS>`__ and
   ```BUILDSDK_CFLAGS`` <#var-BUILDSDK_CFLAGS>`__ default values.

   The default value of the ``BUILD_OPTIMIZATION`` variable is "-O2
   -pipe".

BUILD_OS
   Specifies the operating system in use on the build host (e.g.
   "linux"). The OpenEmbedded build system sets the value of
   ``BUILD_OS`` from the OS reported by the ``uname`` command - the
   first word, converted to lower-case characters.

BUILD_PREFIX
   The toolchain binary prefix used for native recipes. The OpenEmbedded
   build system uses the ``BUILD_PREFIX`` value to set the
   ```TARGET_PREFIX`` <#var-TARGET_PREFIX>`__ when building for
   ``native`` recipes.

BUILD_STRIP
   Specifies the command to be used to strip debugging symbols from
   binaries produced for the build host. By default, ``BUILD_STRIP``
   points to
   ``${``\ ```BUILD_PREFIX`` <#var-BUILD_PREFIX>`__\ ``}strip``.

BUILD_SYS
   Specifies the system, including the architecture and the operating
   system, to use when building for the build host (i.e. when building
   ``native`` recipes).

   The OpenEmbedded build system automatically sets this variable based
   on ```BUILD_ARCH`` <#var-BUILD_ARCH>`__,
   ```BUILD_VENDOR`` <#var-BUILD_VENDOR>`__, and
   ```BUILD_OS`` <#var-BUILD_OS>`__. You do not need to set the
   ``BUILD_SYS`` variable yourself.

BUILD_VENDOR
   Specifies the vendor name to use when building for the build host.
   The default value is an empty string ("").

BUILDDIR
   Points to the location of the `Build Directory <#build-directory>`__.
   You can define this directory indirectly through the
   ````` <#structure-core-script>`__ script by passing in a Build
   Directory path when you run the script. If you run the script and do
   not provide a Build Directory path, the ``BUILDDIR`` defaults to
   ``build`` in the current directory.

BUILDHISTORY_COMMIT
   When inheriting the ```buildhistory`` <#ref-classes-buildhistory>`__
   class, this variable specifies whether or not to commit the build
   history output in a local Git repository. If set to "1", this local
   repository will be maintained automatically by the ``buildhistory``
   class and a commit will be created on every build for changes to each
   top-level subdirectory of the build history output (images, packages,
   and sdk). If you want to track changes to build history over time,
   you should set this value to "1".

   By default, the ``buildhistory`` class does not commit the build
   history output in a local Git repository: BUILDHISTORY_COMMIT ?= "0"

BUILDHISTORY_COMMIT_AUTHOR
   When inheriting the ```buildhistory`` <#ref-classes-buildhistory>`__
   class, this variable specifies the author to use for each Git commit.
   In order for the ``BUILDHISTORY_COMMIT_AUTHOR`` variable to work, the
   ```BUILDHISTORY_COMMIT`` <#var-BUILDHISTORY_COMMIT>`__ variable must
   be set to "1".

   Git requires that the value you provide for the
   ``BUILDHISTORY_COMMIT_AUTHOR`` variable takes the form of "name
   email@host". Providing an email address or host that is not valid
   does not produce an error.

   By default, the ``buildhistory`` class sets the variable as follows:
   BUILDHISTORY_COMMIT_AUTHOR ?= "buildhistory <buildhistory@${DISTRO}>"

BUILDHISTORY_DIR
   When inheriting the ```buildhistory`` <#ref-classes-buildhistory>`__
   class, this variable specifies the directory in which build history
   information is kept. For more information on how the variable works,
   see the ``buildhistory.class``.

   By default, the ``buildhistory`` class sets the directory as follows:
   BUILDHISTORY_DIR ?= "${TOPDIR}/buildhistory"

BUILDHISTORY_FEATURES
   When inheriting the ```buildhistory`` <#ref-classes-buildhistory>`__
   class, this variable specifies the build history features to be
   enabled. For more information on how build history works, see the
   "`Maintaining Build Output
   Quality <&YOCTO_DOCS_DEV_URL;#maintaining-build-output-quality>`__"
   section in the Yocto Project Development Tasks Manual.

   You can specify these features in the form of a space-separated list:

   -  *image:* Analysis of the contents of images, which includes the
      list of installed packages among other things.

   -  *package:* Analysis of the contents of individual packages.

   -  *sdk:* Analysis of the contents of the software development kit
      (SDK).

   -  *task:* Save output file signatures for `shared
      state <&YOCTO_DOCS_OM_URL;#shared-state-cache>`__ (sstate) tasks.
      This saves one file per task and lists the SHA-256 checksums for
      each file staged (i.e. the output of the task).

   By default, the ``buildhistory`` class enables the following
   features: BUILDHISTORY_FEATURES ?= "image package sdk"

BUILDHISTORY_IMAGE_FILES
   When inheriting the ```buildhistory`` <#ref-classes-buildhistory>`__
   class, this variable specifies a list of paths to files copied from
   the image contents into the build history directory under an
   "image-files" directory in the directory for the image, so that you
   can track the contents of each file. The default is to copy
   ``/etc/passwd`` and ``/etc/group``, which allows you to monitor for
   changes in user and group entries. You can modify the list to include
   any file. Specifying an invalid path does not produce an error.
   Consequently, you can include files that might not always be present.

   By default, the ``buildhistory`` class provides paths to the
   following files: BUILDHISTORY_IMAGE_FILES ?= "/etc/passwd /etc/group"

BUILDHISTORY_PUSH_REPO
   When inheriting the ```buildhistory`` <#ref-classes-buildhistory>`__
   class, this variable optionally specifies a remote repository to
   which build history pushes Git changes. In order for
   ``BUILDHISTORY_PUSH_REPO`` to work,
   ```BUILDHISTORY_COMMIT`` <#var-BUILDHISTORY_COMMIT>`__ must be set to
   "1".

   The repository should correspond to a remote address that specifies a
   repository as understood by Git, or alternatively to a remote name
   that you have set up manually using ``git remote`` within the local
   repository.

   By default, the ``buildhistory`` class sets the variable as follows:
   BUILDHISTORY_PUSH_REPO ?= ""

BUILDSDK_CFLAGS
   Specifies the flags to pass to the C compiler when building for the
   SDK. When building in the ``nativesdk-`` context,
   ```CFLAGS`` <#var-CFLAGS>`__ is set to the value of this variable by
   default.

BUILDSDK_CPPFLAGS
   Specifies the flags to pass to the C pre-processor (i.e. to both the
   C and the C++ compilers) when building for the SDK. When building in
   the ``nativesdk-`` context, ```CPPFLAGS`` <#var-CPPFLAGS>`__ is set
   to the value of this variable by default.

BUILDSDK_CXXFLAGS
   Specifies the flags to pass to the C++ compiler when building for the
   SDK. When building in the ``nativesdk-`` context,
   ```CXXFLAGS`` <#var-CXXFLAGS>`__ is set to the value of this variable
   by default.

BUILDSDK_LDFLAGS
   Specifies the flags to pass to the linker when building for the SDK.
   When building in the ``nativesdk-`` context,
   ```LDFLAGS`` <#var-LDFLAGS>`__ is set to the value of this variable
   by default.

BUILDSTATS_BASE
   Points to the location of the directory that holds build statistics
   when you use and enable the
   ```buildstats`` <#ref-classes-buildstats>`__ class. The
   ``BUILDSTATS_BASE`` directory defaults to
   ``${``\ ```TMPDIR`` <#var-TMPDIR>`__\ ``}/buildstats/``.

BUSYBOX_SPLIT_SUID
   For the BusyBox recipe, specifies whether to split the output
   executable file into two parts: one for features that require
   ``setuid root``, and one for the remaining features (i.e. those that
   do not require ``setuid root``).

   The ``BUSYBOX_SPLIT_SUID`` variable defaults to "1", which results in
   splitting the output executable file. Set the variable to "0" to get
   a single output executable file.

CACHE
   Specifies the directory BitBake uses to store a cache of the
   `Metadata <#metadata>`__ so it does not need to be parsed every time
   BitBake is started.

CC
   The minimal command and arguments used to run the C compiler.

CFLAGS
   Specifies the flags to pass to the C compiler. This variable is
   exported to an environment variable and thus made visible to the
   software being built during the compilation step.

   Default initialization for ``CFLAGS`` varies depending on what is
   being built:

   -  ```TARGET_CFLAGS`` <#var-TARGET_CFLAGS>`__ when building for the
      target

   -  ```BUILD_CFLAGS`` <#var-BUILD_CFLAGS>`__ when building for the
      build host (i.e. ``-native``)

   -  ```BUILDSDK_CFLAGS`` <#var-BUILDSDK_CFLAGS>`__ when building for
      an SDK (i.e. ``nativesdk-``)

CLASSOVERRIDE
   An internal variable specifying the special class override that
   should currently apply (e.g. "class-target", "class-native", and so
   forth). The classes that use this variable (e.g.
   ```native`` <#ref-classes-native>`__,
   ```nativesdk`` <#ref-classes-nativesdk>`__, and so forth) set the
   variable to appropriate values.

      **Note**

      CLASSOVERRIDE
      gets its default "class-target" value from the
      bitbake.conf
      file.

   As an example, the following override allows you to install extra
   files, but only when building for the target:
   do_install_append_class-target() { install my-extra-file
   ${D}${sysconfdir} } Here is an example where ``FOO`` is set to
   "native" when building for the build host, and to "other" when not
   building for the build host: FOO_class-native = "native" FOO =
   "other" The underlying mechanism behind ``CLASSOVERRIDE`` is simply
   that it is included in the default value of
   ```OVERRIDES`` <#var-OVERRIDES>`__.

CLEANBROKEN
   If set to "1" within a recipe, ``CLEANBROKEN`` specifies that the
   ``make clean`` command does not work for the software being built.
   Consequently, the OpenEmbedded build system will not try to run
   ``make clean`` during the ```do_configure`` <#ref-tasks-configure>`__
   task, which is the default behavior.

COMBINED_FEATURES
   Provides a list of hardware features that are enabled in both
   ```MACHINE_FEATURES`` <#var-MACHINE_FEATURES>`__ and
   ```DISTRO_FEATURES`` <#var-DISTRO_FEATURES>`__. This select list of
   features contains features that make sense to be controlled both at
   the machine and distribution configuration level. For example, the
   "bluetooth" feature requires hardware support but should also be
   optional at the distribution level, in case the hardware supports
   Bluetooth but you do not ever intend to use it.

COMMON_LICENSE_DIR
   Points to ``meta/files/common-licenses`` in the `Source
   Directory <#source-directory>`__, which is where generic license
   files reside.

COMPATIBLE_HOST
   A regular expression that resolves to one or more hosts (when the
   recipe is native) or one or more targets (when the recipe is
   non-native) with which a recipe is compatible. The regular expression
   is matched against ```HOST_SYS`` <#var-HOST_SYS>`__. You can use the
   variable to stop recipes from being built for classes of systems with
   which the recipes are not compatible. Stopping these builds is
   particularly useful with kernels. The variable also helps to increase
   parsing speed since the build system skips parsing recipes not
   compatible with the current system.

COMPATIBLE_MACHINE
   A regular expression that resolves to one or more target machines
   with which a recipe is compatible. The regular expression is matched
   against ```MACHINEOVERRIDES`` <#var-MACHINEOVERRIDES>`__. You can use
   the variable to stop recipes from being built for machines with which
   the recipes are not compatible. Stopping these builds is particularly
   useful with kernels. The variable also helps to increase parsing
   speed since the build system skips parsing recipes not compatible
   with the current machine.

COMPLEMENTARY_GLOB
   Defines wildcards to match when installing a list of complementary
   packages for all the packages explicitly (or implicitly) installed in
   an image.

      **Note**

      The
      COMPLEMENTARY_GLOB
      variable uses Unix filename pattern matching (
      fnmatch
      ), which is similar to the Unix style pathname pattern expansion (
      glob
      ).

   The resulting list of complementary packages is associated with an
   item that can be added to
   ```IMAGE_FEATURES`` <#var-IMAGE_FEATURES>`__. An example usage of
   this is the "dev-pkgs" item that when added to ``IMAGE_FEATURES``
   will install -dev packages (containing headers and other development
   files) for every package in the image.

   To add a new feature item pointing to a wildcard, use a variable flag
   to specify the feature item name and use the value to specify the
   wildcard. Here is an example: COMPLEMENTARY_GLOB[dev-pkgs] = '*-dev'

COMPONENTS_DIR
   Stores sysroot components for each recipe. The OpenEmbedded build
   system uses ``COMPONENTS_DIR`` when constructing recipe-specific
   sysroots for other recipes.

   The default is
   "``${``\ ```STAGING_DIR`` <#var-STAGING_DIR>`__\ ``}-components``."
   (i.e.
   "``${``\ ```TMPDIR`` <#var-TMPDIR>`__\ ``}/sysroots-components``").

CONF_VERSION
   Tracks the version of the local configuration file (i.e.
   ``local.conf``). The value for ``CONF_VERSION`` increments each time
   ``build/conf/`` compatibility changes.

CONFFILES
   Identifies editable or configurable files that are part of a package.
   If the Package Management System (PMS) is being used to update
   packages on the target system, it is possible that configuration
   files you have changed after the original installation and that you
   now want to remain unchanged are overwritten. In other words,
   editable files might exist in the package that you do not want reset
   as part of the package update process. You can use the ``CONFFILES``
   variable to list the files in the package that you wish to prevent
   the PMS from overwriting during this update process.

   To use the ``CONFFILES`` variable, provide a package name override
   that identifies the resulting package. Then, provide a
   space-separated list of files. Here is an example: CONFFILES_${PN} +=
   "${sysconfdir}/file1 \\ ${sysconfdir}/file2 ${sysconfdir}/file3"

   A relationship exists between the ``CONFFILES`` and ``FILES``
   variables. The files listed within ``CONFFILES`` must be a subset of
   the files listed within ``FILES``. Because the configuration files
   you provide with ``CONFFILES`` are simply being identified so that
   the PMS will not overwrite them, it makes sense that the files must
   already be included as part of the package through the ``FILES``
   variable.

      **Note**

      When specifying paths as part of the
      CONFFILES
      variable, it is good practice to use appropriate path variables.
      For example,
      ${sysconfdir}
      rather than
      /etc
      or
      ${bindir}
      rather than
      /usr/bin
      . You can find a list of these variables at the top of the
      meta/conf/bitbake.conf
      file in the
      Source Directory
      .

CONFIG_INITRAMFS_SOURCE
   Identifies the initial RAM filesystem (initramfs) source files. The
   OpenEmbedded build system receives and uses this kernel Kconfig
   variable as an environment variable. By default, the variable is set
   to null ("").

   The ``CONFIG_INITRAMFS_SOURCE`` can be either a single cpio archive
   with a ``.cpio`` suffix or a space-separated list of directories and
   files for building the initramfs image. A cpio archive should contain
   a filesystem archive to be used as an initramfs image. Directories
   should contain a filesystem layout to be included in the initramfs
   image. Files should contain entries according to the format described
   by the ``usr/gen_init_cpio`` program in the kernel tree.

   If you specify multiple directories and files, the initramfs image
   will be the aggregate of all of them.

   For information on creating an initramfs, see the "`Building an
   Initial RAM Filesystem (initramfs)
   Image <&YOCTO_DOCS_DEV_URL;#building-an-initramfs-image>`__" section
   in the Yocto Project Development Tasks Manual.

CONFIG_SITE
   A list of files that contains ``autoconf`` test results relevant to
   the current build. This variable is used by the Autotools utilities
   when running ``configure``.

CONFIGURE_FLAGS
   The minimal arguments for GNU configure.

CONFLICT_DISTRO_FEATURES
   When inheriting the
   ```distro_features_check`` <#ref-classes-distro_features_check>`__
   class, this variable identifies distribution features that would be
   in conflict should the recipe be built. In other words, if the
   ``CONFLICT_DISTRO_FEATURES`` variable lists a feature that also
   appears in ``DISTRO_FEATURES`` within the current configuration, an
   error occurs and the build stops.

COPYLEFT_LICENSE_EXCLUDE
   A space-separated list of licenses to exclude from the source
   archived by the ```archiver`` <#ref-classes-archiver>`__ class. In
   other words, if a license in a recipe's
   ```LICENSE`` <#var-LICENSE>`__ value is in the value of
   ``COPYLEFT_LICENSE_EXCLUDE``, then its source is not archived by the
   class.

      **Note**

      The
      COPYLEFT_LICENSE_EXCLUDE
      variable takes precedence over the
      COPYLEFT_LICENSE_INCLUDE
      variable.

   The default value, which is "CLOSED Proprietary", for
   ``COPYLEFT_LICENSE_EXCLUDE`` is set by the
   ```copyleft_filter`` <#ref-classes-copyleft_filter>`__ class, which
   is inherited by the ``archiver`` class.

COPYLEFT_LICENSE_INCLUDE
   A space-separated list of licenses to include in the source archived
   by the ```archiver`` <#ref-classes-archiver>`__ class. In other
   words, if a license in a recipe's ```LICENSE`` <#var-LICENSE>`__
   value is in the value of ``COPYLEFT_LICENSE_INCLUDE``, then its
   source is archived by the class.

   The default value is set by the
   ```copyleft_filter`` <#ref-classes-copyleft_filter>`__ class, which
   is inherited by the ``archiver`` class. The default value includes
   "GPL*", "LGPL*", and "AGPL*".

COPYLEFT_PN_EXCLUDE
   A list of recipes to exclude in the source archived by the
   ```archiver`` <#ref-classes-archiver>`__ class. The
   ``COPYLEFT_PN_EXCLUDE`` variable overrides the license inclusion and
   exclusion caused through the
   ```COPYLEFT_LICENSE_INCLUDE`` <#var-COPYLEFT_LICENSE_INCLUDE>`__ and
   ```COPYLEFT_LICENSE_EXCLUDE`` <#var-COPYLEFT_LICENSE_EXCLUDE>`__
   variables, respectively.

   The default value, which is "" indicating to not explicitly exclude
   any recipes by name, for ``COPYLEFT_PN_EXCLUDE`` is set by the
   ```copyleft_filter`` <#ref-classes-copyleft_filter>`__ class, which
   is inherited by the ``archiver`` class.

COPYLEFT_PN_INCLUDE
   A list of recipes to include in the source archived by the
   ```archiver`` <#ref-classes-archiver>`__ class. The
   ``COPYLEFT_PN_INCLUDE`` variable overrides the license inclusion and
   exclusion caused through the
   ```COPYLEFT_LICENSE_INCLUDE`` <#var-COPYLEFT_LICENSE_INCLUDE>`__ and
   ```COPYLEFT_LICENSE_EXCLUDE`` <#var-COPYLEFT_LICENSE_EXCLUDE>`__
   variables, respectively.

   The default value, which is "" indicating to not explicitly include
   any recipes by name, for ``COPYLEFT_PN_INCLUDE`` is set by the
   ```copyleft_filter`` <#ref-classes-copyleft_filter>`__ class, which
   is inherited by the ``archiver`` class.

COPYLEFT_RECIPE_TYPES
   A space-separated list of recipe types to include in the source
   archived by the ```archiver`` <#ref-classes-archiver>`__ class.
   Recipe types are ``target``, ``native``, ``nativesdk``, ``cross``,
   ``crosssdk``, and ``cross-canadian``.

   The default value, which is "target*", for ``COPYLEFT_RECIPE_TYPES``
   is set by the ```copyleft_filter`` <#ref-classes-copyleft_filter>`__
   class, which is inherited by the ``archiver`` class.

COPY_LIC_DIRS
   If set to "1" along with the
   ```COPY_LIC_MANIFEST`` <#var-COPY_LIC_MANIFEST>`__ variable, the
   OpenEmbedded build system copies into the image the license files,
   which are located in ``/usr/share/common-licenses``, for each
   package. The license files are placed in directories within the image
   itself during build time.

      **Note**

      The
      COPY_LIC_DIRS
      does not offer a path for adding licenses for newly installed
      packages to an image, which might be most suitable for read-only
      filesystems that cannot be upgraded. See the
      LICENSE_CREATE_PACKAGE
      variable for additional information. You can also reference the "
      Providing License Text
      " section in the Yocto Project Development Tasks Manual for
      information on providing license text.

COPY_LIC_MANIFEST
   If set to "1", the OpenEmbedded build system copies the license
   manifest for the image to
   ``/usr/share/common-licenses/license.manifest`` within the image
   itself during build time.

      **Note**

      The
      COPY_LIC_MANIFEST
      does not offer a path for adding licenses for newly installed
      packages to an image, which might be most suitable for read-only
      filesystems that cannot be upgraded. See the
      LICENSE_CREATE_PACKAGE
      variable for additional information. You can also reference the "
      Providing License Text
      " section in the Yocto Project Development Tasks Manual for
      information on providing license text.

CORE_IMAGE_EXTRA_INSTALL
   Specifies the list of packages to be added to the image. You should
   only set this variable in the ``local.conf`` configuration file found
   in the `Build Directory <#build-directory>`__.

   This variable replaces ``POKY_EXTRA_INSTALL``, which is no longer
   supported.

COREBASE
   Specifies the parent directory of the OpenEmbedded-Core Metadata
   layer (i.e. ``meta``).

   It is an important distinction that ``COREBASE`` points to the parent
   of this layer and not the layer itself. Consider an example where you
   have cloned the Poky Git repository and retained the ``poky`` name
   for your local copy of the repository. In this case, ``COREBASE``
   points to the ``poky`` folder because it is the parent directory of
   the ``poky/meta`` layer.

COREBASE_FILES
   Lists files from the ```COREBASE`` <#var-COREBASE>`__ directory that
   should be copied other than the layers listed in the
   ``bblayers.conf`` file. The ``COREBASE_FILES`` variable exists for
   the purpose of copying metadata from the OpenEmbedded build system
   into the extensible SDK.

   Explicitly listing files in ``COREBASE`` is needed because it
   typically contains build directories and other files that should not
   normally be copied into the extensible SDK. Consequently, the value
   of ``COREBASE_FILES`` is used in order to only copy the files that
   are actually needed.

CPP
   The minimal command and arguments used to run the C preprocessor.

CPPFLAGS
   Specifies the flags to pass to the C pre-processor (i.e. to both the
   C and the C++ compilers). This variable is exported to an environment
   variable and thus made visible to the software being built during the
   compilation step.

   Default initialization for ``CPPFLAGS`` varies depending on what is
   being built:

   -  ```TARGET_CPPFLAGS`` <#var-TARGET_CPPFLAGS>`__ when building for
      the target

   -  ```BUILD_CPPFLAGS`` <#var-BUILD_CPPFLAGS>`__ when building for the
      build host (i.e. ``-native``)

   -  ```BUILDSDK_CPPFLAGS`` <#var-BUILDSDK_CPPFLAGS>`__ when building
      for an SDK (i.e. ``nativesdk-``)

CROSS_COMPILE
   The toolchain binary prefix for the target tools. The
   ``CROSS_COMPILE`` variable is the same as the
   ```TARGET_PREFIX`` <#var-TARGET_PREFIX>`__ variable.

      **Note**

      The OpenEmbedded build system sets the
      CROSS_COMPILE
      variable only in certain contexts (e.g. when building for kernel
      and kernel module recipes).

CVSDIR
   The directory in which files checked out under the CVS system are
   stored.

CXX
   The minimal command and arguments used to run the C++ compiler.

CXXFLAGS
   Specifies the flags to pass to the C++ compiler. This variable is
   exported to an environment variable and thus made visible to the
   software being built during the compilation step.

   Default initialization for ``CXXFLAGS`` varies depending on what is
   being built:

   -  ```TARGET_CXXFLAGS`` <#var-TARGET_CXXFLAGS>`__ when building for
      the target

   -  ```BUILD_CXXFLAGS`` <#var-BUILD_CXXFLAGS>`__ when building for the
      build host (i.e. ``-native``)

   -  ```BUILDSDK_CXXFLAGS`` <#var-BUILDSDK_CXXFLAGS>`__ when building
      for an SDK (i.e. ``nativesdk-``)

D
   The destination directory. The location in the `Build
   Directory <#build-directory>`__ where components are installed by the
   ```do_install`` <#ref-tasks-install>`__ task. This location defaults
   to: ${WORKDIR}/image

      **Note**

      Tasks that read from or write to this directory should run under
      fakeroot
      .

DATE
   The date the build was started. Dates appear using the year, month,
   and day (YMD) format (e.g. "20150209" for February 9th, 2015).

DATETIME
   The date and time on which the current build started. The format is
   suitable for timestamps.

DEBIAN_NOAUTONAME
   When the ```debian`` <#ref-classes-debian>`__ class is inherited,
   which is the default behavior, ``DEBIAN_NOAUTONAME`` specifies a
   particular package should not be renamed according to Debian library
   package naming. You must use the package name as an override when you
   set this variable. Here is an example from the ``fontconfig`` recipe:
   DEBIAN_NOAUTONAME_fontconfig-utils = "1"

DEBIANNAME
   When the ```debian`` <#ref-classes-debian>`__ class is inherited,
   which is the default behavior, ``DEBIANNAME`` allows you to override
   the library name for an individual package. Overriding the library
   name in these cases is rare. You must use the package name as an
   override when you set this variable. Here is an example from the
   ``dbus`` recipe: DEBIANNAME_${PN} = "dbus-1"

DEBUG_BUILD
   Specifies to build packages with debugging information. This
   influences the value of the ``SELECTED_OPTIMIZATION`` variable.

DEBUG_OPTIMIZATION
   The options to pass in ``TARGET_CFLAGS`` and ``CFLAGS`` when
   compiling a system for debugging. This variable defaults to "-O
   -fno-omit-frame-pointer ${DEBUG_FLAGS} -pipe".

DEFAULT_PREFERENCE
   Specifies a weak bias for recipe selection priority.

   The most common usage of this is variable is to set it to "-1" within
   a recipe for a development version of a piece of software. Using the
   variable in this way causes the stable version of the recipe to build
   by default in the absence of ``PREFERRED_VERSION`` being used to
   build the development version.

      **Note**

      The bias provided by
      DEFAULT_PREFERENCE
      is weak and is overridden by
      BBFILE_PRIORITY
      if that variable is different between two layers that contain
      different versions of the same recipe.

DEFAULTTUNE
   The default CPU and Application Binary Interface (ABI) tunings (i.e.
   the "tune") used by the OpenEmbedded build system. The
   ``DEFAULTTUNE`` helps define
   ```TUNE_FEATURES`` <#var-TUNE_FEATURES>`__.

   The default tune is either implicitly or explicitly set by the
   machine (```MACHINE`` <#var-MACHINE>`__). However, you can override
   the setting using available tunes as defined with
   ```AVAILTUNES`` <#var-AVAILTUNES>`__.

DEPENDS
   Lists a recipe's build-time dependencies. These are dependencies on
   other recipes whose contents (e.g. headers and shared libraries) are
   needed by the recipe at build time.

   As an example, consider a recipe ``foo`` that contains the following
   assignment: DEPENDS = "bar" The practical effect of the previous
   assignment is that all files installed by bar will be available in
   the appropriate staging sysroot, given by the
   ```STAGING_DIR*`` <#var-STAGING_DIR>`__ variables, by the time the
   ```do_configure`` <#ref-tasks-configure>`__ task for ``foo`` runs.
   This mechanism is implemented by having ``do_configure`` depend on
   the ```do_populate_sysroot`` <#ref-tasks-populate_sysroot>`__ task of
   each recipe listed in ``DEPENDS``, through a
   ``[``\ ```deptask`` <&YOCTO_DOCS_BB_URL;#variable-flags>`__\ ``]``
   declaration in the ```base`` <#ref-classes-base>`__ class.

      **Note**

      It seldom is necessary to reference, for example,
      STAGING_DIR_HOST
      explicitly. The standard classes and build-related variables are
      configured to automatically use the appropriate staging sysroots.

   As another example, ``DEPENDS`` can also be used to add utilities
   that run on the build machine during the build. For example, a recipe
   that makes use of a code generator built by the recipe ``codegen``
   might have the following: DEPENDS = "codegen-native" For more
   information, see the ```native`` <#ref-classes-native>`__ class and
   the ```EXTRANATIVEPATH`` <#var-EXTRANATIVEPATH>`__ variable.

      **Note**

      -  ``DEPENDS`` is a list of recipe names. Or, to be more precise,
         it is a list of ```PROVIDES`` <#var-PROVIDES>`__ names, which
         usually match recipe names. Putting a package name such as
         "foo-dev" in ``DEPENDS`` does not make sense. Use "foo"
         instead, as this will put files from all the packages that make
         up ``foo``, which includes those from ``foo-dev``, into the
         sysroot.

      -  One recipe having another recipe in ``DEPENDS`` does not by
         itself add any runtime dependencies between the packages
         produced by the two recipes. However, as explained in the
         "`Automatically Added Runtime
         Dependencies <&YOCTO_DOCS_OM_URL;#automatically-added-runtime-dependencies>`__"
         section in the Yocto Project Overview and Concepts Manual,
         runtime dependencies will often be added automatically, meaning
         ``DEPENDS`` alone is sufficient for most recipes.

      -  Counterintuitively, ``DEPENDS`` is often necessary even for
         recipes that install precompiled components. For example, if
         ``libfoo`` is a precompiled library that links against
         ``libbar``, then linking against ``libfoo`` requires both
         ``libfoo`` and ``libbar`` to be available in the sysroot.
         Without a ``DEPENDS`` from the recipe that installs ``libfoo``
         to the recipe that installs ``libbar``, other recipes might
         fail to link against ``libfoo``.

   For information on runtime dependencies, see the
   ```RDEPENDS`` <#var-RDEPENDS>`__ variable. You can also see the
   "`Tasks <&YOCTO_DOCS_BB_URL;#tasks>`__" and
   "`Dependencies <&YOCTO_DOCS_BB_URL;#dependencies>`__" sections in the
   BitBake User Manual for additional information on tasks and
   dependencies.

DEPLOY_DIR
   Points to the general area that the OpenEmbedded build system uses to
   place images, packages, SDKs, and other output files that are ready
   to be used outside of the build system. By default, this directory
   resides within the `Build Directory <#build-directory>`__ as
   ``${TMPDIR}/deploy``.

   For more information on the structure of the Build Directory, see
   "`The Build Directory - ``build/`` <#structure-build>`__" section.
   For more detail on the contents of the ``deploy`` directory, see the
   "`Images <&YOCTO_DOCS_OM_URL;#images-dev-environment>`__", "`Package
   Feeds <&YOCTO_DOCS_OM_URL;#package-feeds-dev-environment>`__", and
   "`Application Development
   SDK <&YOCTO_DOCS_OM_URL;#sdk-dev-environment>`__" sections all in the
   Yocto Project Overview and Concepts Manual.

DEPLOY_DIR_DEB
   Points to the area that the OpenEmbedded build system uses to place
   Debian packages that are ready to be used outside of the build
   system. This variable applies only when
   ```PACKAGE_CLASSES`` <#var-PACKAGE_CLASSES>`__ contains
   "package_deb".

   The BitBake configuration file initially defines the
   ``DEPLOY_DIR_DEB`` variable as a sub-folder of
   ```DEPLOY_DIR`` <#var-DEPLOY_DIR>`__: DEPLOY_DIR_DEB =
   "${DEPLOY_DIR}/deb"

   The ```package_deb`` <#ref-classes-package_deb>`__ class uses the
   ``DEPLOY_DIR_DEB`` variable to make sure the
   ```do_package_write_deb`` <#ref-tasks-package_write_deb>`__ task
   writes Debian packages into the appropriate folder. For more
   information on how packaging works, see the "`Package
   Feeds <&YOCTO_DOCS_OM_URL;#package-feeds-dev-environment>`__" section
   in the Yocto Project Overview and Concepts Manual.

DEPLOY_DIR_IMAGE
   Points to the area that the OpenEmbedded build system uses to place
   images and other associated output files that are ready to be
   deployed onto the target machine. The directory is machine-specific
   as it contains the ``${MACHINE}`` name. By default, this directory
   resides within the `Build Directory <#build-directory>`__ as
   ``${DEPLOY_DIR}/images/${MACHINE}/``.

   For more information on the structure of the Build Directory, see
   "`The Build Directory - ``build/`` <#structure-build>`__" section.
   For more detail on the contents of the ``deploy`` directory, see the
   "`Images <&YOCTO_DOCS_OM_URL;#images-dev-environment>`__" and
   "`Application Development
   SDK <&YOCTO_DOCS_OM_URL;#sdk-dev-environment>`__" sections both in
   the Yocto Project Overview and Concepts Manual.

DEPLOY_DIR_IPK
   Points to the area that the OpenEmbedded build system uses to place
   IPK packages that are ready to be used outside of the build system.
   This variable applies only when
   ```PACKAGE_CLASSES`` <#var-PACKAGE_CLASSES>`__ contains
   "package_ipk".

   The BitBake configuration file initially defines this variable as a
   sub-folder of ```DEPLOY_DIR`` <#var-DEPLOY_DIR>`__: DEPLOY_DIR_IPK =
   "${DEPLOY_DIR}/ipk"

   The ```package_ipk`` <#ref-classes-package_ipk>`__ class uses the
   ``DEPLOY_DIR_IPK`` variable to make sure the
   ```do_package_write_ipk`` <#ref-tasks-package_write_ipk>`__ task
   writes IPK packages into the appropriate folder. For more information
   on how packaging works, see the "`Package
   Feeds <&YOCTO_DOCS_OM_URL;#package-feeds-dev-environment>`__" section
   in the Yocto Project Overview and Concepts Manual.

DEPLOY_DIR_RPM
   Points to the area that the OpenEmbedded build system uses to place
   RPM packages that are ready to be used outside of the build system.
   This variable applies only when
   ```PACKAGE_CLASSES`` <#var-PACKAGE_CLASSES>`__ contains
   "package_rpm".

   The BitBake configuration file initially defines this variable as a
   sub-folder of ```DEPLOY_DIR`` <#var-DEPLOY_DIR>`__: DEPLOY_DIR_RPM =
   "${DEPLOY_DIR}/rpm"

   The ```package_rpm`` <#ref-classes-package_rpm>`__ class uses the
   ``DEPLOY_DIR_RPM`` variable to make sure the
   ```do_package_write_rpm`` <#ref-tasks-package_write_rpm>`__ task
   writes RPM packages into the appropriate folder. For more information
   on how packaging works, see the "`Package
   Feeds <&YOCTO_DOCS_OM_URL;#package-feeds-dev-environment>`__" section
   in the Yocto Project Overview and Concepts Manual.

DEPLOY_DIR_TAR
   Points to the area that the OpenEmbedded build system uses to place
   tarballs that are ready to be used outside of the build system. This
   variable applies only when
   ```PACKAGE_CLASSES`` <#var-PACKAGE_CLASSES>`__ contains
   "package_tar".

   The BitBake configuration file initially defines this variable as a
   sub-folder of ```DEPLOY_DIR`` <#var-DEPLOY_DIR>`__: DEPLOY_DIR_TAR =
   "${DEPLOY_DIR}/tar"

   The ```package_tar`` <#ref-classes-package_tar>`__ class uses the
   ``DEPLOY_DIR_TAR`` variable to make sure the
   ```do_package_write_tar`` <#ref-tasks-package_write_tar>`__ task
   writes TAR packages into the appropriate folder. For more information
   on how packaging works, see the "`Package
   Feeds <&YOCTO_DOCS_OM_URL;#package-feeds-dev-environment>`__" section
   in the Yocto Project Overview and Concepts Manual.

DEPLOYDIR
   When inheriting the ```deploy`` <#ref-classes-deploy>`__ class, the
   ``DEPLOYDIR`` points to a temporary work area for deployed files that
   is set in the ``deploy`` class as follows: DEPLOYDIR =
   "${WORKDIR}/deploy-${```PN`` <#var-PN>`__}"

   Recipes inheriting the ``deploy`` class should copy files to be
   deployed into ``DEPLOYDIR``, and the class will take care of copying
   them into ```DEPLOY_DIR_IMAGE`` <#var-DEPLOY_DIR_IMAGE>`__
   afterwards.

DESCRIPTION
   The package description used by package managers. If not set,
   ``DESCRIPTION`` takes the value of the ```SUMMARY`` <#var-SUMMARY>`__
   variable.

DISTRO
   The short name of the distribution. For information on the long name
   of the distribution, see the ```DISTRO_NAME`` <#var-DISTRO_NAME>`__
   variable.

   The ``DISTRO`` variable corresponds to a distribution configuration
   file whose root name is the same as the variable's argument and whose
   filename extension is ``.conf``. For example, the distribution
   configuration file for the Poky distribution is named ``poky.conf``
   and resides in the ``meta-poky/conf/distro`` directory of the `Source
   Directory <#source-directory>`__.

   Within that ``poky.conf`` file, the ``DISTRO`` variable is set as
   follows: DISTRO = "poky"

   Distribution configuration files are located in a ``conf/distro``
   directory within the `Metadata <#metadata>`__ that contains the
   distribution configuration. The value for ``DISTRO`` must not contain
   spaces, and is typically all lower-case.

      **Note**

      If the
      DISTRO
      variable is blank, a set of default configurations are used, which
      are specified within
      meta/conf/distro/defaultsetup.conf
      also in the Source Directory.

DISTRO_CODENAME
   Specifies a codename for the distribution being built.

DISTRO_EXTRA_RDEPENDS
   Specifies a list of distro-specific packages to add to all images.
   This variable takes affect through ``packagegroup-base`` so the
   variable only really applies to the more full-featured images that
   include ``packagegroup-base``. You can use this variable to keep
   distro policy out of generic images. As with all other distro
   variables, you set this variable in the distro ``.conf`` file.

DISTRO_EXTRA_RRECOMMENDS
   Specifies a list of distro-specific packages to add to all images if
   the packages exist. The packages might not exist or be empty (e.g.
   kernel modules). The list of packages are automatically installed but
   you can remove them.

DISTRO_FEATURES
   The software support you want in your distribution for various
   features. You define your distribution features in the distribution
   configuration file.

   In most cases, the presence or absence of a feature in
   ``DISTRO_FEATURES`` is translated to the appropriate option supplied
   to the configure script during the
   ```do_configure`` <#ref-tasks-configure>`__ task for recipes that
   optionally support the feature. For example, specifying "x11" in
   ``DISTRO_FEATURES``, causes every piece of software built for the
   target that can optionally support X11 to have its X11 support
   enabled.

   Two more examples are Bluetooth and NFS support. For a more complete
   list of features that ships with the Yocto Project and that you can
   provide with this variable, see the "`Distro
   Features <#ref-features-distro>`__" section.

DISTRO_FEATURES_BACKFILL
   Features to be added to ``DISTRO_FEATURES`` if not also present in
   ``DISTRO_FEATURES_BACKFILL_CONSIDERED``.

   This variable is set in the ``meta/conf/bitbake.conf`` file. It is
   not intended to be user-configurable. It is best to just reference
   the variable to see which distro features are being backfilled for
   all distro configurations. See the "`Feature
   Backfilling <#ref-features-backfill>`__" section for more
   information.

DISTRO_FEATURES_BACKFILL_CONSIDERED
   Features from ``DISTRO_FEATURES_BACKFILL`` that should not be
   backfilled (i.e. added to ``DISTRO_FEATURES``) during the build. See
   the "`Feature Backfilling <#ref-features-backfill>`__" section for
   more information.

DISTRO_FEATURES_DEFAULT
   A convenience variable that gives you the default list of distro
   features with the exception of any features specific to the C library
   (``libc``).

   When creating a custom distribution, you might find it useful to be
   able to reuse the default
   ```DISTRO_FEATURES`` <#var-DISTRO_FEATURES>`__ options without the
   need to write out the full set. Here is an example that uses
   ``DISTRO_FEATURES_DEFAULT`` from a custom distro configuration file:
   DISTRO_FEATURES ?= "${DISTRO_FEATURES_DEFAULT} myfeature"

DISTRO_FEATURES_FILTER_NATIVE
   Specifies a list of features that if present in the target
   ```DISTRO_FEATURES`` <#var-DISTRO_FEATURES>`__ value should be
   included in ``DISTRO_FEATURES`` when building native recipes. This
   variable is used in addition to the features filtered using the
   ```DISTRO_FEATURES_NATIVE`` <#var-DISTRO_FEATURES_NATIVE>`__
   variable.

DISTRO_FEATURES_FILTER_NATIVESDK
   Specifies a list of features that if present in the target
   ```DISTRO_FEATURES`` <#var-DISTRO_FEATURES>`__ value should be
   included in ``DISTRO_FEATURES`` when building nativesdk recipes. This
   variable is used in addition to the features filtered using the
   ```DISTRO_FEATURES_NATIVESDK`` <#var-DISTRO_FEATURES_NATIVESDK>`__
   variable.

DISTRO_FEATURES_NATIVE
   Specifies a list of features that should be included in
   ```DISTRO_FEATURES`` <#var-DISTRO_FEATURES>`__ when building native
   recipes. This variable is used in addition to the features filtered
   using the
   ```DISTRO_FEATURES_FILTER_NATIVE`` <#var-DISTRO_FEATURES_FILTER_NATIVE>`__
   variable.

DISTRO_FEATURES_NATIVESDK
   Specifies a list of features that should be included in
   ```DISTRO_FEATURES`` <#var-DISTRO_FEATURES>`__ when building
   nativesdk recipes. This variable is used in addition to the features
   filtered using the
   ```DISTRO_FEATURES_FILTER_NATIVESDK`` <#var-DISTRO_FEATURES_FILTER_NATIVESDK>`__
   variable.

DISTRO_NAME
   The long name of the distribution. For information on the short name
   of the distribution, see the ```DISTRO`` <#var-DISTRO>`__ variable.

   The ``DISTRO_NAME`` variable corresponds to a distribution
   configuration file whose root name is the same as the variable's
   argument and whose filename extension is ``.conf``. For example, the
   distribution configuration file for the Poky distribution is named
   ``poky.conf`` and resides in the ``meta-poky/conf/distro`` directory
   of the `Source Directory <#source-directory>`__.

   Within that ``poky.conf`` file, the ``DISTRO_NAME`` variable is set
   as follows: DISTRO_NAME = "Poky (Yocto Project Reference Distro)"

   Distribution configuration files are located in a ``conf/distro``
   directory within the `Metadata <#metadata>`__ that contains the
   distribution configuration.

      **Note**

      If the
      DISTRO_NAME
      variable is blank, a set of default configurations are used, which
      are specified within
      meta/conf/distro/defaultsetup.conf
      also in the Source Directory.

DISTRO_VERSION
   The version of the distribution.

DISTROOVERRIDES
   A colon-separated list of overrides specific to the current
   distribution. By default, this list includes the value of
   ```DISTRO`` <#var-DISTRO>`__.

   You can extend ``DISTROOVERRIDES`` to add extra overrides that should
   apply to the distribution.

   The underlying mechanism behind ``DISTROOVERRIDES`` is simply that it
   is included in the default value of
   ```OVERRIDES`` <#var-OVERRIDES>`__.

DL_DIR
   The central download directory used by the build process to store
   downloads. By default, ``DL_DIR`` gets files suitable for mirroring
   for everything except Git repositories. If you want tarballs of Git
   repositories, use the
   ```BB_GENERATE_MIRROR_TARBALLS`` <#var-BB_GENERATE_MIRROR_TARBALLS>`__
   variable.

   You can set this directory by defining the ``DL_DIR`` variable in the
   ``conf/local.conf`` file. This directory is self-maintaining and you
   should not have to touch it. By default, the directory is
   ``downloads`` in the `Build Directory <#build-directory>`__. #DL_DIR
   ?= "${TOPDIR}/downloads" To specify a different download directory,
   simply remove the comment from the line and provide your directory.

   During a first build, the system downloads many different source code
   tarballs from various upstream projects. Downloading can take a
   while, particularly if your network connection is slow. Tarballs are
   all stored in the directory defined by ``DL_DIR`` and the build
   system looks there first to find source tarballs.

      **Note**

      When wiping and rebuilding, you can preserve this directory to
      speed up this part of subsequent builds.

   You can safely share this directory between multiple builds on the
   same development machine. For additional information on how the build
   process gets source files when working behind a firewall or proxy
   server, see this specific question in the
   "`FAQ <#how-does-the-yocto-project-obtain-source-code-and-will-it-work-behind-my-firewall-or-proxy-server>`__"
   chapter. You can also refer to the "`Working Behind a Network
   Proxy <&YOCTO_WIKI_URL;/wiki/Working_Behind_a_Network_Proxy>`__" Wiki
   page.

DOC_COMPRESS
   When inheriting the ```compress_doc`` <#ref-classes-compress_doc>`__
   class, this variable sets the compression policy used when the
   OpenEmbedded build system compresses man pages and info pages. By
   default, the compression method used is gz (gzip). Other policies
   available are xz and bz2.

   For information on policies and on how to use this variable, see the
   comments in the ``meta/classes/compress_doc.bbclass`` file.

EFI_PROVIDER
   When building bootable images (i.e. where ``hddimg``, ``iso``, or
   ``wic.vmdk`` is in ```IMAGE_FSTYPES`` <#var-IMAGE_FSTYPES>`__), the
   ``EFI_PROVIDER`` variable specifies the EFI bootloader to use. The
   default is "grub-efi", but "systemd-boot" can be used instead.

   See the ```systemd-boot`` <#ref-classes-systemd-boot>`__ and
   ```image-live`` <#ref-classes-image-live>`__ classes for more
   information.

ENABLE_BINARY_LOCALE_GENERATION
   Variable that controls which locales for ``glibc`` are generated
   during the build (useful if the target device has 64Mbytes of RAM or
   less).

ERR_REPORT_DIR
   When used with the ```report-error`` <#ref-classes-report-error>`__
   class, specifies the path used for storing the debug files created by
   the `error reporting
   tool <&YOCTO_DOCS_DEV_URL;#using-the-error-reporting-tool>`__, which
   allows you to submit build errors you encounter to a central
   database. By default, the value of this variable is
   ``${``\ ```LOG_DIR`` <#var-LOG_DIR>`__\ ``}/error-report``.

   You can set ``ERR_REPORT_DIR`` to the path you want the error
   reporting tool to store the debug files as follows in your
   ``local.conf`` file: ERR_REPORT_DIR = "path"

ERROR_QA
   Specifies the quality assurance checks whose failures are reported as
   errors by the OpenEmbedded build system. You set this variable in
   your distribution configuration file. For a list of the checks you
   can control with this variable, see the
   "```insane.bbclass`` <#ref-classes-insane>`__" section.

EXCLUDE_FROM_SHLIBS
   Triggers the OpenEmbedded build system's shared libraries resolver to
   exclude an entire package when scanning for shared libraries.

      **Note**

      The shared libraries resolver's functionality results in part from
      the internal function
      package_do_shlibs
      , which is part of the
      do_package
      task. You should be aware that the shared libraries resolver might
      implicitly define some dependencies between packages.

   The ``EXCLUDE_FROM_SHLIBS`` variable is similar to the
   ```PRIVATE_LIBS`` <#var-PRIVATE_LIBS>`__ variable, which excludes a
   package's particular libraries only and not the whole package.

   Use the ``EXCLUDE_FROM_SHLIBS`` variable by setting it to "1" for a
   particular package: EXCLUDE_FROM_SHLIBS = "1"

EXCLUDE_FROM_WORLD
   Directs BitBake to exclude a recipe from world builds (i.e.
   ``bitbake world``). During world builds, BitBake locates, parses and
   builds all recipes found in every layer exposed in the
   ``bblayers.conf`` configuration file.

   To exclude a recipe from a world build using this variable, set the
   variable to "1" in the recipe.

      **Note**

      Recipes added to
      EXCLUDE_FROM_WORLD
      may still be built during a world build in order to satisfy
      dependencies of other recipes. Adding a recipe to
      EXCLUDE_FROM_WORLD
      only ensures that the recipe is not explicitly added to the list
      of build targets in a world build.

EXTENDPE
   Used with file and pathnames to create a prefix for a recipe's
   version based on the recipe's ```PE`` <#var-PE>`__ value. If ``PE``
   is set and greater than zero for a recipe, ``EXTENDPE`` becomes that
   value (e.g if ``PE`` is equal to "1" then ``EXTENDPE`` becomes "1_").
   If a recipe's ``PE`` is not set (the default) or is equal to zero,
   ``EXTENDPE`` becomes "".

   See the ```STAMP`` <#var-STAMP>`__ variable for an example.

EXTENDPKGV
   The full package version specification as it appears on the final
   packages produced by a recipe. The variable's value is normally used
   to fix a runtime dependency to the exact same version of another
   package in the same recipe: RDEPENDS_${PN}-additional-module = "${PN}
   (= ${EXTENDPKGV})"

   The dependency relationships are intended to force the package
   manager to upgrade these types of packages in lock-step.

EXTERNAL_KERNEL_TOOLS
   When set, the ``EXTERNAL_KERNEL_TOOLS`` variable indicates that these
   tools are not in the source tree.

   When kernel tools are available in the tree, they are preferred over
   any externally installed tools. Setting the ``EXTERNAL_KERNEL_TOOLS``
   variable tells the OpenEmbedded build system to prefer the installed
   external tools. See the
   ```kernel-yocto`` <#ref-classes-kernel-yocto>`__ class in
   ``meta/classes`` to see how the variable is used.

EXTERNALSRC
   When inheriting the ```externalsrc`` <#ref-classes-externalsrc>`__
   class, this variable points to the source tree, which is outside of
   the OpenEmbedded build system. When set, this variable sets the
   ```S`` <#var-S>`__ variable, which is what the OpenEmbedded build
   system uses to locate unpacked recipe source code.

   For more information on ``externalsrc.bbclass``, see the
   "```externalsrc.bbclass`` <#ref-classes-externalsrc>`__" section. You
   can also find information on how to use this variable in the
   "`Building Software from an External
   Source <&YOCTO_DOCS_DEV_URL;#building-software-from-an-external-source>`__"
   section in the Yocto Project Development Tasks Manual.

EXTERNALSRC_BUILD
   When inheriting the ```externalsrc`` <#ref-classes-externalsrc>`__
   class, this variable points to the directory in which the recipe's
   source code is built, which is outside of the OpenEmbedded build
   system. When set, this variable sets the ```B`` <#var-B>`__ variable,
   which is what the OpenEmbedded build system uses to locate the Build
   Directory.

   For more information on ``externalsrc.bbclass``, see the
   "```externalsrc.bbclass`` <#ref-classes-externalsrc>`__" section. You
   can also find information on how to use this variable in the
   "`Building Software from an External
   Source <&YOCTO_DOCS_DEV_URL;#building-software-from-an-external-source>`__"
   section in the Yocto Project Development Tasks Manual.

EXTRA_AUTORECONF
   For recipes inheriting the ```autotools`` <#ref-classes-autotools>`__
   class, you can use ``EXTRA_AUTORECONF`` to specify extra options to
   pass to the ``autoreconf`` command that is executed during the
   ```do_configure`` <#ref-tasks-configure>`__ task.

   The default value is "--exclude=autopoint".

EXTRA_IMAGE_FEATURES
   A list of additional features to include in an image. When listing
   more than one feature, separate them with a space.

   Typically, you configure this variable in your ``local.conf`` file,
   which is found in the `Build Directory <#build-directory>`__.
   Although you can use this variable from within a recipe, best
   practices dictate that you do not.

      **Note**

      To enable primary features from within the image recipe, use the
      IMAGE_FEATURES
      variable.

   Here are some examples of features you can add: "dbg-pkgs" - Adds
   -dbg packages for all installed packages including symbol information
   for debugging and profiling. "debug-tweaks" - Makes an image suitable
   for debugging. For example, allows root logins without passwords and
   enables post-installation logging. See the 'allow-empty-password' and
   'post-install-logging' features in the "`Image
   Features <#ref-features-image>`__" section for more information.
   "dev-pkgs" - Adds -dev packages for all installed packages. This is
   useful if you want to develop against the libraries in the image.
   "read-only-rootfs" - Creates an image whose root filesystem is
   read-only. See the "`Creating a Read-Only Root
   Filesystem <&YOCTO_DOCS_DEV_URL;#creating-a-read-only-root-filesystem>`__"
   section in the Yocto Project Development Tasks Manual for more
   information "tools-debug" - Adds debugging tools such as gdb and
   strace. "tools-sdk" - Adds development tools such as gcc, make,
   pkgconfig and so forth. "tools-testapps" - Adds useful testing tools
   such as ts_print, aplay, arecord and so forth.

   For a complete list of image features that ships with the Yocto
   Project, see the "`Image Features <#ref-features-image>`__" section.

   For an example that shows how to customize your image by using this
   variable, see the "`Customizing Images Using Custom
   ``IMAGE_FEATURES`` and
   ``EXTRA_IMAGE_FEATURES`` <&YOCTO_DOCS_DEV_URL;#usingpoky-extend-customimage-imagefeatures>`__"
   section in the Yocto Project Development Tasks Manual.

EXTRA_IMAGECMD
   Specifies additional options for the image creation command that has
   been specified in ```IMAGE_CMD`` <#var-IMAGE_CMD>`__. When setting
   this variable, use an override for the associated image type. Here is
   an example: EXTRA_IMAGECMD_ext3 ?= "-i 4096"

EXTRA_IMAGEDEPENDS
   A list of recipes to build that do not provide packages for
   installing into the root filesystem.

   Sometimes a recipe is required to build the final image but is not
   needed in the root filesystem. You can use the ``EXTRA_IMAGEDEPENDS``
   variable to list these recipes and thus specify the dependencies. A
   typical example is a required bootloader in a machine configuration.

      **Note**

      To add packages to the root filesystem, see the various
      \*
      RDEPENDS
      and
      \*
      RRECOMMENDS
      variables.

EXTRANATIVEPATH
   A list of subdirectories of
   ``${``\ ```STAGING_BINDIR_NATIVE`` <#var-STAGING_BINDIR_NATIVE>`__\ ``}``
   added to the beginning of the environment variable ``PATH``. As an
   example, the following prepends
   "${STAGING_BINDIR_NATIVE}/foo:${STAGING_BINDIR_NATIVE}/bar:" to
   ``PATH``: EXTRANATIVEPATH = "foo bar"

EXTRA_OECMAKE
   Additional `CMake <https://cmake.org/overview/>`__ options. See the
   ```cmake`` <#ref-classes-cmake>`__ class for additional information.

EXTRA_OECONF
   Additional ``configure`` script options. See
   ```PACKAGECONFIG_CONFARGS`` <#var-PACKAGECONFIG_CONFARGS>`__ for
   additional information on passing configure script options.

EXTRA_OEMAKE
   Additional GNU ``make`` options.

   Because the ``EXTRA_OEMAKE`` defaults to "", you need to set the
   variable to specify any required GNU options.

   ```PARALLEL_MAKE`` <#var-PARALLEL_MAKE>`__ and
   ```PARALLEL_MAKEINST`` <#var-PARALLEL_MAKEINST>`__ also make use of
   ``EXTRA_OEMAKE`` to pass the required flags.

EXTRA_OESCONS
   When inheriting the ```scons`` <#ref-classes-scons>`__ class, this
   variable specifies additional configuration options you want to pass
   to the ``scons`` command line.

EXTRA_USERS_PARAMS
   When inheriting the ```extrausers`` <#ref-classes-extrausers>`__
   class, this variable provides image level user and group operations.
   This is a more global method of providing user and group
   configuration as compared to using the
   ```useradd`` <#ref-classes-useradd>`__ class, which ties user and
   group configurations to a specific recipe.

   The set list of commands you can configure using the
   ``EXTRA_USERS_PARAMS`` is shown in the ``extrausers`` class. These
   commands map to the normal Unix commands of the same names: #
   EXTRA_USERS_PARAMS = "\\ # useradd -p '' tester; \\ # groupadd
   developers; \\ # userdel nobody; \\ # groupdel -g video; \\ #
   groupmod -g 1020 developers; \\ # usermod -s /bin/sh tester; \\ # "

FEATURE_PACKAGES
   Defines one or more packages to include in an image when a specific
   item is included in ```IMAGE_FEATURES`` <#var-IMAGE_FEATURES>`__.
   When setting the value, ``FEATURE_PACKAGES`` should have the name of
   the feature item as an override. Here is an example:
   FEATURE_PACKAGES_widget = "package1 package2"

   In this example, if "widget" were added to ``IMAGE_FEATURES``,
   package1 and package2 would be included in the image.

      **Note**

      Packages installed by features defined through
      FEATURE_PACKAGES
      are often package groups. While similarly named, you should not
      confuse the
      FEATURE_PACKAGES
      variable with package groups, which are discussed elsewhere in the
      documentation.

FEED_DEPLOYDIR_BASE_URI
   Points to the base URL of the server and location within the
   document-root that provides the metadata and packages required by
   OPKG to support runtime package management of IPK packages. You set
   this variable in your ``local.conf`` file.

   Consider the following example: FEED_DEPLOYDIR_BASE_URI =
   "http://192.168.7.1/BOARD-dir" This example assumes you are serving
   your packages over HTTP and your databases are located in a directory
   named ``BOARD-dir``, which is underneath your HTTP server's
   document-root. In this case, the OpenEmbedded build system generates
   a set of configuration files for you in your target that work with
   the feed.

FILES
   The list of files and directories that are placed in a package. The
   ```PACKAGES`` <#var-PACKAGES>`__ variable lists the packages
   generated by a recipe.

   To use the ``FILES`` variable, provide a package name override that
   identifies the resulting package. Then, provide a space-separated
   list of files or paths that identify the files you want included as
   part of the resulting package. Here is an example: FILES_${PN} +=
   "${bindir}/mydir1 ${bindir}/mydir2/myfile"

      **Note**

      -  When specifying files or paths, you can pattern match using
         Python's
         ```glob`` <https://docs.python.org/2/library/glob.html>`__
         syntax. For details on the syntax, see the documentation by
         following the previous link.

      -  When specifying paths as part of the ``FILES`` variable, it is
         good practice to use appropriate path variables. For example,
         use ``${sysconfdir}`` rather than ``/etc``, or ``${bindir}``
         rather than ``/usr/bin``. You can find a list of these
         variables at the top of the ``meta/conf/bitbake.conf`` file in
         the `Source Directory <#source-directory>`__. You will also
         find the default values of the various ``FILES_*`` variables in
         this file.

   If some of the files you provide with the ``FILES`` variable are
   editable and you know they should not be overwritten during the
   package update process by the Package Management System (PMS), you
   can identify these files so that the PMS will not overwrite them. See
   the ```CONFFILES`` <#var-CONFFILES>`__ variable for information on
   how to identify these files to the PMS.

FILES_SOLIBSDEV
   Defines the file specification to match
   ```SOLIBSDEV`` <#var-SOLIBSDEV>`__. In other words,
   ``FILES_SOLIBSDEV`` defines the full path name of the development
   symbolic link (symlink) for shared libraries on the target platform.

   The following statement from the ``bitbake.conf`` shows how it is
   set: FILES_SOLIBSDEV ?= "${base_libdir}/lib*${SOLIBSDEV}
   ${libdir}/lib*${SOLIBSDEV}"

FILESEXTRAPATHS
   Extends the search path the OpenEmbedded build system uses when
   looking for files and patches as it processes recipes and append
   files. The default directories BitBake uses when it processes recipes
   are initially defined by the ```FILESPATH`` <#var-FILESPATH>`__
   variable. You can extend ``FILESPATH`` variable by using
   ``FILESEXTRAPATHS``.

   Best practices dictate that you accomplish this by using
   ``FILESEXTRAPATHS`` from within a ``.bbappend`` file and that you
   prepend paths as follows: FILESEXTRAPATHS_prepend :=
   "${THISDIR}/${PN}:" In the above example, the build system first
   looks for files in a directory that has the same name as the
   corresponding append file.

      **Note**

      When extending ``FILESEXTRAPATHS``, be sure to use the immediate
      expansion (``:=``) operator. Immediate expansion makes sure that
      BitBake evaluates ```THISDIR`` <#var-THISDIR>`__ at the time the
      directive is encountered rather than at some later time when
      expansion might result in a directory that does not contain the
      files you need.

      Also, include the trailing separating colon character if you are
      prepending. The trailing colon character is necessary because you
      are directing BitBake to extend the path by prepending directories
      to the search path.

   Here is another common use: FILESEXTRAPATHS_prepend :=
   "${THISDIR}/files:" In this example, the build system extends the
   ``FILESPATH`` variable to include a directory named ``files`` that is
   in the same directory as the corresponding append file.

   This next example specifically adds three paths:
   FILESEXTRAPATHS_prepend := "path_1:path_2:path_3:"

   A final example shows how you can extend the search path and include
   a ```MACHINE`` <#var-MACHINE>`__-specific override, which is useful
   in a BSP layer: FILESEXTRAPATHS_prepend_intel-x86-common :=
   "${THISDIR}/${PN}:" The previous statement appears in the
   ``linux-yocto-dev.bbappend`` file, which is found in the Yocto
   Project `Source
   Repositories <&YOCTO_DOCS_OM_URL;#source-repositories>`__ in
   ``meta-intel/common/recipes-kernel/linux``. Here, the machine
   override is a special ```PACKAGE_ARCH`` <#var-PACKAGE_ARCH>`__
   definition for multiple ``meta-intel`` machines.

      **Note**

      For a layer that supports a single BSP, the override could just be
      the value of
      MACHINE
      .

   By prepending paths in ``.bbappend`` files, you allow multiple append
   files that reside in different layers but are used for the same
   recipe to correctly extend the path.

FILESOVERRIDES
   A subset of ```OVERRIDES`` <#var-OVERRIDES>`__ used by the
   OpenEmbedded build system for creating
   ```FILESPATH`` <#var-FILESPATH>`__. The ``FILESOVERRIDES`` variable
   uses overrides to automatically extend the
   ```FILESPATH`` <#var-FILESPATH>`__ variable. For an example of how
   that works, see the ```FILESPATH`` <#var-FILESPATH>`__ variable
   description. Additionally, you find more information on how overrides
   are handled in the "`Conditional Syntax
   (Overrides) <&YOCTO_DOCS_BB_URL;#conditional-syntax-overrides>`__"
   section of the BitBake User Manual.

   By default, the ``FILESOVERRIDES`` variable is defined as:
   FILESOVERRIDES =
   "${TRANSLATED_TARGET_ARCH}:${MACHINEOVERRIDES}:${DISTROOVERRIDES}"

      **Note**

      Do not hand-edit the
      FILESOVERRIDES
      variable. The values match up with expected overrides and are used
      in an expected manner by the build system.

FILESPATH
   The default set of directories the OpenEmbedded build system uses
   when searching for patches and files.

   During the build process, BitBake searches each directory in
   ``FILESPATH`` in the specified order when looking for files and
   patches specified by each ``file://`` URI in a recipe's
   ```SRC_URI`` <#var-SRC_URI>`__ statements.

   The default value for the ``FILESPATH`` variable is defined in the
   ``base.bbclass`` class found in ``meta/classes`` in the `Source
   Directory <#source-directory>`__: FILESPATH =
   "${@base_set_filespath(["${FILE_DIRNAME}/${BP}", \\
   "${FILE_DIRNAME}/${BPN}", "${FILE_DIRNAME}/files"], d)}" The
   ``FILESPATH`` variable is automatically extended using the overrides
   from the ```FILESOVERRIDES`` <#var-FILESOVERRIDES>`__ variable.

      **Note**

      -  Do not hand-edit the ``FILESPATH`` variable. If you want the
         build system to look in directories other than the defaults,
         extend the ``FILESPATH`` variable by using the
         ```FILESEXTRAPATHS`` <#var-FILESEXTRAPATHS>`__ variable.

      -  Be aware that the default ``FILESPATH`` directories do not map
         to directories in custom layers where append files
         (``.bbappend``) are used. If you want the build system to find
         patches or files that reside with your append files, you need
         to extend the ``FILESPATH`` variable by using the
         ``FILESEXTRAPATHS`` variable.

   You can take advantage of this searching behavior in useful ways. For
   example, consider a case where the following directory structure
   exists for general and machine-specific configurations:
   files/defconfig files/MACHINEA/defconfig files/MACHINEB/defconfig
   Also in the example, the ``SRC_URI`` statement contains
   "file://defconfig". Given this scenario, you can set
   ```MACHINE`` <#var-MACHINE>`__ to "MACHINEA" and cause the build
   system to use files from ``files/MACHINEA``. Set ``MACHINE`` to
   "MACHINEB" and the build system uses files from ``files/MACHINEB``.
   Finally, for any machine other than "MACHINEA" and "MACHINEB", the
   build system uses files from ``files/defconfig``.

   You can find out more about the patching process in the
   "`Patching <&YOCTO_DOCS_OM_URL;#patching-dev-environment>`__" section
   in the Yocto Project Overview and Concepts Manual and the "`Patching
   Code <&YOCTO_DOCS_DEV_URL;#new-recipe-patching-code>`__" section in
   the Yocto Project Development Tasks Manual. See the
   ```do_patch`` <#ref-tasks-patch>`__ task as well.

FILESYSTEM_PERMS_TABLES
   Allows you to define your own file permissions settings table as part
   of your configuration for the packaging process. For example, suppose
   you need a consistent set of custom permissions for a set of groups
   and users across an entire work project. It is best to do this in the
   packages themselves but this is not always possible.

   By default, the OpenEmbedded build system uses the ``fs-perms.txt``,
   which is located in the ``meta/files`` folder in the `Source
   Directory <#source-directory>`__. If you create your own file
   permissions setting table, you should place it in your layer or the
   distro's layer.

   You define the ``FILESYSTEM_PERMS_TABLES`` variable in the
   ``conf/local.conf`` file, which is found in the `Build
   Directory <#build-directory>`__, to point to your custom
   ``fs-perms.txt``. You can specify more than a single file permissions
   setting table. The paths you specify to these files must be defined
   within the ```BBPATH`` <#var-BBPATH>`__ variable.

   For guidance on how to create your own file permissions settings
   table file, examine the existing ``fs-perms.txt``.

FONT_EXTRA_RDEPENDS
   When inheriting the ```fontcache`` <#ref-classes-fontcache>`__ class,
   this variable specifies the runtime dependencies for font packages.
   By default, the ``FONT_EXTRA_RDEPENDS`` is set to "fontconfig-utils".

FONT_PACKAGES
   When inheriting the ```fontcache`` <#ref-classes-fontcache>`__ class,
   this variable identifies packages containing font files that need to
   be cached by Fontconfig. By default, the ``fontcache`` class assumes
   that fonts are in the recipe's main package (i.e.
   ``${``\ ```PN`` <#var-PN>`__\ ``}``). Use this variable if fonts you
   need are in a package other than that main package.

FORCE_RO_REMOVE
   Forces the removal of the packages listed in ``ROOTFS_RO_UNNEEDED``
   during the generation of the root filesystem.

   Set the variable to "1" to force the removal of these packages.

FULL_OPTIMIZATION
   The options to pass in ``TARGET_CFLAGS`` and ``CFLAGS`` when
   compiling an optimized system. This variable defaults to "-O2 -pipe
   ${DEBUG_FLAGS}".

GCCPIE
   Enables Position Independent Executables (PIE) within the GNU C
   Compiler (GCC). Enabling PIE in the GCC makes Return Oriented
   Programming (ROP) attacks much more difficult to execute.

   By default the ``security_flags.inc`` file enables PIE by setting the
   variable as follows: GCCPIE ?= "--enable-default-pie"

GCCVERSION
   Specifies the default version of the GNU C Compiler (GCC) used for
   compilation. By default, ``GCCVERSION`` is set to "8.x" in the
   ``meta/conf/distro/include/tcmode-default.inc`` include file:
   GCCVERSION ?= "8.%" You can override this value by setting it in a
   configuration file such as the ``local.conf``.

GDB
   The minimal command and arguments to run the GNU Debugger.

GITDIR
   The directory in which a local copy of a Git repository is stored
   when it is cloned.

GLIBC_GENERATE_LOCALES
   Specifies the list of GLIBC locales to generate should you not wish
   to generate all LIBC locals, which can be time consuming.

      **Note**

      If you specifically remove the locale
      en_US.UTF-8
      , you must set
      IMAGE_LINGUAS
      appropriately.

   You can set ``GLIBC_GENERATE_LOCALES`` in your ``local.conf`` file.
   By default, all locales are generated. GLIBC_GENERATE_LOCALES =
   "en_GB.UTF-8 en_US.UTF-8"

GROUPADD_PARAM
   When inheriting the ```useradd`` <#ref-classes-useradd>`__ class,
   this variable specifies for a package what parameters should be
   passed to the ``groupadd`` command if you wish to add a group to the
   system when the package is installed.

   Here is an example from the ``dbus`` recipe: GROUPADD_PARAM_${PN} =
   "-r netdev" For information on the standard Linux shell command
   ``groupadd``, see ` <http://linux.die.net/man/8/groupadd>`__.

GROUPMEMS_PARAM
   When inheriting the ```useradd`` <#ref-classes-useradd>`__ class,
   this variable specifies for a package what parameters should be
   passed to the ``groupmems`` command if you wish to modify the members
   of a group when the package is installed.

   For information on the standard Linux shell command ``groupmems``,
   see ` <http://linux.die.net/man/8/groupmems>`__.

GRUB_GFXSERIAL
   Configures the GNU GRand Unified Bootloader (GRUB) to have graphics
   and serial in the boot menu. Set this variable to "1" in your
   ``local.conf`` or distribution configuration file to enable graphics
   and serial in the menu.

   See the ```grub-efi`` <#ref-classes-grub-efi>`__ class for more
   information on how this variable is used.

GRUB_OPTS
   Additional options to add to the GNU GRand Unified Bootloader (GRUB)
   configuration. Use a semi-colon character (``;``) to separate
   multiple options.

   The ``GRUB_OPTS`` variable is optional. See the
   ```grub-efi`` <#ref-classes-grub-efi>`__ class for more information
   on how this variable is used.

GRUB_TIMEOUT
   Specifies the timeout before executing the default ``LABEL`` in the
   GNU GRand Unified Bootloader (GRUB).

   The ``GRUB_TIMEOUT`` variable is optional. See the
   ```grub-efi`` <#ref-classes-grub-efi>`__ class for more information
   on how this variable is used.

GTKIMMODULES_PACKAGES
   When inheriting the
   ```gtk-immodules-cache`` <#ref-classes-gtk-immodules-cache>`__ class,
   this variable specifies the packages that contain the GTK+ input
   method modules being installed when the modules are in packages other
   than the main package.

HOMEPAGE
   Website where more information about the software the recipe is
   building can be found.

HOST_ARCH
   The name of the target architecture, which is normally the same as
   ```TARGET_ARCH`` <#var-TARGET_ARCH>`__. The OpenEmbedded build system
   supports many architectures. Here is an example list of architectures
   supported. This list is by no means complete as the architecture is
   configurable: arm i586 x86_64 powerpc powerpc64 mips mipsel

HOST_CC_ARCH
   Specifies architecture-specific compiler flags that are passed to the
   C compiler.

   Default initialization for ``HOST_CC_ARCH`` varies depending on what
   is being built:

   -  ```TARGET_CC_ARCH`` <#var-TARGET_CC_ARCH>`__ when building for the
      target

   -  ``BUILD_CC_ARCH`` when building for the build host (i.e.
      ``-native``)

   -  ``BUILDSDK_CC_ARCH`` when building for an SDK (i.e.
      ``nativesdk-``)

HOST_OS
   Specifies the name of the target operating system, which is normally
   the same as the ```TARGET_OS`` <#var-TARGET_OS>`__. The variable can
   be set to "linux" for ``glibc``-based systems and to "linux-musl" for
   ``musl``. For ARM/EABI targets, there are also "linux-gnueabi" and
   "linux-musleabi" values possible.

HOST_PREFIX
   Specifies the prefix for the cross-compile toolchain. ``HOST_PREFIX``
   is normally the same as ```TARGET_PREFIX`` <#var-TARGET_PREFIX>`__.

HOST_SYS
   Specifies the system, including the architecture and the operating
   system, for which the build is occurring in the context of the
   current recipe.

   The OpenEmbedded build system automatically sets this variable based
   on ```HOST_ARCH`` <#var-HOST_ARCH>`__,
   ```HOST_VENDOR`` <#var-HOST_VENDOR>`__, and
   ```HOST_OS`` <#var-HOST_OS>`__ variables.

      **Note**

      You do not need to set the variable yourself.

   Consider these two examples:

   -  Given a native recipe on a 32-bit x86 machine running Linux, the
      value is "i686-linux".

   -  Given a recipe being built for a little-endian MIPS target running
      Linux, the value might be "mipsel-linux".

HOSTTOOLS
   A space-separated list (filter) of tools on the build host that
   should be allowed to be called from within build tasks. Using this
   filter helps reduce the possibility of host contamination. If a tool
   specified in the value of ``HOSTTOOLS`` is not found on the build
   host, the OpenEmbedded build system produces an error and the build
   is not started.

   For additional information, see
   ```HOSTTOOLS_NONFATAL`` <#var-HOSTTOOLS_NONFATAL>`__.

HOSTTOOLS_NONFATAL
   A space-separated list (filter) of tools on the build host that
   should be allowed to be called from within build tasks. Using this
   filter helps reduce the possibility of host contamination. Unlike
   ```HOSTTOOLS`` <#var-HOSTTOOLS>`__, the OpenEmbedded build system
   does not produce an error if a tool specified in the value of
   ``HOSTTOOLS_NONFATAL`` is not found on the build host. Thus, you can
   use ``HOSTTOOLS_NONFATAL`` to filter optional host tools.

HOST_VENDOR
   Specifies the name of the vendor. ``HOST_VENDOR`` is normally the
   same as ```TARGET_VENDOR`` <#var-TARGET_VENDOR>`__.

ICECC_DISABLED
   Disables or enables the ``icecc`` (Icecream) function. For more
   information on this function and best practices for using this
   variable, see the "```icecc.bbclass`` <#ref-classes-icecc>`__"
   section.

   Setting this variable to "1" in your ``local.conf`` disables the
   function: ICECC_DISABLED ??= "1" To enable the function, set the
   variable as follows: ICECC_DISABLED = ""

ICECC_ENV_EXEC
   Points to the ``icecc-create-env`` script that you provide. This
   variable is used by the ```icecc`` <#ref-classes-icecc>`__ class. You
   set this variable in your ``local.conf`` file.

   If you do not point to a script that you provide, the OpenEmbedded
   build system uses the default script provided by the
   ``icecc-create-env.bb`` recipe, which is a modified version and not
   the one that comes with ``icecc``.

ICECC_PARALLEL_MAKE
   Extra options passed to the ``make`` command during the
   ```do_compile`` <#ref-tasks-compile>`__ task that specify parallel
   compilation. This variable usually takes the form of "-j x", where x
   represents the maximum number of parallel threads ``make`` can run.

      **Note**

      The options passed affect builds on all enabled machines on the
      network, which are machines running the
      iceccd
      daemon.

   If your enabled machines support multiple cores, coming up with the
   maximum number of parallel threads that gives you the best
   performance could take some experimentation since machine speed,
   network lag, available memory, and existing machine loads can all
   affect build time. Consequently, unlike the
   ```PARALLEL_MAKE`` <#var-PARALLEL_MAKE>`__ variable, there is no
   rule-of-thumb for setting ``ICECC_PARALLEL_MAKE`` to achieve optimal
   performance.

   If you do not set ``ICECC_PARALLEL_MAKE``, the build system does not
   use it (i.e. the system does not detect and assign the number of
   cores as is done with ``PARALLEL_MAKE``).

ICECC_PATH
   The location of the ``icecc`` binary. You can set this variable in
   your ``local.conf`` file. If your ``local.conf`` file does not define
   this variable, the ```icecc`` <#ref-classes-icecc>`__ class attempts
   to define it by locating ``icecc`` using ``which``.

ICECC_USER_CLASS_BL
   Identifies user classes that you do not want the Icecream distributed
   compile support to consider. This variable is used by the
   ```icecc`` <#ref-classes-icecc>`__ class. You set this variable in
   your ``local.conf`` file.

   When you list classes using this variable, you are "blacklisting"
   them from distributed compilation across remote hosts. Any classes
   you list will be distributed and compiled locally.

ICECC_USER_PACKAGE_BL
   Identifies user recipes that you do not want the Icecream distributed
   compile support to consider. This variable is used by the
   ```icecc`` <#ref-classes-icecc>`__ class. You set this variable in
   your ``local.conf`` file.

   When you list packages using this variable, you are "blacklisting"
   them from distributed compilation across remote hosts. Any packages
   you list will be distributed and compiled locally.

ICECC_USER_PACKAGE_WL
   Identifies user recipes that use an empty
   ```PARALLEL_MAKE`` <#var-PARALLEL_MAKE>`__ variable that you want to
   force remote distributed compilation on using the Icecream
   distributed compile support. This variable is used by the
   ```icecc`` <#ref-classes-icecc>`__ class. You set this variable in
   your ``local.conf`` file.

IMAGE_BASENAME
   The base name of image output files. This variable defaults to the
   recipe name (``${``\ ```PN`` <#var-PN>`__\ ``}``).

IMAGE_BOOT_FILES
   A space-separated list of files installed into the boot partition
   when preparing an image using the Wic tool with the
   ``bootimg-partition`` source plugin. By default, the files are
   installed under the same name as the source files. To change the
   installed name, separate it from the original name with a semi-colon
   (;). Source files need to be located in
   ```DEPLOY_DIR_IMAGE`` <#var-DEPLOY_DIR_IMAGE>`__. Here are two
   examples: IMAGE_BOOT_FILES = "u-boot.img uImage;kernel"
   IMAGE_BOOT_FILES = "u-boot.${UBOOT_SUFFIX} ${KERNEL_IMAGETYPE}"

   Alternatively, source files can be picked up using a glob pattern. In
   this case, the destination file must have the same name as the base
   name of the source file path. To install files into a directory
   within the target location, pass its name after a semi-colon (;).
   Here are two examples: IMAGE_BOOT_FILES = "bcm2835-bootfiles/*"
   IMAGE_BOOT_FILES = "bcm2835-bootfiles/*;boot/" The first example
   installs all files from ``${DEPLOY_DIR_IMAGE}/bcm2835-bootfiles``
   into the root of the target partition. The second example installs
   the same files into a ``boot`` directory within the target partition.

   You can find information on how to use the Wic tool in the "`Creating
   Partitioned Images Using
   Wic <&YOCTO_DOCS_DEV_URL;#creating-partitioned-images-using-wic>`__"
   section of the Yocto Project Development Tasks Manual. Reference
   material for Wic is located in the "`OpenEmbedded Kickstart (.wks)
   Reference <&YOCTO_DOCS_REF_URL;#ref-kickstart>`__" chapter.

IMAGE_CLASSES
   A list of classes that all images should inherit. You typically use
   this variable to specify the list of classes that register the
   different types of images the OpenEmbedded build system creates.

   The default value for ``IMAGE_CLASSES`` is ``image_types``. You can
   set this variable in your ``local.conf`` or in a distribution
   configuration file.

   For more information, see ``meta/classes/image_types.bbclass`` in the
   `Source Directory <#source-directory>`__.

IMAGE_CMD
   Specifies the command to create the image file for a specific image
   type, which corresponds to the value set set in
   ```IMAGE_FSTYPES`` <#var-IMAGE_FSTYPES>`__, (e.g. ``ext3``,
   ``btrfs``, and so forth). When setting this variable, you should use
   an override for the associated type. Here is an example:
   IMAGE_CMD_jffs2 = "mkfs.jffs2 --root=${IMAGE_ROOTFS} \\ --faketime
   --output=${DEPLOY_DIR_IMAGE}/${IMAGE_NAME}.rootfs.jffs2 \\
   ${EXTRA_IMAGECMD}"

   You typically do not need to set this variable unless you are adding
   support for a new image type. For more examples on how to set this
   variable, see the ```image_types`` <#ref-classes-image_types>`__
   class file, which is ``meta/classes/image_types.bbclass``.

IMAGE_DEVICE_TABLES
   Specifies one or more files that contain custom device tables that
   are passed to the ``makedevs`` command as part of creating an image.
   These files list basic device nodes that should be created under
   ``/dev`` within the image. If ``IMAGE_DEVICE_TABLES`` is not set,
   ``files/device_table-minimal.txt`` is used, which is located by
   ```BBPATH`` <#var-BBPATH>`__. For details on how you should write
   device table files, see ``meta/files/device_table-minimal.txt`` as an
   example.

IMAGE_FEATURES
   The primary list of features to include in an image. Typically, you
   configure this variable in an image recipe. Although you can use this
   variable from your ``local.conf`` file, which is found in the `Build
   Directory <#build-directory>`__, best practices dictate that you do
   not.

      **Note**

      To enable extra features from outside the image recipe, use the
      EXTRA_IMAGE_FEATURES
      variable.

   For a list of image features that ships with the Yocto Project, see
   the "`Image Features <#ref-features-image>`__" section.

   For an example that shows how to customize your image by using this
   variable, see the "`Customizing Images Using Custom
   ``IMAGE_FEATURES`` and
   ``EXTRA_IMAGE_FEATURES`` <&YOCTO_DOCS_DEV_URL;#usingpoky-extend-customimage-imagefeatures>`__"
   section in the Yocto Project Development Tasks Manual.

IMAGE_FSTYPES
   Specifies the formats the OpenEmbedded build system uses during the
   build when creating the root filesystem. For example, setting
   ``IMAGE_FSTYPES`` as follows causes the build system to create root
   filesystems using two formats: ``.ext3`` and ``.tar.bz2``:
   IMAGE_FSTYPES = "ext3 tar.bz2"

   For the complete list of supported image formats from which you can
   choose, see ```IMAGE_TYPES`` <#var-IMAGE_TYPES>`__.

      **Note**

      -  If an image recipe uses the "inherit image" line and you are
         setting ``IMAGE_FSTYPES`` inside the recipe, you must set
         ``IMAGE_FSTYPES`` prior to using the "inherit image" line.

      -  Due to the way the OpenEmbedded build system processes this
         variable, you cannot update its contents by using ``_append``
         or ``_prepend``. You must use the ``+=`` operator to add one or
         more options to the ``IMAGE_FSTYPES`` variable.

IMAGE_INSTALL
   Used by recipes to specify the packages to install into an image
   through the ```image`` <#ref-classes-image>`__ class. Use the
   ``IMAGE_INSTALL`` variable with care to avoid ordering issues.

   Image recipes set ``IMAGE_INSTALL`` to specify the packages to
   install into an image through ``image.bbclass``. Additionally,
   "helper" classes such as the
   ```core-image`` <#ref-classes-core-image>`__ class exist that can
   take lists used with ``IMAGE_FEATURES`` and turn them into
   auto-generated entries in ``IMAGE_INSTALL`` in addition to its
   default contents.

   When you use this variable, it is best to use it as follows:
   IMAGE_INSTALL_append = " package-name" Be sure to include the space
   between the quotation character and the start of the package name or
   names.

      **Note**

      -  When working with a
         ```core-image-minimal-initramfs`` <#images-core-image-minimal-initramfs>`__
         image, do not use the ``IMAGE_INSTALL`` variable to specify
         packages for installation. Instead, use the
         ```PACKAGE_INSTALL`` <#var-PACKAGE_INSTALL>`__ variable, which
         allows the initial RAM filesystem (initramfs) recipe to use a
         fixed set of packages and not be affected by ``IMAGE_INSTALL``.
         For information on creating an initramfs, see the "`Building an
         Initial RAM Filesystem (initramfs)
         Image <&YOCTO_DOCS_DEV_URL;#building-an-initramfs-image>`__"
         section in the Yocto Project Development Tasks Manual.

      -  Using ``IMAGE_INSTALL`` with the
         ```+=`` <&YOCTO_DOCS_BB_URL;#appending-and-prepending>`__
         BitBake operator within the ``/conf/local.conf`` file or from
         within an image recipe is not recommended. Use of this operator
         in these ways can cause ordering issues. Since
         ``core-image.bbclass`` sets ``IMAGE_INSTALL`` to a default
         value using the
         ```?=`` <&YOCTO_DOCS_BB_URL;#setting-a-default-value>`__
         operator, using a ``+=`` operation against ``IMAGE_INSTALL``
         results in unexpected behavior when used within
         ``conf/local.conf``. Furthermore, the same operation from
         within an image recipe may or may not succeed depending on the
         specific situation. In both these cases, the behavior is
         contrary to how most users expect the ``+=`` operator to work.

IMAGE_LINGUAS
   Specifies the list of locales to install into the image during the
   root filesystem construction process. The OpenEmbedded build system
   automatically splits locale files, which are used for localization,
   into separate packages. Setting the ``IMAGE_LINGUAS`` variable
   ensures that any locale packages that correspond to packages already
   selected for installation into the image are also installed. Here is
   an example: IMAGE_LINGUAS = "pt-br de-de"

   In this example, the build system ensures any Brazilian Portuguese
   and German locale files that correspond to packages in the image are
   installed (i.e. ``*-locale-pt-br`` and ``*-locale-de-de`` as well as
   ``*-locale-pt`` and ``*-locale-de``, since some software packages
   only provide locale files by language and not by country-specific
   language).

   See the ```GLIBC_GENERATE_LOCALES`` <#var-GLIBC_GENERATE_LOCALES>`__
   variable for information on generating GLIBC locales.

IMAGE_MANIFEST
   The manifest file for the image. This file lists all the installed
   packages that make up the image. The file contains package
   information on a line-per-package basis as follows: packagename
   packagearch version

   The ```image`` <#ref-classes-image>`__ class defines the manifest
   file as follows: IMAGE_MANIFEST =
   "${DEPLOY_DIR_IMAGE}/${IMAGE_NAME}.rootfs.manifest" The location is
   derived using the ```DEPLOY_DIR_IMAGE`` <#var-DEPLOY_DIR_IMAGE>`__
   and ```IMAGE_NAME`` <#var-IMAGE_NAME>`__ variables. You can find
   information on how the image is created in the "`Image
   Generation <&YOCTO_DOCS_OM_URL;#image-generation-dev-environment>`__"
   section in the Yocto Project Overview and Concepts Manual.

IMAGE_NAME
   The name of the output image files minus the extension. This variable
   is derived using the ```IMAGE_BASENAME`` <#var-IMAGE_BASENAME>`__,
   ```MACHINE`` <#var-MACHINE>`__, and ```DATETIME`` <#var-DATETIME>`__
   variables: IMAGE_NAME = "${IMAGE_BASENAME}-${MACHINE}-${DATETIME}"

IMAGE_OVERHEAD_FACTOR
   Defines a multiplier that the build system applies to the initial
   image size for cases when the multiplier times the returned disk
   usage value for the image is greater than the sum of
   ``IMAGE_ROOTFS_SIZE`` and ``IMAGE_ROOTFS_EXTRA_SPACE``. The result of
   the multiplier applied to the initial image size creates free disk
   space in the image as overhead. By default, the build process uses a
   multiplier of 1.3 for this variable. This default value results in
   30% free disk space added to the image when this method is used to
   determine the final generated image size. You should be aware that
   post install scripts and the package management system uses disk
   space inside this overhead area. Consequently, the multiplier does
   not produce an image with all the theoretical free disk space. See
   ``IMAGE_ROOTFS_SIZE`` for information on how the build system
   determines the overall image size.

   The default 30% free disk space typically gives the image enough room
   to boot and allows for basic post installs while still leaving a
   small amount of free disk space. If 30% free space is inadequate, you
   can increase the default value. For example, the following setting
   gives you 50% free space added to the image: IMAGE_OVERHEAD_FACTOR =
   "1.5"

   Alternatively, you can ensure a specific amount of free disk space is
   added to the image by using the ``IMAGE_ROOTFS_EXTRA_SPACE``
   variable.

IMAGE_PKGTYPE
   Defines the package type (i.e. DEB, RPM, IPK, or TAR) used by the
   OpenEmbedded build system. The variable is defined appropriately by
   the ```package_deb`` <#ref-classes-package_deb>`__,
   ```package_rpm`` <#ref-classes-package_rpm>`__,
   ```package_ipk`` <#ref-classes-package_ipk>`__, or
   ```package_tar`` <#ref-classes-package_tar>`__ class.

      **Note**

      The
      package_tar
      class is broken and is not supported. It is recommended that you
      do not use it.

   The ```populate_sdk_*`` <#ref-classes-populate-sdk-*>`__ and
   ```image`` <#ref-classes-image>`__ classes use the ``IMAGE_PKGTYPE``
   for packaging up images and SDKs.

   You should not set the ``IMAGE_PKGTYPE`` manually. Rather, the
   variable is set indirectly through the appropriate
   ```package_*`` <#ref-classes-package>`__ class using the
   ```PACKAGE_CLASSES`` <#var-PACKAGE_CLASSES>`__ variable. The
   OpenEmbedded build system uses the first package type (e.g. DEB, RPM,
   or IPK) that appears with the variable

      **Note**

      Files using the
      .tar
      format are never used as a substitute packaging format for DEB,
      RPM, and IPK formatted files for your image or SDK.

IMAGE_POSTPROCESS_COMMAND
   Specifies a list of functions to call once the OpenEmbedded build
   system creates the final image output files. You can specify
   functions separated by semicolons: IMAGE_POSTPROCESS_COMMAND +=
   "function; ... "

   If you need to pass the root filesystem path to a command within the
   function, you can use ``${IMAGE_ROOTFS}``, which points to the
   directory that becomes the root filesystem image. See the
   ```IMAGE_ROOTFS`` <#var-IMAGE_ROOTFS>`__ variable for more
   information.

IMAGE_PREPROCESS_COMMAND
   Specifies a list of functions to call before the OpenEmbedded build
   system creates the final image output files. You can specify
   functions separated by semicolons: IMAGE_PREPROCESS_COMMAND +=
   "function; ... "

   If you need to pass the root filesystem path to a command within the
   function, you can use ``${IMAGE_ROOTFS}``, which points to the
   directory that becomes the root filesystem image. See the
   ```IMAGE_ROOTFS`` <#var-IMAGE_ROOTFS>`__ variable for more
   information.

IMAGE_ROOTFS
   The location of the root filesystem while it is under construction
   (i.e. during the ```do_rootfs`` <#ref-tasks-rootfs>`__ task). This
   variable is not configurable. Do not change it.

IMAGE_ROOTFS_ALIGNMENT
   Specifies the alignment for the output image file in Kbytes. If the
   size of the image is not a multiple of this value, then the size is
   rounded up to the nearest multiple of the value. The default value is
   "1". See ```IMAGE_ROOTFS_SIZE`` <#var-IMAGE_ROOTFS_SIZE>`__ for
   additional information.

IMAGE_ROOTFS_EXTRA_SPACE
   Defines additional free disk space created in the image in Kbytes. By
   default, this variable is set to "0". This free disk space is added
   to the image after the build system determines the image size as
   described in ``IMAGE_ROOTFS_SIZE``.

   This variable is particularly useful when you want to ensure that a
   specific amount of free disk space is available on a device after an
   image is installed and running. For example, to be sure 5 Gbytes of
   free disk space is available, set the variable as follows:
   IMAGE_ROOTFS_EXTRA_SPACE = "5242880"

   For example, the Yocto Project Build Appliance specifically requests
   40 Gbytes of extra space with the line: IMAGE_ROOTFS_EXTRA_SPACE =
   "41943040"

IMAGE_ROOTFS_SIZE
   Defines the size in Kbytes for the generated image. The OpenEmbedded
   build system determines the final size for the generated image using
   an algorithm that takes into account the initial disk space used for
   the generated image, a requested size for the image, and requested
   additional free disk space to be added to the image. Programatically,
   the build system determines the final size of the generated image as
   follows: if (image-du \* overhead) < rootfs-size:
   internal-rootfs-size = rootfs-size + xspace else:
   internal-rootfs-size = (image-du \* overhead) + xspace where:
   image-du = Returned value of the du command on the image. overhead =
   IMAGE_OVERHEAD_FACTOR rootfs-size = IMAGE_ROOTFS_SIZE
   internal-rootfs-size = Initial root filesystem size before any
   modifications. xspace = IMAGE_ROOTFS_EXTRA_SPACE

   See the ```IMAGE_OVERHEAD_FACTOR`` <#var-IMAGE_OVERHEAD_FACTOR>`__
   and ```IMAGE_ROOTFS_EXTRA_SPACE`` <#var-IMAGE_ROOTFS_EXTRA_SPACE>`__
   variables for related information.

IMAGE_TYPEDEP
   Specifies a dependency from one image type on another. Here is an
   example from the ```image-live`` <#ref-classes-image-live>`__ class:
   IMAGE_TYPEDEP_live = "ext3"

   In the previous example, the variable ensures that when "live" is
   listed with the ```IMAGE_FSTYPES`` <#var-IMAGE_FSTYPES>`__ variable,
   the OpenEmbedded build system produces an ``ext3`` image first since
   one of the components of the live image is an ``ext3`` formatted
   partition containing the root filesystem.

IMAGE_TYPES
   Specifies the complete list of supported image types by default:
   btrfs container cpio cpio.gz cpio.lz4 cpio.lzma cpio.xz cramfs elf
   ext2 ext2.bz2 ext2.gz ext2.lzma ext3 ext3.gz ext4 ext4.gz f2fs hddimg
   iso jffs2 jffs2.sum multiubi squashfs squashfs-lz4 squashfs-lzo
   squashfs-xz tar tar.bz2 tar.gz tar.lz4 tar.xz ubi ubifs wic wic.bz2
   wic.gz wic.lzma

   For more information about these types of images, see
   ``meta/classes/image_types*.bbclass`` in the `Source
   Directory <#source-directory>`__.

INC_PR
   Helps define the recipe revision for recipes that share a common
   ``include`` file. You can think of this variable as part of the
   recipe revision as set from within an include file.

   Suppose, for example, you have a set of recipes that are used across
   several projects. And, within each of those recipes the revision (its
   ```PR`` <#var-PR>`__ value) is set accordingly. In this case, when
   the revision of those recipes changes, the burden is on you to find
   all those recipes and be sure that they get changed to reflect the
   updated version of the recipe. In this scenario, it can get
   complicated when recipes that are used in many places and provide
   common functionality are upgraded to a new revision.

   A more efficient way of dealing with this situation is to set the
   ``INC_PR`` variable inside the ``include`` files that the recipes
   share and then expand the ``INC_PR`` variable within the recipes to
   help define the recipe revision.

   The following provides an example that shows how to use the
   ``INC_PR`` variable given a common ``include`` file that defines the
   variable. Once the variable is defined in the ``include`` file, you
   can use the variable to set the ``PR`` values in each recipe. You
   will notice that when you set a recipe's ``PR`` you can provide more
   granular revisioning by appending values to the ``INC_PR`` variable:
   recipes-graphics/xorg-font/xorg-font-common.inc:INC_PR = "r2"
   recipes-graphics/xorg-font/encodings_1.0.4.bb:PR = "${INC_PR}.1"
   recipes-graphics/xorg-font/font-util_1.3.0.bb:PR = "${INC_PR}.0"
   recipes-graphics/xorg-font/font-alias_1.0.3.bb:PR = "${INC_PR}.3" The
   first line of the example establishes the baseline revision to be
   used for all recipes that use the ``include`` file. The remaining
   lines in the example are from individual recipes and show how the
   ``PR`` value is set.

INCOMPATIBLE_LICENSE
   Specifies a space-separated list of license names (as they would
   appear in ```LICENSE`` <#var-LICENSE>`__) that should be excluded
   from the build. Recipes that provide no alternatives to listed
   incompatible licenses are not built. Packages that are individually
   licensed with the specified incompatible licenses will be deleted.

      **Note**

      This functionality is only regularly tested using the following
      setting:
      ::

              INCOMPATIBLE_LICENSE = "GPL-3.0 LGPL-3.0 AGPL-3.0"
                             

      Although you can use other settings, you might be required to
      remove dependencies on or provide alternatives to components that
      are required to produce a functional system image.

   ..

      **Note**

      It is possible to define a list of licenses that are allowed to be
      used instead of the licenses that are excluded. To do this, define
      a variable
      COMPATIBLE_LICENSES
      with the names of the licences that are allowed. Then define
      INCOMPATIBLE_LICENSE
      as:
      ::

              INCOMPATIBLE_LICENSE = "${@' '.join(sorted(set(d.getVar('AVAILABLE_LICENSES').split()) - set(d.getVar('COMPATIBLE_LICENSES').split())))}"
                             

      This will result in
      INCOMPATIBLE_LICENSE
      containing the names of all licences from
      AVAILABLE_LICENSES
      except the ones specified in
      COMPATIBLE_LICENSES
      , thus only allowing the latter licences to be used.

INHERIT
   Causes the named class or classes to be inherited globally. Anonymous
   functions in the class or classes are not executed for the base
   configuration and in each individual recipe. The OpenEmbedded build
   system ignores changes to ``INHERIT`` in individual recipes.

   For more information on ``INHERIT``, see the "```INHERIT``
   Configuration
   Directive <&YOCTO_DOCS_BB_URL;#inherit-configuration-directive>`__"
   section in the Bitbake User Manual.

INHERIT_DISTRO
   Lists classes that will be inherited at the distribution level. It is
   unlikely that you want to edit this variable.

   The default value of the variable is set as follows in the
   ``meta/conf/distro/defaultsetup.conf`` file: INHERIT_DISTRO ?=
   "debian devshell sstate license"

INHIBIT_DEFAULT_DEPS
   Prevents the default dependencies, namely the C compiler and standard
   C library (libc), from being added to ```DEPENDS`` <#var-DEPENDS>`__.
   This variable is usually used within recipes that do not require any
   compilation using the C compiler.

   Set the variable to "1" to prevent the default dependencies from
   being added.

INHIBIT_PACKAGE_DEBUG_SPLIT
   Prevents the OpenEmbedded build system from splitting out debug
   information during packaging. By default, the build system splits out
   debugging information during the
   ```do_package`` <#ref-tasks-package>`__ task. For more information on
   how debug information is split out, see the
   ```PACKAGE_DEBUG_SPLIT_STYLE`` <#var-PACKAGE_DEBUG_SPLIT_STYLE>`__
   variable.

   To prevent the build system from splitting out debug information
   during packaging, set the ``INHIBIT_PACKAGE_DEBUG_SPLIT`` variable as
   follows: INHIBIT_PACKAGE_DEBUG_SPLIT = "1"

INHIBIT_PACKAGE_STRIP
   If set to "1", causes the build to not strip binaries in resulting
   packages and prevents the ``-dbg`` package from containing the source
   files.

   By default, the OpenEmbedded build system strips binaries and puts
   the debugging symbols into ``${``\ ```PN`` <#var-PN>`__\ ``}-dbg``.
   Consequently, you should not set ``INHIBIT_PACKAGE_STRIP`` when you
   plan to debug in general.

INHIBIT_SYSROOT_STRIP
   If set to "1", causes the build to not strip binaries in the
   resulting sysroot.

   By default, the OpenEmbedded build system strips binaries in the
   resulting sysroot. When you specifically set the
   ``INHIBIT_SYSROOT_STRIP`` variable to "1" in your recipe, you inhibit
   this stripping.

   If you want to use this variable, include the
   ```staging`` <#ref-classes-staging>`__ class. This class uses a
   ``sys_strip()`` function to test for the variable and acts
   accordingly.

      **Note**

      Use of the
      INHIBIT_SYSROOT_STRIP
      variable occurs in rare and special circumstances. For example,
      suppose you are building bare-metal firmware by using an external
      GCC toolchain. Furthermore, even if the toolchain's binaries are
      strippable, other files exist that are needed for the build that
      are not strippable.

INITRAMFS_FSTYPES
   Defines the format for the output image of an initial RAM filesystem
   (initramfs), which is used during boot. Supported formats are the
   same as those supported by the
   ```IMAGE_FSTYPES`` <#var-IMAGE_FSTYPES>`__ variable.

   The default value of this variable, which is set in the
   ``meta/conf/bitbake.conf`` configuration file in the `Source
   Directory <#source-directory>`__, is "cpio.gz". The Linux kernel's
   initramfs mechanism, as opposed to the initial RAM filesystem
   `initrd <https://en.wikipedia.org/wiki/Initrd>`__ mechanism, expects
   an optionally compressed cpio archive.

INITRAMFS_IMAGE
   Specifies the ```PROVIDES`` <#var-PROVIDES>`__ name of an image
   recipe that is used to build an initial RAM filesystem (initramfs)
   image. In other words, the ``INITRAMFS_IMAGE`` variable causes an
   additional recipe to be built as a dependency to whatever root
   filesystem recipe you might be using (e.g. ``core-image-sato``). The
   initramfs image recipe you provide should set
   ```IMAGE_FSTYPES`` <#var-IMAGE_FSTYPES>`__ to
   ```INITRAMFS_FSTYPES`` <#var-INITRAMFS_FSTYPES>`__.

   An initramfs image provides a temporary root filesystem used for
   early system initialization (e.g. loading of modules needed to locate
   and mount the "real" root filesystem).

      **Note**

      See the
      meta/recipes-core/images/core-image-minimal-initramfs.bb
      recipe in the
      Source Directory
      for an example initramfs recipe. To select this sample recipe as
      the one built to provide the initramfs image, set
      INITRAMFS_IMAGE
      to "core-image-minimal-initramfs".

   You can also find more information by referencing the
   ``meta-poky/conf/local.conf.sample.extended`` configuration file in
   the Source Directory, the ```image`` <#ref-classes-image>`__ class,
   and the ```kernel`` <#ref-classes-kernel>`__ class to see how to use
   the ``INITRAMFS_IMAGE`` variable.

   If ``INITRAMFS_IMAGE`` is empty, which is the default, then no
   initramfs image is built.

   For more information, you can also see the
   ```INITRAMFS_IMAGE_BUNDLE`` <#var-INITRAMFS_IMAGE_BUNDLE>`__
   variable, which allows the generated image to be bundled inside the
   kernel image. Additionally, for information on creating an initramfs
   image, see the "`Building an Initial RAM Filesystem (initramfs)
   Image <&YOCTO_DOCS_DEV_URL;#building-an-initramfs-image>`__" section
   in the Yocto Project Development Tasks Manual.

INITRAMFS_IMAGE_BUNDLE
   Controls whether or not the image recipe specified by
   ```INITRAMFS_IMAGE`` <#var-INITRAMFS_IMAGE>`__ is run through an
   extra pass
   (```do_bundle_initramfs`` <#ref-tasks-bundle_initramfs>`__) during
   kernel compilation in order to build a single binary that contains
   both the kernel image and the initial RAM filesystem (initramfs)
   image. This makes use of the
   ```CONFIG_INITRAMFS_SOURCE`` <#var-CONFIG_INITRAMFS_SOURCE>`__ kernel
   feature.

      **Note**

      Using an extra compilation pass to bundle the initramfs avoids a
      circular dependency between the kernel recipe and the initramfs
      recipe should the initramfs include kernel modules. Should that be
      the case, the initramfs recipe depends on the kernel for the
      kernel modules, and the kernel depends on the initramfs recipe
      since the initramfs is bundled inside the kernel image.

   The combined binary is deposited into the ``tmp/deploy`` directory,
   which is part of the `Build Directory <#build-directory>`__.

   Setting the variable to "1" in a configuration file causes the
   OpenEmbedded build system to generate a kernel image with the
   initramfs specified in ``INITRAMFS_IMAGE`` bundled within:
   INITRAMFS_IMAGE_BUNDLE = "1" By default, the
   ```kernel`` <#ref-classes-kernel>`__ class sets this variable to a
   null string as follows: INITRAMFS_IMAGE_BUNDLE ?= ""

      **Note**

      You must set the
      INITRAMFS_IMAGE_BUNDLE
      variable in a configuration file. You cannot set the variable in a
      recipe file.

   See the
   ```local.conf.sample.extended`` <&YOCTO_GIT_URL;/cgit/cgit.cgi/poky/tree/meta-poky/conf/local.conf.sample.extended>`__
   file for additional information. Also, for information on creating an
   initramfs, see the "`Building an Initial RAM Filesystem (initramfs)
   Image <&YOCTO_DOCS_DEV_URL;#building-an-initramfs-image>`__" section
   in the Yocto Project Development Tasks Manual.

INITRAMFS_LINK_NAME
   The link name of the initial RAM filesystem image. This variable is
   set in the ``meta/classes/kernel-artifact-names.bbclass`` file as
   follows: INITRAMFS_LINK_NAME ?=
   "initramfs-${KERNEL_ARTIFACT_LINK_NAME}" The value of the
   ``KERNEL_ARTIFACT_LINK_NAME`` variable, which is set in the same
   file, has the following value: KERNEL_ARTIFACT_LINK_NAME ?=
   "${MACHINE}"

   See the ```MACHINE`` <#var-MACHINE>`__ variable for additional
   information.

INITRAMFS_NAME
   The base name of the initial RAM filesystem image. This variable is
   set in the ``meta/classes/kernel-artifact-names.bbclass`` file as
   follows: INITRAMFS_NAME ?= "initramfs-${KERNEL_ARTIFACT_NAME}" The
   value of the ```KERNEL_ARTIFACT_NAME`` <#var-KERNEL_ARTIFACT_NAME>`__
   variable, which is set in the same file, has the following value:
   KERNEL_ARTIFACT_NAME ?=
   "${PKGE}-${PKGV}-${PKGR}-${MACHINE}${IMAGE_VERSION_SUFFIX}"

INITRD
   Indicates list of filesystem images to concatenate and use as an
   initial RAM disk (``initrd``).

   The ``INITRD`` variable is an optional variable used with the
   ```image-live`` <#ref-classes-image-live>`__ class.

INITRD_IMAGE
   When building a "live" bootable image (i.e. when
   ```IMAGE_FSTYPES`` <#var-IMAGE_FSTYPES>`__ contains "live"),
   ``INITRD_IMAGE`` specifies the image recipe that should be built to
   provide the initial RAM disk image. The default value is
   "core-image-minimal-initramfs".

   See the ```image-live`` <#ref-classes-image-live>`__ class for more
   information.

INITSCRIPT_NAME
   The filename of the initialization script as installed to
   ``${sysconfdir}/init.d``.

   This variable is used in recipes when using ``update-rc.d.bbclass``.
   The variable is mandatory.

INITSCRIPT_PACKAGES
   A list of the packages that contain initscripts. If multiple packages
   are specified, you need to append the package name to the other
   ``INITSCRIPT_*`` as an override.

   This variable is used in recipes when using ``update-rc.d.bbclass``.
   The variable is optional and defaults to the ```PN`` <#var-PN>`__
   variable.

INITSCRIPT_PARAMS
   Specifies the options to pass to ``update-rc.d``. Here is an example:
   INITSCRIPT_PARAMS = "start 99 5 2 . stop 20 0 1 6 ."

   In this example, the script has a runlevel of 99, starts the script
   in initlevels 2 and 5, and stops the script in levels 0, 1 and 6.

   The variable's default value is "defaults", which is set in the
   ```update-rc.d`` <#ref-classes-update-rc.d>`__ class.

   The value in ``INITSCRIPT_PARAMS`` is passed through to the
   ``update-rc.d`` command. For more information on valid parameters,
   please see the ``update-rc.d`` manual page at
   ` <http://www.tin.org/bin/man.cgi?section=8&topic=update-rc.d>`__.

INSANE_SKIP
   Specifies the QA checks to skip for a specific package within a
   recipe. For example, to skip the check for symbolic link ``.so``
   files in the main package of a recipe, add the following to the
   recipe. The package name override must be used, which in this example
   is ``${PN}``: INSANE_SKIP_${PN} += "dev-so"

   See the "```insane.bbclass`` <#ref-classes-insane>`__" section for a
   list of the valid QA checks you can specify using this variable.

INSTALL_TIMEZONE_FILE
   By default, the ``tzdata`` recipe packages an ``/etc/timezone`` file.
   Set the ``INSTALL_TIMEZONE_FILE`` variable to "0" at the
   configuration level to disable this behavior.

IPK_FEED_URIS
   When the IPK backend is in use and package management is enabled on
   the target, you can use this variable to set up ``opkg`` in the
   target image to point to package feeds on a nominated server. Once
   the feed is established, you can perform installations or upgrades
   using the package manager at runtime.

KARCH
   Defines the kernel architecture used when assembling the
   configuration. Architectures supported for this release are: powerpc
   i386 x86_64 arm qemu mips

   You define the ``KARCH`` variable in the `BSP
   Descriptions <&YOCTO_DOCS_KERNEL_DEV_URL;#bsp-descriptions>`__.

KBRANCH
   A regular expression used by the build process to explicitly identify
   the kernel branch that is validated, patched, and configured during a
   build. You must set this variable to ensure the exact kernel branch
   you want is being used by the build process.

   Values for this variable are set in the kernel's recipe file and the
   kernel's append file. For example, if you are using the
   ``linux-yocto_4.12`` kernel, the kernel recipe file is the
   ``meta/recipes-kernel/linux/linux-yocto_4.12.bb`` file. ``KBRANCH``
   is set as follows in that kernel recipe file: KBRANCH ?=
   "standard/base"

   This variable is also used from the kernel's append file to identify
   the kernel branch specific to a particular machine or target
   hardware. Continuing with the previous kernel example, the kernel's
   append file (i.e. ``linux-yocto_4.12.bbappend``) is located in the
   BSP layer for a given machine. For example, the append file for the
   Beaglebone, EdgeRouter, and generic versions of both 32 and 64-bit IA
   machines (``meta-yocto-bsp``) is named
   ``meta-yocto-bsp/recipes-kernel/linux/linux-yocto_4.12.bbappend``.
   Here are the related statements from that append file:
   KBRANCH_genericx86 = "standard/base" KBRANCH_genericx86-64 =
   "standard/base" KBRANCH_edgerouter = "standard/edgerouter"
   KBRANCH_beaglebone = "standard/beaglebone" The ``KBRANCH`` statements
   identify the kernel branch to use when building for each supported
   BSP.

KBUILD_DEFCONFIG
   When used with the ```kernel-yocto`` <#ref-classes-kernel-yocto>`__
   class, specifies an "in-tree" kernel configuration file for use
   during a kernel build.

   Typically, when using a ``defconfig`` to configure a kernel during a
   build, you place the file in your layer in the same manner as you
   would place patch files and configuration fragment files (i.e.
   "out-of-tree"). However, if you want to use a ``defconfig`` file that
   is part of the kernel tree (i.e. "in-tree"), you can use the
   ``KBUILD_DEFCONFIG`` variable and append the
   ```KMACHINE`` <#var-KMACHINE>`__ variable to point to the
   ``defconfig`` file.

   To use the variable, set it in the append file for your kernel recipe
   using the following form: KBUILD_DEFCONFIG_KMACHINE ?= defconfig_file
   Here is an example from a "raspberrypi2" ``KMACHINE`` build that uses
   a ``defconfig`` file named "bcm2709_defconfig":
   KBUILD_DEFCONFIG_raspberrypi2 = "bcm2709_defconfig" As an
   alternative, you can use the following within your append file:
   KBUILD_DEFCONFIG_pn-linux-yocto ?= defconfig_file For more
   information on how to use the ``KBUILD_DEFCONFIG`` variable, see the
   "`Using an "In-Tree" ``defconfig``
   File <&YOCTO_DOCS_KERNEL_DEV_URL;#using-an-in-tree-defconfig-file>`__"
   section in the Yocto Project Linux Kernel Development Manual.

KERNEL_ALT_IMAGETYPE
   Specifies an alternate kernel image type for creation in addition to
   the kernel image type specified using the
   ```KERNEL_IMAGETYPE`` <#var-KERNEL_IMAGETYPE>`__ variable.

KERNEL_ARTIFACT_NAME
   Specifies the name of all of the build artifacts. You can change the
   name of the artifacts by changing the ``KERNEL_ARTIFACT_NAME``
   variable.

   The value of ``KERNEL_ARTIFACT_NAME``, which is set in the
   ``meta/classes/kernel-artifact-names.bbclass`` file, has the
   following default value: KERNEL_ARTIFACT_NAME ?=
   "${PKGE}-${PKGV}-${PKGR}-${MACHINE}${IMAGE_VERSION_SUFFIX}"

   See the ```PKGE`` <#var-PKGE>`__, ```PKGV`` <#var-PKGV>`__,
   ```PKGR`` <#var-PKGR>`__, and ```MACHINE`` <#var-MACHINE>`__
   variables for additional information.

      **Note**

      The
      IMAGE_VERSION_SUFFIX
      variable is set to
      DATETIME
      .

KERNEL_CLASSES
   A list of classes defining kernel image types that the
   ```kernel`` <#ref-classes-kernel>`__ class should inherit. You
   typically append this variable to enable extended image types. An
   example is the "kernel-fitimage", which enables fitImage support and
   resides in ``meta/classes/kernel-fitimage.bbclass``. You can register
   custom kernel image types with the ``kernel`` class using this
   variable.

KERNEL_DEVICETREE
   Specifies the name of the generated Linux kernel device tree (i.e.
   the ``.dtb``) file.

      **Note**

      Legacy support exists for specifying the full path to the device
      tree. However, providing just the
      .dtb
      file is preferred.

   In order to use this variable, the
   ```kernel-devicetree`` <#ref-classes-kernel-devicetree>`__ class must
   be inherited.

KERNEL_DTB_LINK_NAME
   The link name of the kernel device tree binary (DTB). This variable
   is set in the ``meta/classes/kernel-artifact-names.bbclass`` file as
   follows: KERNEL_DTB_LINK_NAME ?= "${KERNEL_ARTIFACT_LINK_NAME}" The
   value of the ``KERNEL_ARTIFACT_LINK_NAME`` variable, which is set in
   the same file, has the following value: KERNEL_ARTIFACT_LINK_NAME ?=
   "${MACHINE}"

   See the ```MACHINE`` <#var-MACHINE>`__ variable for additional
   information.

KERNEL_DTB_NAME
   The base name of the kernel device tree binary (DTB). This variable
   is set in the ``meta/classes/kernel-artifact-names.bbclass`` file as
   follows: KERNEL_DTB_NAME ?= "${KERNEL_ARTIFACT_NAME}" The value of
   the ```KERNEL_ARTIFACT_NAME`` <#var-KERNEL_ARTIFACT_NAME>`__
   variable, which is set in the same file, has the following value:
   KERNEL_ARTIFACT_NAME ?=
   "${PKGE}-${PKGV}-${PKGR}-${MACHINE}${IMAGE_VERSION_SUFFIX}"

KERNEL_EXTRA_ARGS
   Specifies additional ``make`` command-line arguments the OpenEmbedded
   build system passes on when compiling the kernel.

KERNEL_FEATURES
   Includes additional kernel metadata. In the OpenEmbedded build
   system, the default Board Support Packages (BSPs)
   `Metadata <#metadata>`__ is provided through the
   ```KMACHINE`` <#var-KMACHINE>`__ and ```KBRANCH`` <#var-KBRANCH>`__
   variables. You can use the ``KERNEL_FEATURES`` variable from within
   the kernel recipe or kernel append file to further add metadata for
   all BSPs or specific BSPs.

   The metadata you add through this variable includes config fragments
   and features descriptions, which usually includes patches as well as
   config fragments. You typically override the ``KERNEL_FEATURES``
   variable for a specific machine. In this way, you can provide
   validated, but optional, sets of kernel configurations and features.

   For example, the following example from the ``linux-yocto-rt_4.12``
   kernel recipe adds "netfilter" and "taskstats" features to all BSPs
   as well as "virtio" configurations to all QEMU machines. The last two
   statements add specific configurations to targeted machine types:
   KERNEL_EXTRA_FEATURES ?= "features/netfilter/netfilter.scc
   features/taskstats/taskstats.scc" KERNEL_FEATURES_append = "
   ${KERNEL_EXTRA_FEATURES}" KERNEL_FEATURES_append_qemuall = "
   cfg/virtio.scc" KERNEL_FEATURES_append_qemux86 = " cfg/sound.scc
   cfg/paravirt_kvm.scc" KERNEL_FEATURES_append_qemux86-64 = "
   cfg/sound.scc"

KERNEL_FIT_LINK_NAME
   The link name of the kernel flattened image tree (FIT) image. This
   variable is set in the ``meta/classes/kernel-artifact-names.bbclass``
   file as follows: KERNEL_FIT_LINK_NAME ?=
   "${KERNEL_ARTIFACT_LINK_NAME}" The value of the
   ``KERNEL_ARTIFACT_LINK_NAME`` variable, which is set in the same
   file, has the following value: KERNEL_ARTIFACT_LINK_NAME ?=
   "${MACHINE}"

   See the ```MACHINE`` <#var-MACHINE>`__ variable for additional
   information.

KERNEL_FIT_NAME
   The base name of the kernel flattened image tree (FIT) image. This
   variable is set in the ``meta/classes/kernel-artifact-names.bbclass``
   file as follows: KERNEL_FIT_NAME ?= "${KERNEL_ARTIFACT_NAME}" The
   value of the ```KERNEL_ARTIFACT_NAME`` <#var-KERNEL_ARTIFACT_NAME>`__
   variable, which is set in the same file, has the following value:
   KERNEL_ARTIFACT_NAME ?=
   "${PKGE}-${PKGV}-${PKGR}-${MACHINE}${IMAGE_VERSION_SUFFIX}"

KERNEL_IMAGE_LINK_NAME
   The link name for the kernel image. This variable is set in the
   ``meta/classes/kernel-artifact-names.bbclass`` file as follows:
   KERNEL_IMAGE_LINK_NAME ?= "${KERNEL_ARTIFACT_LINK_NAME}" The value of
   the ``KERNEL_ARTIFACT_LINK_NAME`` variable, which is set in the same
   file, has the following value: KERNEL_ARTIFACT_LINK_NAME ?=
   "${MACHINE}"

   See the ```MACHINE`` <#var-MACHINE>`__ variable for additional
   information.

KERNEL_IMAGE_MAXSIZE
   Specifies the maximum size of the kernel image file in kilobytes. If
   ``KERNEL_IMAGE_MAXSIZE`` is set, the size of the kernel image file is
   checked against the set value during the
   ```do_sizecheck`` <#ref-tasks-sizecheck>`__ task. The task fails if
   the kernel image file is larger than the setting.

   ``KERNEL_IMAGE_MAXSIZE`` is useful for target devices that have a
   limited amount of space in which the kernel image must be stored.

   By default, this variable is not set, which means the size of the
   kernel image is not checked.

KERNEL_IMAGE_NAME
   The base name of the kernel image. This variable is set in the
   ``meta/classes/kernel-artifact-names.bbclass`` file as follows:
   KERNEL_IMAGE_NAME ?= "${KERNEL_ARTIFACT_NAME}" The value of the
   ```KERNEL_ARTIFACT_NAME`` <#var-KERNEL_ARTIFACT_NAME>`__ variable,
   which is set in the same file, has the following value:
   KERNEL_ARTIFACT_NAME ?=
   "${PKGE}-${PKGV}-${PKGR}-${MACHINE}${IMAGE_VERSION_SUFFIX}"

KERNEL_IMAGETYPE
   The type of kernel to build for a device, usually set by the machine
   configuration files and defaults to "zImage". This variable is used
   when building the kernel and is passed to ``make`` as the target to
   build.

   If you want to build an alternate kernel image type, use the
   ```KERNEL_ALT_IMAGETYPE`` <#var-KERNEL_ALT_IMAGETYPE>`__ variable.

KERNEL_MODULE_AUTOLOAD
   Lists kernel modules that need to be auto-loaded during boot.

      **Note**

      This variable replaces the deprecated
      module_autoload
      variable.

   You can use the ``KERNEL_MODULE_AUTOLOAD`` variable anywhere that it
   can be recognized by the kernel recipe or by an out-of-tree kernel
   module recipe (e.g. a machine configuration file, a distribution
   configuration file, an append file for the recipe, or the recipe
   itself).

   Specify it as follows: KERNEL_MODULE_AUTOLOAD += "module_name1
   module_name2 module_name3"

   Including ``KERNEL_MODULE_AUTOLOAD`` causes the OpenEmbedded build
   system to populate the ``/etc/modules-load.d/modname.conf`` file with
   the list of modules to be auto-loaded on boot. The modules appear
   one-per-line in the file. Here is an example of the most common use
   case: KERNEL_MODULE_AUTOLOAD += "module_name"

   For information on how to populate the ``modname.conf`` file with
   ``modprobe.d`` syntax lines, see the
   ```KERNEL_MODULE_PROBECONF`` <#var-KERNEL_MODULE_PROBECONF>`__
   variable.

KERNEL_MODULE_PROBECONF
   Provides a list of modules for which the OpenEmbedded build system
   expects to find ``module_conf_``\ modname values that specify
   configuration for each of the modules. For information on how to
   provide those module configurations, see the
   ```module_conf_*`` <#var-module_conf>`__ variable.

KERNEL_PATH
   The location of the kernel sources. This variable is set to the value
   of the ```STAGING_KERNEL_DIR`` <#var-STAGING_KERNEL_DIR>`__ within
   the ```module`` <#ref-classes-module>`__ class. For information on
   how this variable is used, see the "`Incorporating Out-of-Tree
   Modules <&YOCTO_DOCS_KERNEL_DEV_URL;#incorporating-out-of-tree-modules>`__"
   section in the Yocto Project Linux Kernel Development Manual.

   To help maximize compatibility with out-of-tree drivers used to build
   modules, the OpenEmbedded build system also recognizes and uses the
   ```KERNEL_SRC`` <#var-KERNEL_SRC>`__ variable, which is identical to
   the ``KERNEL_PATH`` variable. Both variables are common variables
   used by external Makefiles to point to the kernel source directory.

KERNEL_SRC
   The location of the kernel sources. This variable is set to the value
   of the ```STAGING_KERNEL_DIR`` <#var-STAGING_KERNEL_DIR>`__ within
   the ```module`` <#ref-classes-module>`__ class. For information on
   how this variable is used, see the "`Incorporating Out-of-Tree
   Modules <&YOCTO_DOCS_KERNEL_DEV_URL;#incorporating-out-of-tree-modules>`__"
   section in the Yocto Project Linux Kernel Development Manual.

   To help maximize compatibility with out-of-tree drivers used to build
   modules, the OpenEmbedded build system also recognizes and uses the
   ```KERNEL_PATH`` <#var-KERNEL_PATH>`__ variable, which is identical
   to the ``KERNEL_SRC`` variable. Both variables are common variables
   used by external Makefiles to point to the kernel source directory.

KERNEL_VERSION
   Specifies the version of the kernel as extracted from ``version.h``
   or ``utsrelease.h`` within the kernel sources. Effects of setting
   this variable do not take affect until the kernel has been
   configured. Consequently, attempting to refer to this variable in
   contexts prior to configuration will not work.

KERNELDEPMODDEPEND
   Specifies whether the data referenced through
   ```PKGDATA_DIR`` <#var-PKGDATA_DIR>`__ is needed or not. The
   ``KERNELDEPMODDEPEND`` does not control whether or not that data
   exists, but simply whether or not it is used. If you do not need to
   use the data, set the ``KERNELDEPMODDEPEND`` variable in your
   ``initramfs`` recipe. Setting the variable there when the data is not
   needed avoids a potential dependency loop.

KFEATURE_DESCRIPTION
   Provides a short description of a configuration fragment. You use
   this variable in the ``.scc`` file that describes a configuration
   fragment file. Here is the variable used in a file named ``smp.scc``
   to describe SMP being enabled: define KFEATURE_DESCRIPTION "Enable
   SMP"

KMACHINE
   The machine as known by the kernel. Sometimes the machine name used
   by the kernel does not match the machine name used by the
   OpenEmbedded build system. For example, the machine name that the
   OpenEmbedded build system understands as ``core2-32-intel-common``
   goes by a different name in the Linux Yocto kernel. The kernel
   understands that machine as ``intel-core2-32``. For cases like these,
   the ``KMACHINE`` variable maps the kernel machine name to the
   OpenEmbedded build system machine name.

   These mappings between different names occur in the Yocto Linux
   Kernel's ``meta`` branch. As an example take a look in the
   ``common/recipes-kernel/linux/linux-yocto_3.19.bbappend`` file:
   LINUX_VERSION_core2-32-intel-common = "3.19.0"
   COMPATIBLE_MACHINE_core2-32-intel-common = "${MACHINE}"
   SRCREV_meta_core2-32-intel-common =
   "8897ef68b30e7426bc1d39895e71fb155d694974"
   SRCREV_machine_core2-32-intel-common =
   "43b9eced9ba8a57add36af07736344dcc383f711"
   KMACHINE_core2-32-intel-common = "intel-core2-32"
   KBRANCH_core2-32-intel-common = "standard/base"
   KERNEL_FEATURES_append_core2-32-intel-common =
   "${KERNEL_FEATURES_INTEL_COMMON}" The ``KMACHINE`` statement says
   that the kernel understands the machine name as "intel-core2-32".
   However, the OpenEmbedded build system understands the machine as
   "core2-32-intel-common".

KTYPE
   Defines the kernel type to be used in assembling the configuration.
   The linux-yocto recipes define "standard", "tiny", and "preempt-rt"
   kernel types. See the "`Kernel
   Types <&YOCTO_DOCS_KERNEL_DEV_URL;#kernel-types>`__" section in the
   Yocto Project Linux Kernel Development Manual for more information on
   kernel types.

   You define the ``KTYPE`` variable in the `BSP
   Descriptions <&YOCTO_DOCS_KERNEL_DEV_URL;#bsp-descriptions>`__. The
   value you use must match the value used for the
   ```LINUX_KERNEL_TYPE`` <#var-LINUX_KERNEL_TYPE>`__ value used by the
   kernel recipe.

LABELS
   Provides a list of targets for automatic configuration.

   See the ```grub-efi`` <#ref-classes-grub-efi>`__ class for more
   information on how this variable is used.

LAYERDEPENDS
   Lists the layers, separated by spaces, on which this recipe depends.
   Optionally, you can specify a specific layer version for a dependency
   by adding it to the end of the layer name. Here is an example:
   LAYERDEPENDS_mylayer = "anotherlayer (=3)" In this previous example,
   version 3 of "anotherlayer" is compared against
   ```LAYERVERSION`` <#var-LAYERVERSION>`__\ ``_anotherlayer``.

   An error is produced if any dependency is missing or the version
   numbers (if specified) do not match exactly. This variable is used in
   the ``conf/layer.conf`` file and must be suffixed with the name of
   the specific layer (e.g. ``LAYERDEPENDS_mylayer``).

LAYERDIR
   When used inside the ``layer.conf`` configuration file, this variable
   provides the path of the current layer. This variable is not
   available outside of ``layer.conf`` and references are expanded
   immediately when parsing of the file completes.

LAYERRECOMMENDS
   Lists the layers, separated by spaces, recommended for use with this
   layer.

   Optionally, you can specify a specific layer version for a
   recommendation by adding the version to the end of the layer name.
   Here is an example: LAYERRECOMMENDS_mylayer = "anotherlayer (=3)" In
   this previous example, version 3 of "anotherlayer" is compared
   against ``LAYERVERSION_anotherlayer``.

   This variable is used in the ``conf/layer.conf`` file and must be
   suffixed with the name of the specific layer (e.g.
   ``LAYERRECOMMENDS_mylayer``).

LAYERSERIES_COMPAT
   Lists the versions of the `OpenEmbedded-Core <#oe-core>`__ for which
   a layer is compatible. Using the ``LAYERSERIES_COMPAT`` variable
   allows the layer maintainer to indicate which combinations of the
   layer and OE-Core can be expected to work. The variable gives the
   system a way to detect when a layer has not been tested with new
   releases of OE-Core (e.g. the layer is not maintained).

   To specify the OE-Core versions for which a layer is compatible, use
   this variable in your layer's ``conf/layer.conf`` configuration file.
   For the list, use the Yocto Project `Release
   Name <https://wiki.yoctoproject.org/wiki/Releases>`__ (e.g.
   DISTRO_NAME_NO_CAP). To specify multiple OE-Core versions for the
   layer, use a space-separated list: LAYERSERIES_COMPAT_layer_root_name
   = "DISTRO_NAME_NO_CAP DISTRO_NAME_NO_CAP_MINUS_ONE"

      **Note**

      Setting
      LAYERSERIES_COMPAT
      is required by the Yocto Project Compatible version 2 standard.
      The OpenEmbedded build system produces a warning if the variable
      is not set for any given layer.

   See the "`Creating Your Own
   Layer <&YOCTO_DOCS_DEV_URL;#creating-your-own-layer>`__" section in
   the Yocto Project Development Tasks Manual.

LAYERVERSION
   Optionally specifies the version of a layer as a single number. You
   can use this within ```LAYERDEPENDS`` <#var-LAYERDEPENDS>`__ for
   another layer in order to depend on a specific version of the layer.
   This variable is used in the ``conf/layer.conf`` file and must be
   suffixed with the name of the specific layer (e.g.
   ``LAYERVERSION_mylayer``).

LD
   The minimal command and arguments used to run the linker.

LDFLAGS
   Specifies the flags to pass to the linker. This variable is exported
   to an environment variable and thus made visible to the software
   being built during the compilation step.

   Default initialization for ``LDFLAGS`` varies depending on what is
   being built:

   -  ```TARGET_LDFLAGS`` <#var-TARGET_LDFLAGS>`__ when building for the
      target

   -  ```BUILD_LDFLAGS`` <#var-BUILD_LDFLAGS>`__ when building for the
      build host (i.e. ``-native``)

   -  ```BUILDSDK_LDFLAGS`` <#var-BUILDSDK_LDFLAGS>`__ when building for
      an SDK (i.e. ``nativesdk-``)

LEAD_SONAME
   Specifies the lead (or primary) compiled library file (i.e. ``.so``)
   that the ```debian`` <#ref-classes-debian>`__ class applies its
   naming policy to given a recipe that packages multiple libraries.

   This variable works in conjunction with the ``debian`` class.

LIC_FILES_CHKSUM
   Checksums of the license text in the recipe source code.

   This variable tracks changes in license text of the source code
   files. If the license text is changed, it will trigger a build
   failure, which gives the developer an opportunity to review any
   license change.

   This variable must be defined for all recipes (unless
   ```LICENSE`` <#var-LICENSE>`__ is set to "CLOSED").

   For more information, see the "`Tracking License
   Changes <&YOCTO_DOCS_DEV_URL;#usingpoky-configuring-LIC_FILES_CHKSUM>`__"
   section in the Yocto Project Development Tasks Manual.

LICENSE
   The list of source licenses for the recipe. Follow these rules:

   -  Do not use spaces within individual license names.

   -  Separate license names using \| (pipe) when there is a choice
      between licenses.

   -  Separate license names using & (ampersand) when multiple licenses
      exist that cover different parts of the source.

   -  You can use spaces between license names.

   -  For standard licenses, use the names of the files in
      ``meta/files/common-licenses/`` or the
      ```SPDXLICENSEMAP`` <#var-SPDXLICENSEMAP>`__ flag names defined in
      ``meta/conf/licenses.conf``.

   Here are some examples: LICENSE = "LGPLv2.1 \| GPLv3" LICENSE =
   "MPL-1 & LGPLv2.1" LICENSE = "GPLv2+" The first example is from the
   recipes for Qt, which the user may choose to distribute under either
   the LGPL version 2.1 or GPL version 3. The second example is from
   Cairo where two licenses cover different parts of the source code.
   The final example is from ``sysstat``, which presents a single
   license.

   You can also specify licenses on a per-package basis to handle
   situations where components of the output have different licenses.
   For example, a piece of software whose code is licensed under GPLv2
   but has accompanying documentation licensed under the GNU Free
   Documentation License 1.2 could be specified as follows: LICENSE =
   "GFDL-1.2 & GPLv2" LICENSE_${PN} = "GPLv2" LICENSE_${PN}-doc =
   "GFDL-1.2"

LICENSE_CREATE_PACKAGE
   Setting ``LICENSE_CREATE_PACKAGE`` to "1" causes the OpenEmbedded
   build system to create an extra package (i.e.
   ``${``\ ```PN`` <#var-PN>`__\ ``}-lic``) for each recipe and to add
   those packages to the
   ```RRECOMMENDS`` <#var-RRECOMMENDS>`__\ ``_${PN}``.

   The ``${PN}-lic`` package installs a directory in
   ``/usr/share/licenses`` named ``${PN}``, which is the recipe's base
   name, and installs files in that directory that contain license and
   copyright information (i.e. copies of the appropriate license files
   from ``meta/common-licenses`` that match the licenses specified in
   the ```LICENSE`` <#var-LICENSE>`__ variable of the recipe metadata
   and copies of files marked in
   ```LIC_FILES_CHKSUM`` <#var-LIC_FILES_CHKSUM>`__ as containing
   license text).

   For related information on providing license text, see the
   ```COPY_LIC_DIRS`` <#var-COPY_LIC_DIRS>`__ variable, the
   ```COPY_LIC_MANIFEST`` <#var-COPY_LIC_MANIFEST>`__ variable, and the
   "`Providing License
   Text <&YOCTO_DOCS_DEV_URL;#providing-license-text>`__" section in the
   Yocto Project Development Tasks Manual.

LICENSE_FLAGS
   Specifies additional flags for a recipe you must whitelist through
   ```LICENSE_FLAGS_WHITELIST`` <#var-LICENSE_FLAGS_WHITELIST>`__ in
   order to allow the recipe to be built. When providing multiple flags,
   separate them with spaces.

   This value is independent of ```LICENSE`` <#var-LICENSE>`__ and is
   typically used to mark recipes that might require additional licenses
   in order to be used in a commercial product. For more information,
   see the "`Enabling Commercially Licensed
   Recipes <&YOCTO_DOCS_DEV_URL;#enabling-commercially-licensed-recipes>`__"
   section in the Yocto Project Development Tasks Manual.

LICENSE_FLAGS_WHITELIST
   Lists license flags that when specified in
   ```LICENSE_FLAGS`` <#var-LICENSE_FLAGS>`__ within a recipe should not
   prevent that recipe from being built. This practice is otherwise
   known as "whitelisting" license flags. For more information, see the
   "`Enabling Commercially Licensed
   Recipes <&YOCTO_DOCS_DEV_URL;#enabling-commercially-licensed-recipes>`__"
   section in the Yocto Project Development Tasks Manual.

LICENSE_PATH
   Path to additional licenses used during the build. By default, the
   OpenEmbedded build system uses ``COMMON_LICENSE_DIR`` to define the
   directory that holds common license text used during the build. The
   ``LICENSE_PATH`` variable allows you to extend that location to other
   areas that have additional licenses: LICENSE_PATH +=
   "path-to-additional-common-licenses"

LINUX_KERNEL_TYPE
   Defines the kernel type to be used in assembling the configuration.
   The linux-yocto recipes define "standard", "tiny", and "preempt-rt"
   kernel types. See the "`Kernel
   Types <&YOCTO_DOCS_KERNEL_DEV_URL;#kernel-types>`__" section in the
   Yocto Project Linux Kernel Development Manual for more information on
   kernel types.

   If you do not specify a ``LINUX_KERNEL_TYPE``, it defaults to
   "standard". Together with ```KMACHINE`` <#var-KMACHINE>`__, the
   ``LINUX_KERNEL_TYPE`` variable defines the search arguments used by
   the kernel tools to find the appropriate description within the
   kernel `Metadata <#metadata>`__ with which to build out the sources
   and configuration.

LINUX_VERSION
   The Linux version from ``kernel.org`` on which the Linux kernel image
   being built using the OpenEmbedded build system is based. You define
   this variable in the kernel recipe. For example, the
   ``linux-yocto-3.4.bb`` kernel recipe found in
   ``meta/recipes-kernel/linux`` defines the variables as follows:
   LINUX_VERSION ?= "3.4.24"

   The ``LINUX_VERSION`` variable is used to define ```PV`` <#var-PV>`__
   for the recipe: PV = "${LINUX_VERSION}+git${SRCPV}"

LINUX_VERSION_EXTENSION
   A string extension compiled into the version string of the Linux
   kernel built with the OpenEmbedded build system. You define this
   variable in the kernel recipe. For example, the linux-yocto kernel
   recipes all define the variable as follows: LINUX_VERSION_EXTENSION
   ?= "-yocto-${`LINUX_KERNEL_TYPE <#var-LINUX_KERNEL_TYPE>`__}"

   Defining this variable essentially sets the Linux kernel
   configuration item ``CONFIG_LOCALVERSION``, which is visible through
   the ``uname`` command. Here is an example that shows the extension
   assuming it was set as previously shown: $ uname -r 3.7.0-rc8-custom

LOG_DIR
   Specifies the directory to which the OpenEmbedded build system writes
   overall log files. The default directory is ``${TMPDIR}/log``.

   For the directory containing logs specific to each task, see the
   ```T`` <#var-T>`__ variable.

MACHINE
   Specifies the target device for which the image is built. You define
   ``MACHINE`` in the ``local.conf`` file found in the `Build
   Directory <#build-directory>`__. By default, ``MACHINE`` is set to
   "qemux86", which is an x86-based architecture machine to be emulated
   using QEMU: MACHINE ?= "qemux86"

   The variable corresponds to a machine configuration file of the same
   name, through which machine-specific configurations are set. Thus,
   when ``MACHINE`` is set to "qemux86" there exists the corresponding
   ``qemux86.conf`` machine configuration file, which can be found in
   the `Source Directory <#source-directory>`__ in
   ``meta/conf/machine``.

   The list of machines supported by the Yocto Project as shipped
   include the following: MACHINE ?= "qemuarm" MACHINE ?= "qemuarm64"
   MACHINE ?= "qemumips" MACHINE ?= "qemumips64" MACHINE ?= "qemuppc"
   MACHINE ?= "qemux86" MACHINE ?= "qemux86-64" MACHINE ?= "genericx86"
   MACHINE ?= "genericx86-64" MACHINE ?= "beaglebone" MACHINE ?=
   "edgerouter" The last five are Yocto Project reference hardware
   boards, which are provided in the ``meta-yocto-bsp`` layer.

      **Note**

      Adding additional Board Support Package (BSP) layers to your
      configuration adds new possible settings for
      MACHINE
      .

MACHINE_ARCH
   Specifies the name of the machine-specific architecture. This
   variable is set automatically from ```MACHINE`` <#var-MACHINE>`__ or
   ```TUNE_PKGARCH`` <#var-TUNE_PKGARCH>`__. You should not hand-edit
   the ``MACHINE_ARCH`` variable.

MACHINE_ESSENTIAL_EXTRA_RDEPENDS
   A list of required machine-specific packages to install as part of
   the image being built. The build process depends on these packages
   being present. Furthermore, because this is a "machine-essential"
   variable, the list of packages are essential for the machine to boot.
   The impact of this variable affects images based on
   ``packagegroup-core-boot``, including the ``core-image-minimal``
   image.

   This variable is similar to the
   ``MACHINE_ESSENTIAL_EXTRA_RRECOMMENDS`` variable with the exception
   that the image being built has a build dependency on the variable's
   list of packages. In other words, the image will not build if a file
   in this list is not found.

   As an example, suppose the machine for which you are building
   requires ``example-init`` to be run during boot to initialize the
   hardware. In this case, you would use the following in the machine's
   ``.conf`` configuration file: MACHINE_ESSENTIAL_EXTRA_RDEPENDS +=
   "example-init"

MACHINE_ESSENTIAL_EXTRA_RRECOMMENDS
   A list of recommended machine-specific packages to install as part of
   the image being built. The build process does not depend on these
   packages being present. However, because this is a
   "machine-essential" variable, the list of packages are essential for
   the machine to boot. The impact of this variable affects images based
   on ``packagegroup-core-boot``, including the ``core-image-minimal``
   image.

   This variable is similar to the ``MACHINE_ESSENTIAL_EXTRA_RDEPENDS``
   variable with the exception that the image being built does not have
   a build dependency on the variable's list of packages. In other
   words, the image will still build if a package in this list is not
   found. Typically, this variable is used to handle essential kernel
   modules, whose functionality may be selected to be built into the
   kernel rather than as a module, in which case a package will not be
   produced.

   Consider an example where you have a custom kernel where a specific
   touchscreen driver is required for the machine to be usable. However,
   the driver can be built as a module or into the kernel depending on
   the kernel configuration. If the driver is built as a module, you
   want it to be installed. But, when the driver is built into the
   kernel, you still want the build to succeed. This variable sets up a
   "recommends" relationship so that in the latter case, the build will
   not fail due to the missing package. To accomplish this, assuming the
   package for the module was called ``kernel-module-ab123``, you would
   use the following in the machine's ``.conf`` configuration file:
   MACHINE_ESSENTIAL_EXTRA_RRECOMMENDS += "kernel-module-ab123"

      **Note**

      In this example, the
      kernel-module-ab123
      recipe needs to explicitly set its
      PACKAGES
      variable to ensure that BitBake does not use the kernel recipe's
      PACKAGES_DYNAMIC
      variable to satisfy the dependency.

   Some examples of these machine essentials are flash, screen,
   keyboard, mouse, or touchscreen drivers (depending on the machine).

MACHINE_EXTRA_RDEPENDS
   A list of machine-specific packages to install as part of the image
   being built that are not essential for the machine to boot. However,
   the build process for more fully-featured images depends on the
   packages being present.

   This variable affects all images based on ``packagegroup-base``,
   which does not include the ``core-image-minimal`` or
   ``core-image-full-cmdline`` images.

   The variable is similar to the ``MACHINE_EXTRA_RRECOMMENDS`` variable
   with the exception that the image being built has a build dependency
   on the variable's list of packages. In other words, the image will
   not build if a file in this list is not found.

   An example is a machine that has WiFi capability but is not essential
   for the machine to boot the image. However, if you are building a
   more fully-featured image, you want to enable the WiFi. The package
   containing the firmware for the WiFi hardware is always expected to
   exist, so it is acceptable for the build process to depend upon
   finding the package. In this case, assuming the package for the
   firmware was called ``wifidriver-firmware``, you would use the
   following in the ``.conf`` file for the machine:
   MACHINE_EXTRA_RDEPENDS += "wifidriver-firmware"

MACHINE_EXTRA_RRECOMMENDS
   A list of machine-specific packages to install as part of the image
   being built that are not essential for booting the machine. The image
   being built has no build dependency on this list of packages.

   This variable affects only images based on ``packagegroup-base``,
   which does not include the ``core-image-minimal`` or
   ``core-image-full-cmdline`` images.

   This variable is similar to the ``MACHINE_EXTRA_RDEPENDS`` variable
   with the exception that the image being built does not have a build
   dependency on the variable's list of packages. In other words, the
   image will build if a file in this list is not found.

   An example is a machine that has WiFi capability but is not essential
   For the machine to boot the image. However, if you are building a
   more fully-featured image, you want to enable WiFi. In this case, the
   package containing the WiFi kernel module will not be produced if the
   WiFi driver is built into the kernel, in which case you still want
   the build to succeed instead of failing as a result of the package
   not being found. To accomplish this, assuming the package for the
   module was called ``kernel-module-examplewifi``, you would use the
   following in the ``.conf`` file for the machine:
   MACHINE_EXTRA_RRECOMMENDS += "kernel-module-examplewifi"

MACHINE_FEATURES
   Specifies the list of hardware features the
   ```MACHINE`` <#var-MACHINE>`__ is capable of supporting. For related
   information on enabling features, see the
   ```DISTRO_FEATURES`` <#var-DISTRO_FEATURES>`__,
   ```COMBINED_FEATURES`` <#var-COMBINED_FEATURES>`__, and
   ```IMAGE_FEATURES`` <#var-IMAGE_FEATURES>`__ variables.

   For a list of hardware features supported by the Yocto Project as
   shipped, see the "`Machine Features <#ref-features-machine>`__"
   section.

MACHINE_FEATURES_BACKFILL
   Features to be added to ``MACHINE_FEATURES`` if not also present in
   ``MACHINE_FEATURES_BACKFILL_CONSIDERED``.

   This variable is set in the ``meta/conf/bitbake.conf`` file. It is
   not intended to be user-configurable. It is best to just reference
   the variable to see which machine features are being backfilled for
   all machine configurations. See the "`Feature
   Backfilling <#ref-features-backfill>`__" section for more
   information.

MACHINE_FEATURES_BACKFILL_CONSIDERED
   Features from ``MACHINE_FEATURES_BACKFILL`` that should not be
   backfilled (i.e. added to ``MACHINE_FEATURES``) during the build. See
   the "`Feature Backfilling <#ref-features-backfill>`__" section for
   more information.

MACHINEOVERRIDES
   A colon-separated list of overrides that apply to the current
   machine. By default, this list includes the value of
   ```MACHINE`` <#var-MACHINE>`__.

   You can extend ``MACHINEOVERRIDES`` to add extra overrides that
   should apply to a machine. For example, all machines emulated in QEMU
   (e.g. ``qemuarm``, ``qemux86``, and so forth) include a file named
   ``meta/conf/machine/include/qemu.inc`` that prepends the following
   override to ``MACHINEOVERRIDES``: MACHINEOVERRIDES =. "qemuall:" This
   override allows variables to be overriden for all machines emulated
   in QEMU, like in the following example from the ``connman-conf``
   recipe: SRC_URI_append_qemuall = "file://wired.config \\
   file://wired-setup \\ " The underlying mechanism behind
   ``MACHINEOVERRIDES`` is simply that it is included in the default
   value of ```OVERRIDES`` <#var-OVERRIDES>`__.

MAINTAINER
   The email address of the distribution maintainer.

MIRRORS
   Specifies additional paths from which the OpenEmbedded build system
   gets source code. When the build system searches for source code, it
   first tries the local download directory. If that location fails, the
   build system tries locations defined by
   ```PREMIRRORS`` <#var-PREMIRRORS>`__, the upstream source, and then
   locations specified by ``MIRRORS`` in that order.

   Assuming your distribution (```DISTRO`` <#var-DISTRO>`__) is "poky",
   the default value for ``MIRRORS`` is defined in the
   ``conf/distro/poky.conf`` file in the ``meta-poky`` Git repository.

MLPREFIX
   Specifies a prefix has been added to ```PN`` <#var-PN>`__ to create a
   special version of a recipe or package (i.e. a Multilib version). The
   variable is used in places where the prefix needs to be added to or
   removed from a the name (e.g. the ```BPN`` <#var-BPN>`__ variable).
   ``MLPREFIX`` gets set when a prefix has been added to ``PN``.

      **Note**

      The "ML" in
      MLPREFIX
      stands for "MultiLib". This representation is historical and comes
      from a time when
      nativesdk
      was a suffix rather than a prefix on the recipe name. When
      nativesdk
      was turned into a prefix, it made sense to set
      MLPREFIX
      for it as well.

   To help understand when ``MLPREFIX`` might be needed, consider when
   ```BBCLASSEXTEND`` <#var-BBCLASSEXTEND>`__ is used to provide a
   ``nativesdk`` version of a recipe in addition to the target version.
   If that recipe declares build-time dependencies on tasks in other
   recipes by using ```DEPENDS`` <#var-DEPENDS>`__, then a dependency on
   "foo" will automatically get rewritten to a dependency on
   "nativesdk-foo". However, dependencies like the following will not
   get rewritten automatically: do_foo[depends] += "recipe:do_foo" If
   you want such a dependency to also get transformed, you can do the
   following: do_foo[depends] += "${MLPREFIX}recipe:do_foo"

module_autoload
   This variable has been replaced by the ``KERNEL_MODULE_AUTOLOAD``
   variable. You should replace all occurrences of ``module_autoload``
   with additions to ``KERNEL_MODULE_AUTOLOAD``, for example:
   module_autoload_rfcomm = "rfcomm"

   should now be replaced with: KERNEL_MODULE_AUTOLOAD += "rfcomm" See
   the ```KERNEL_MODULE_AUTOLOAD`` <#var-KERNEL_MODULE_AUTOLOAD>`__
   variable for more information.

module_conf
   Specifies ```modprobe.d`` <http://linux.die.net/man/5/modprobe.d>`__
   syntax lines for inclusion in the ``/etc/modprobe.d/modname.conf``
   file.

   You can use this variable anywhere that it can be recognized by the
   kernel recipe or out-of-tree kernel module recipe (e.g. a machine
   configuration file, a distribution configuration file, an append file
   for the recipe, or the recipe itself). If you use this variable, you
   must also be sure to list the module name in the
   ```KERNEL_MODULE_AUTOLOAD`` <#var-KERNEL_MODULE_AUTOLOAD>`__
   variable.

   Here is the general syntax: module_conf_module_name =
   "modprobe.d-syntax" You must use the kernel module name override.

   Run ``man modprobe.d`` in the shell to find out more information on
   the exact syntax you want to provide with ``module_conf``.

   Including ``module_conf`` causes the OpenEmbedded build system to
   populate the ``/etc/modprobe.d/modname.conf`` file with
   ``modprobe.d`` syntax lines. Here is an example that adds the options
   ``arg1`` and ``arg2`` to a module named ``mymodule``:
   module_conf_mymodule = "options mymodule arg1=val1 arg2=val2"

   For information on how to specify kernel modules to auto-load on
   boot, see the
   ```KERNEL_MODULE_AUTOLOAD`` <#var-KERNEL_MODULE_AUTOLOAD>`__
   variable.

MODULE_TARBALL_DEPLOY
   Controls creation of the ``modules-*.tgz`` file. Set this variable to
   "0" to disable creation of this file, which contains all of the
   kernel modules resulting from a kernel build.

MODULE_TARBALL_LINK_NAME
   The link name of the kernel module tarball. This variable is set in
   the ``meta/classes/kernel-artifact-names.bbclass`` file as follows:
   MODULE_TARBALL_LINK_NAME ?= "${KERNEL_ARTIFACT_LINK_NAME}" The value
   of the ``KERNEL_ARTIFACT_LINK_NAME`` variable, which is set in the
   same file, has the following value: KERNEL_ARTIFACT_LINK_NAME ?=
   "${MACHINE}"

   See the ```MACHINE`` <#var-MACHINE>`__ variable for additional
   information.

MODULE_TARBALL_NAME
   The base name of the kernel module tarball. This variable is set in
   the ``meta/classes/kernel-artifact-names.bbclass`` file as follows:
   MODULE_TARBALL_NAME ?= "${KERNEL_ARTIFACT_NAME}" The value of the
   ```KERNEL_ARTIFACT_NAME`` <#var-KERNEL_ARTIFACT_NAME>`__ variable,
   which is set in the same file, has the following value:
   KERNEL_ARTIFACT_NAME ?=
   "${PKGE}-${PKGV}-${PKGR}-${MACHINE}${IMAGE_VERSION_SUFFIX}"

MULTIMACH_TARGET_SYS
   Uniquely identifies the type of the target system for which packages
   are being built. This variable allows output for different types of
   target systems to be put into different subdirectories of the same
   output directory.

   The default value of this variable is:
   ${PACKAGE_ARCH}${TARGET_VENDOR}-${TARGET_OS} Some classes (e.g.
   ```cross-canadian`` <#ref-classes-cross-canadian>`__) modify the
   ``MULTIMACH_TARGET_SYS`` value.

   See the ```STAMP`` <#var-STAMP>`__ variable for an example. See the
   ```STAGING_DIR_TARGET`` <#var-STAGING_DIR_TARGET>`__ variable for
   more information.

NATIVELSBSTRING
   A string identifying the host distribution. Strings consist of the
   host distributor ID followed by the release, as reported by the
   ``lsb_release`` tool or as read from ``/etc/lsb-release``. For
   example, when running a build on Ubuntu 12.10, the value is
   "Ubuntu-12.10". If this information is unable to be determined, the
   value resolves to "Unknown".

   This variable is used by default to isolate native shared state
   packages for different distributions (e.g. to avoid problems with
   ``glibc`` version incompatibilities). Additionally, the variable is
   checked against
   ```SANITY_TESTED_DISTROS`` <#var-SANITY_TESTED_DISTROS>`__ if that
   variable is set.

NM
   The minimal command and arguments to run ``nm``.

NO_GENERIC_LICENSE
   Avoids QA errors when you use a non-common, non-CLOSED license in a
   recipe. Packages exist, such as the linux-firmware package, with many
   licenses that are not in any way common. Also, new licenses are added
   occasionally to avoid introducing a lot of common license files,
   which are only applicable to a specific package.
   ``NO_GENERIC_LICENSE`` is used to allow copying a license that does
   not exist in common licenses.

   The following example shows how to add ``NO_GENERIC_LICENSE`` to a
   recipe: NO_GENERIC_LICENSE[license_name] =
   "license_file_in_fetched_source" The following is an example that
   uses the ``LICENSE.Abilis.txt`` file as the license from the fetched
   source: NO_GENERIC_LICENSE[Firmware-Abilis] = "LICENSE.Abilis.txt"

NO_RECOMMENDATIONS
   Prevents installation of all "recommended-only" packages.
   Recommended-only packages are packages installed only through the
   ```RRECOMMENDS`` <#var-RRECOMMENDS>`__ variable). Setting the
   ``NO_RECOMMENDATIONS`` variable to "1" turns this feature on:
   NO_RECOMMENDATIONS = "1"

   You can set this variable globally in your ``local.conf`` file or you
   can attach it to a specific image recipe by using the recipe name
   override: NO_RECOMMENDATIONS_pn-target_image = "1"

   It is important to realize that if you choose to not install packages
   using this variable and some other packages are dependent on them
   (i.e. listed in a recipe's ```RDEPENDS`` <#var-RDEPENDS>`__
   variable), the OpenEmbedded build system ignores your request and
   will install the packages to avoid dependency errors.

      **Note**

      Some recommended packages might be required for certain system
      functionality, such as kernel modules. It is up to you to add
      packages with the
      IMAGE_INSTALL
      variable.

   Support for this variable exists only when using the IPK and RPM
   packaging backend. Support does not exist for DEB.

   See the ```BAD_RECOMMENDATIONS`` <#var-BAD_RECOMMENDATIONS>`__ and
   the ```PACKAGE_EXCLUDE`` <#var-PACKAGE_EXCLUDE>`__ variables for
   related information.

NOAUTOPACKAGEDEBUG
   Disables auto package from splitting ``.debug`` files. If a recipe
   requires ``FILES_${PN}-dbg`` to be set manually, the
   ``NOAUTOPACKAGEDEBUG`` can be defined allowing you to define the
   content of the debug package. For example: NOAUTOPACKAGEDEBUG = "1"
   FILES_${PN}-dev = "${includedir}/${QT_DIR_NAME}/Qt/*" FILES_${PN}-dbg
   = "/usr/src/debug/" FILES_${QT_BASE_NAME}-demos-doc =
   "${docdir}/${QT_DIR_NAME}/qch/qt.qch"

OBJCOPY
   The minimal command and arguments to run ``objcopy``.

OBJDUMP
   The minimal command and arguments to run ``objdump``.

OE_BINCONFIG_EXTRA_MANGLE
   When inheriting the ```binconfig`` <#ref-classes-binconfig>`__ class,
   this variable specifies additional arguments passed to the "sed"
   command. The sed command alters any paths in configuration scripts
   that have been set up during compilation. Inheriting this class
   results in all paths in these scripts being changed to point into the
   ``sysroots/`` directory so that all builds that use the script will
   use the correct directories for the cross compiling layout.

   See the ``meta/classes/binconfig.bbclass`` in the `Source
   Directory <#source-directory>`__ for details on how this class
   applies these additional sed command arguments. For general
   information on the ``binconfig`` class, see the
   "```binconfig.bbclass`` <#ref-classes-binconfig>`__" section.

OE_IMPORTS
   An internal variable used to tell the OpenEmbedded build system what
   Python modules to import for every Python function run by the system.

      **Note**

      Do not set this variable. It is for internal use only.

OE_INIT_ENV_SCRIPT
   The name of the build environment setup script for the purposes of
   setting up the environment within the extensible SDK. The default
   value is "oe-init-build-env".

   If you use a custom script to set up your build environment, set the
   ``OE_INIT_ENV_SCRIPT`` variable to its name.

OE_TERMINAL
   Controls how the OpenEmbedded build system spawns interactive
   terminals on the host development system (e.g. using the BitBake
   command with the ``-c devshell`` command-line option). For more
   information, see the "`Using a Development
   Shell <&YOCTO_DOCS_DEV_URL;#platdev-appdev-devshell>`__" section in
   the Yocto Project Development Tasks Manual.

   You can use the following values for the ``OE_TERMINAL`` variable:
   auto gnome xfce rxvt screen konsole none

OEROOT
   The directory from which the top-level build environment setup script
   is sourced. The Yocto Project provides a top-level build environment
   setup script: ````` <#structure-core-script>`__. When you run this
   script, the ``OEROOT`` variable resolves to the directory that
   contains the script.

   For additional information on how this variable is used, see the
   initialization script.

OLDEST_KERNEL
   Declares the oldest version of the Linux kernel that the produced
   binaries must support. This variable is passed into the build of the
   Embedded GNU C Library (``glibc``).

   The default for this variable comes from the
   ``meta/conf/bitbake.conf`` configuration file. You can override this
   default by setting the variable in a custom distribution
   configuration file.

OVERRIDES
   A colon-separated list of overrides that currently apply. Overrides
   are a BitBake mechanism that allows variables to be selectively
   overridden at the end of parsing. The set of overrides in
   ``OVERRIDES`` represents the "state" during building, which includes
   the current recipe being built, the machine for which it is being
   built, and so forth.

   As an example, if the string "an-override" appears as an element in
   the colon-separated list in ``OVERRIDES``, then the following
   assignment will override ``FOO`` with the value "overridden" at the
   end of parsing: FOO_an-override = "overridden" See the "`Conditional
   Syntax
   (Overrides) <&YOCTO_DOCS_BB_URL;#conditional-syntax-overrides>`__"
   section in the BitBake User Manual for more information on the
   overrides mechanism.

   The default value of ``OVERRIDES`` includes the values of the
   ```CLASSOVERRIDE`` <#var-CLASSOVERRIDE>`__,
   ```MACHINEOVERRIDES`` <#var-MACHINEOVERRIDES>`__, and
   ```DISTROOVERRIDES`` <#var-DISTROOVERRIDES>`__ variables. Another
   important override included by default is ``pn-${PN}``. This override
   allows variables to be set for a single recipe within configuration
   (``.conf``) files. Here is an example: FOO_pn-myrecipe =
   "myrecipe-specific value"

      **Note**

      An easy way to see what overrides apply is to search for
      OVERRIDES
      in the output of the
      bitbake -e
      command. See the "
      Viewing Variable Values
      " section in the Yocto Project Development Tasks Manual for more
      information.

P
   The recipe name and version. ``P`` is comprised of the following:
   ${PN}-${PV}

PACKAGE_ARCH
   The architecture of the resulting package or packages.

   By default, the value of this variable is set to
   ```TUNE_PKGARCH`` <#var-TUNE_PKGARCH>`__ when building for the
   target, ```BUILD_ARCH`` <#var-BUILD_ARCH>`__ when building for the
   build host, and "${SDK_ARCH}-${SDKPKGSUFFIX}" when building for the
   SDK.

      **Note**

      See
      SDK_ARCH
      for more information.

   However, if your recipe's output packages are built specific to the
   target machine rather than generally for the architecture of the
   machine, you should set ``PACKAGE_ARCH`` to the value of
   ```MACHINE_ARCH`` <#var-MACHINE_ARCH>`__ in the recipe as follows:
   PACKAGE_ARCH = "${MACHINE_ARCH}"

PACKAGE_ARCHS
   Specifies a list of architectures compatible with the target machine.
   This variable is set automatically and should not normally be
   hand-edited. Entries are separated using spaces and listed in order
   of priority. The default value for ``PACKAGE_ARCHS`` is "all any
   noarch ${PACKAGE_EXTRA_ARCHS} ${MACHINE_ARCH}".

PACKAGE_BEFORE_PN
   Enables easily adding packages to ``PACKAGES`` before ``${PN}`` so
   that those added packages can pick up files that would normally be
   included in the default package.

PACKAGE_CLASSES
   This variable, which is set in the ``local.conf`` configuration file
   found in the ``conf`` folder of the `Build
   Directory <#build-directory>`__, specifies the package manager the
   OpenEmbedded build system uses when packaging data.

   You can provide one or more of the following arguments for the
   variable: PACKAGE_CLASSES ?= "package_rpm package_deb package_ipk
   package_tar"

      **Note**

      While it is a legal option, the
      package_tar
      class has limited functionality due to no support for package
      dependencies by that backend. Therefore, it is recommended that
      you do not use it.

   The build system uses only the first argument in the list as the
   package manager when creating your image or SDK. However, packages
   will be created using any additional packaging classes you specify.
   For example, if you use the following in your ``local.conf`` file:
   PACKAGE_CLASSES ?= "package_ipk" The OpenEmbedded build system uses
   the IPK package manager to create your image or SDK.

   For information on packaging and build performance effects as a
   result of the package manager in use, see the
   "```package.bbclass`` <#ref-classes-package>`__" section.

PACKAGE_DEBUG_SPLIT_STYLE
   Determines how to split up the binary and debug information when
   creating ``*-dbg`` packages to be used with the GNU Project Debugger
   (GDB).

   With the ``PACKAGE_DEBUG_SPLIT_STYLE`` variable, you can control
   where debug information, which can include or exclude source files,
   is stored:

   -  ".debug": Debug symbol files are placed next to the binary in a
      ``.debug`` directory on the target. For example, if a binary is
      installed into ``/bin``, the corresponding debug symbol files are
      installed in ``/bin/.debug``. Source files are placed in
      ``/usr/src/debug``.

   -  "debug-file-directory": Debug symbol files are placed under
      ``/usr/lib/debug`` on the target, and separated by the path from
      where the binary is installed. For example, if a binary is
      installed in ``/bin``, the corresponding debug symbols are
      installed in ``/usr/lib/debug/bin``. Source files are placed in
      ``/usr/src/debug``.

   -  "debug-without-src": The same behavior as ".debug" previously
      described with the exception that no source files are installed.

   -  "debug-with-srcpkg": The same behavior as ".debug" previously
      described with the exception that all source files are placed in a
      separate ``*-src`` pkg. This is the default behavior.

   You can find out more about debugging using GDB by reading the
   "`Debugging With the GNU Project Debugger (GDB)
   Remotely <&YOCTO_DOCS_DEV_URL;#platdev-gdb-remotedebug>`__" section
   in the Yocto Project Development Tasks Manual.

PACKAGE_EXCLUDE_COMPLEMENTARY
   Prevents specific packages from being installed when you are
   installing complementary packages.

   You might find that you want to prevent installing certain packages
   when you are installing complementary packages. For example, if you
   are using ```IMAGE_FEATURES`` <#var-IMAGE_FEATURES>`__ to install
   ``dev-pkgs``, you might not want to install all packages from a
   particular multilib. If you find yourself in this situation, you can
   use the ``PACKAGE_EXCLUDE_COMPLEMENTARY`` variable to specify regular
   expressions to match the packages you want to exclude.

PACKAGE_EXCLUDE
   Lists packages that should not be installed into an image. For
   example: PACKAGE_EXCLUDE = "package_name package_name package_name
   ..."

   You can set this variable globally in your ``local.conf`` file or you
   can attach it to a specific image recipe by using the recipe name
   override: PACKAGE_EXCLUDE_pn-target_image = "package_name"

   If you choose to not install a package using this variable and some
   other package is dependent on it (i.e. listed in a recipe's
   ```RDEPENDS`` <#var-RDEPENDS>`__ variable), the OpenEmbedded build
   system generates a fatal installation error. Because the build system
   halts the process with a fatal error, you can use the variable with
   an iterative development process to remove specific components from a
   system.

   Support for this variable exists only when using the IPK and RPM
   packaging backend. Support does not exist for DEB.

   See the ```NO_RECOMMENDATIONS`` <#var-NO_RECOMMENDATIONS>`__ and the
   ```BAD_RECOMMENDATIONS`` <#var-BAD_RECOMMENDATIONS>`__ variables for
   related information.

PACKAGE_EXTRA_ARCHS
   Specifies the list of architectures compatible with the device CPU.
   This variable is useful when you build for several different devices
   that use miscellaneous processors such as XScale and ARM926-EJS.

PACKAGE_FEED_ARCHS
   Optionally specifies the package architectures used as part of the
   package feed URIs during the build. When used, the
   ``PACKAGE_FEED_ARCHS`` variable is appended to the final package feed
   URI, which is constructed using the
   ```PACKAGE_FEED_URIS`` <#var-PACKAGE_FEED_URIS>`__ and
   ```PACKAGE_FEED_BASE_PATHS`` <#var-PACKAGE_FEED_BASE_PATHS>`__
   variables.

      **Note**

      You can use the
      PACKAGE_FEEDS_ARCHS
      variable to whitelist specific package architectures. If you do
      not need to whitelist specific architectures, which is a common
      case, you can omit this variable. Omitting the variable results in
      all available architectures for the current machine being included
      into remote package feeds.

   Consider the following example where the ``PACKAGE_FEED_URIS``,
   ``PACKAGE_FEED_BASE_PATHS``, and ``PACKAGE_FEED_ARCHS`` variables are
   defined in your ``local.conf`` file: PACKAGE_FEED_URIS =
   "https://example.com/packagerepos/release \\
   https://example.com/packagerepos/updates" PACKAGE_FEED_BASE_PATHS =
   "rpm rpm-dev" PACKAGE_FEED_ARCHS = "all core2-64" Given these
   settings, the resulting package feeds are as follows:
   https://example.com/packagerepos/release/rpm/all
   https://example.com/packagerepos/release/rpm/core2-64
   https://example.com/packagerepos/release/rpm-dev/all
   https://example.com/packagerepos/release/rpm-dev/core2-64
   https://example.com/packagerepos/updates/rpm/all
   https://example.com/packagerepos/updates/rpm/core2-64
   https://example.com/packagerepos/updates/rpm-dev/all
   https://example.com/packagerepos/updates/rpm-dev/core2-64

PACKAGE_FEED_BASE_PATHS
   Specifies the base path used when constructing package feed URIs. The
   ``PACKAGE_FEED_BASE_PATHS`` variable makes up the middle portion of a
   package feed URI used by the OpenEmbedded build system. The base path
   lies between the ```PACKAGE_FEED_URIS`` <#var-PACKAGE_FEED_URIS>`__
   and ```PACKAGE_FEED_ARCHS`` <#var-PACKAGE_FEED_ARCHS>`__ variables.

   Consider the following example where the ``PACKAGE_FEED_URIS``,
   ``PACKAGE_FEED_BASE_PATHS``, and ``PACKAGE_FEED_ARCHS`` variables are
   defined in your ``local.conf`` file: PACKAGE_FEED_URIS =
   "https://example.com/packagerepos/release \\
   https://example.com/packagerepos/updates" PACKAGE_FEED_BASE_PATHS =
   "rpm rpm-dev" PACKAGE_FEED_ARCHS = "all core2-64" Given these
   settings, the resulting package feeds are as follows:
   https://example.com/packagerepos/release/rpm/all
   https://example.com/packagerepos/release/rpm/core2-64
   https://example.com/packagerepos/release/rpm-dev/all
   https://example.com/packagerepos/release/rpm-dev/core2-64
   https://example.com/packagerepos/updates/rpm/all
   https://example.com/packagerepos/updates/rpm/core2-64
   https://example.com/packagerepos/updates/rpm-dev/all
   https://example.com/packagerepos/updates/rpm-dev/core2-64

PACKAGE_FEED_URIS
   Specifies the front portion of the package feed URI used by the
   OpenEmbedded build system. Each final package feed URI is comprised
   of ``PACKAGE_FEED_URIS``,
   ```PACKAGE_FEED_BASE_PATHS`` <#var-PACKAGE_FEED_BASE_PATHS>`__, and
   ```PACKAGE_FEED_ARCHS`` <#var-PACKAGE_FEED_ARCHS>`__ variables.

   Consider the following example where the ``PACKAGE_FEED_URIS``,
   ``PACKAGE_FEED_BASE_PATHS``, and ``PACKAGE_FEED_ARCHS`` variables are
   defined in your ``local.conf`` file: PACKAGE_FEED_URIS =
   "https://example.com/packagerepos/release \\
   https://example.com/packagerepos/updates" PACKAGE_FEED_BASE_PATHS =
   "rpm rpm-dev" PACKAGE_FEED_ARCHS = "all core2-64" Given these
   settings, the resulting package feeds are as follows:
   https://example.com/packagerepos/release/rpm/all
   https://example.com/packagerepos/release/rpm/core2-64
   https://example.com/packagerepos/release/rpm-dev/all
   https://example.com/packagerepos/release/rpm-dev/core2-64
   https://example.com/packagerepos/updates/rpm/all
   https://example.com/packagerepos/updates/rpm/core2-64
   https://example.com/packagerepos/updates/rpm-dev/all
   https://example.com/packagerepos/updates/rpm-dev/core2-64

PACKAGE_GROUP
   The ``PACKAGE_GROUP`` variable has been renamed to
   ```FEATURE_PACKAGES`` <#var-FEATURE_PACKAGES>`__. See the variable
   description for ``FEATURE_PACKAGES`` for information.

   If if you use the ``PACKAGE_GROUP`` variable, the OpenEmbedded build
   system issues a warning message.

PACKAGE_INSTALL
   The final list of packages passed to the package manager for
   installation into the image.

   Because the package manager controls actual installation of all
   packages, the list of packages passed using ``PACKAGE_INSTALL`` is
   not the final list of packages that are actually installed. This
   variable is internal to the image construction code. Consequently, in
   general, you should use the
   ```IMAGE_INSTALL`` <#var-IMAGE_INSTALL>`__ variable to specify
   packages for installation. The exception to this is when working with
   the
   ```core-image-minimal-initramfs`` <#images-core-image-minimal-initramfs>`__
   image. When working with an initial RAM filesystem (initramfs) image,
   use the ``PACKAGE_INSTALL`` variable. For information on creating an
   initramfs, see the "`Building an Initial RAM Filesystem (initramfs)
   Image <&YOCTO_DOCS_DEV_URL;#building-an-initramfs-image>`__" section
   in the Yocto Project Development Tasks Manual.

PACKAGE_INSTALL_ATTEMPTONLY
   Specifies a list of packages the OpenEmbedded build system attempts
   to install when creating an image. If a listed package fails to
   install, the build system does not generate an error. This variable
   is generally not user-defined.

PACKAGE_PREPROCESS_FUNCS
   Specifies a list of functions run to pre-process the
   ```PKGD`` <#var-PKGD>`__ directory prior to splitting the files out
   to individual packages.

PACKAGE_WRITE_DEPS
   Specifies a list of dependencies for post-installation and
   pre-installation scripts on native/cross tools. If your
   post-installation or pre-installation script can execute at rootfs
   creation time rather than on the target but depends on a native tool
   in order to execute, you need to list the tools in
   ``PACKAGE_WRITE_DEPS``.

   For information on running post-installation scripts, see the
   "`Post-Installation
   Scripts <&YOCTO_DOCS_DEV_URL;#new-recipe-post-installation-scripts>`__"
   section in the Yocto Project Development Tasks Manual.

PACKAGECONFIG
   This variable provides a means of enabling or disabling features of a
   recipe on a per-recipe basis. ``PACKAGECONFIG`` blocks are defined in
   recipes when you specify features and then arguments that define
   feature behaviors. Here is the basic block structure (broken over
   multiple lines for readability): PACKAGECONFIG ??= "f1 f2 f3 ..."
   PACKAGECONFIG[f1] = "\\ --with-f1, \\ --without-f1, \\
   build-deps-for-f1, \\ runtime-deps-for-f1, \\
   runtime-recommends-for-f1, \\ packageconfig-conflicts-for-f1 \\ "
   PACKAGECONFIG[f2] = "\\ ... and so on and so on ...

   The ``PACKAGECONFIG`` variable itself specifies a space-separated
   list of the features to enable. Following the features, you can
   determine the behavior of each feature by providing up to six
   order-dependent arguments, which are separated by commas. You can
   omit any argument you like but must retain the separating commas. The
   order is important and specifies the following:

   1. Extra arguments that should be added to the configure script
      argument list (```EXTRA_OECONF`` <#var-EXTRA_OECONF>`__ or
      ```PACKAGECONFIG_CONFARGS`` <#var-PACKAGECONFIG_CONFARGS>`__) if
      the feature is enabled.

   2. Extra arguments that should be added to ``EXTRA_OECONF`` or
      ``PACKAGECONFIG_CONFARGS`` if the feature is disabled.

   3. Additional build dependencies (```DEPENDS`` <#var-DEPENDS>`__)
      that should be added if the feature is enabled.

   4. Additional runtime dependencies (```RDEPENDS`` <#var-RDEPENDS>`__)
      that should be added if the feature is enabled.

   5. Additional runtime recommendations
      (```RRECOMMENDS`` <#var-RRECOMMENDS>`__) that should be added if
      the feature is enabled.

   6. Any conflicting (that is, mutually exclusive) ``PACKAGECONFIG``
      settings for this feature.

   Consider the following ``PACKAGECONFIG`` block taken from the
   ``librsvg`` recipe. In this example the feature is ``gtk``, which has
   three arguments that determine the feature's behavior.
   PACKAGECONFIG[gtk] = "--with-gtk3,--without-gtk3,gtk+3" The
   ``--with-gtk3`` and ``gtk+3`` arguments apply only if the feature is
   enabled. In this case, ``--with-gtk3`` is added to the configure
   script argument list and ``gtk+3`` is added to ``DEPENDS``. On the
   other hand, if the feature is disabled say through a ``.bbappend``
   file in another layer, then the second argument ``--without-gtk3`` is
   added to the configure script instead.

   The basic ``PACKAGECONFIG`` structure previously described holds true
   regardless of whether you are creating a block or changing a block.
   When creating a block, use the structure inside your recipe.

   If you want to change an existing ``PACKAGECONFIG`` block, you can do
   so one of two ways:

   -  *Append file:* Create an append file named
      recipename\ ``.bbappend`` in your layer and override the value of
      ``PACKAGECONFIG``. You can either completely override the
      variable: PACKAGECONFIG = "f4 f5" Or, you can just append the
      variable: PACKAGECONFIG_append = " f4"

   -  *Configuration file:* This method is identical to changing the
      block through an append file except you edit your ``local.conf``
      or ``mydistro.conf`` file. As with append files previously
      described, you can either completely override the variable:
      PACKAGECONFIG_pn-recipename = "f4 f5" Or, you can just amend the
      variable: PACKAGECONFIG_append_pn-recipename = " f4"

PACKAGECONFIG_CONFARGS
   A space-separated list of configuration options generated from the
   ```PACKAGECONFIG`` <#var-PACKAGECONFIG>`__ setting.

   Classes such as ```autotools`` <#ref-classes-autotools>`__ and
   ```cmake`` <#ref-classes-cmake>`__ use ``PACKAGECONFIG_CONFARGS`` to
   pass ``PACKAGECONFIG`` options to ``configure`` and ``cmake``,
   respectively. If you are using ``PACKAGECONFIG`` but not a class that
   handles the ``do_configure`` task, then you need to use
   ``PACKAGECONFIG_CONFARGS`` appropriately.

PACKAGEGROUP_DISABLE_COMPLEMENTARY
   For recipes inheriting the
   ```packagegroup`` <#ref-classes-packagegroup>`__ class, setting
   ``PACKAGEGROUP_DISABLE_COMPLEMENTARY`` to "1" specifies that the
   normal complementary packages (i.e. ``-dev``, ``-dbg``, and so forth)
   should not be automatically created by the ``packagegroup`` recipe,
   which is the default behavior.

PACKAGES
   The list of packages the recipe creates. The default value is the
   following: ${PN}-dbg ${PN}-staticdev ${PN}-dev ${PN}-doc ${PN}-locale
   ${PACKAGE_BEFORE_PN} ${PN}

   During packaging, the ```do_package`` <#ref-tasks-package>`__ task
   goes through ``PACKAGES`` and uses the ```FILES`` <#var-FILES>`__
   variable corresponding to each package to assign files to the
   package. If a file matches the ``FILES`` variable for more than one
   package in ``PACKAGES``, it will be assigned to the earliest
   (leftmost) package.

   Packages in the variable's list that are empty (i.e. where none of
   the patterns in ``FILES_``\ pkg match any files installed by the
   ```do_install`` <#ref-tasks-install>`__ task) are not generated,
   unless generation is forced through the
   ```ALLOW_EMPTY`` <#var-ALLOW_EMPTY>`__ variable.

PACKAGES_DYNAMIC
   A promise that your recipe satisfies runtime dependencies for
   optional modules that are found in other recipes.
   ``PACKAGES_DYNAMIC`` does not actually satisfy the dependencies, it
   only states that they should be satisfied. For example, if a hard,
   runtime dependency (```RDEPENDS`` <#var-RDEPENDS>`__) of another
   package is satisfied at build time through the ``PACKAGES_DYNAMIC``
   variable, but a package with the module name is never actually
   produced, then the other package will be broken. Thus, if you attempt
   to include that package in an image, you will get a dependency
   failure from the packaging system during the
   ```do_rootfs`` <#ref-tasks-rootfs>`__ task.

   Typically, if there is a chance that such a situation can occur and
   the package that is not created is valid without the dependency being
   satisfied, then you should use ```RRECOMMENDS`` <#var-RRECOMMENDS>`__
   (a soft runtime dependency) instead of ``RDEPENDS``.

   For an example of how to use the ``PACKAGES_DYNAMIC`` variable when
   you are splitting packages, see the "`Handling Optional Module
   Packaging <&YOCTO_DOCS_DEV_URL;#handling-optional-module-packaging>`__"
   section in the Yocto Project Development Tasks Manual.

PACKAGESPLITFUNCS
   Specifies a list of functions run to perform additional splitting of
   files into individual packages. Recipes can either prepend to this
   variable or prepend to the ``populate_packages`` function in order to
   perform additional package splitting. In either case, the function
   should set ```PACKAGES`` <#var-PACKAGES>`__,
   ```FILES`` <#var-FILES>`__, ```RDEPENDS`` <#var-RDEPENDS>`__ and
   other packaging variables appropriately in order to perform the
   desired splitting.

PARALLEL_MAKE
   Extra options passed to the ``make`` command during the
   ```do_compile`` <#ref-tasks-compile>`__ task in order to specify
   parallel compilation on the local build host. This variable is
   usually in the form "-j x", where x represents the maximum number of
   parallel threads ``make`` can run.

      **Note**

      In order for
      PARALLEL_MAKE
      to be effective,
      make
      must be called with
      ${
      EXTRA_OEMAKE
      }
      . An easy way to ensure this is to use the
      oe_runmake
      function.

   By default, the OpenEmbedded build system automatically sets this
   variable to be equal to the number of cores the build system uses.

      **Note**

      If the software being built experiences dependency issues during
      the
      do_compile
      task that result in race conditions, you can clear the
      PARALLEL_MAKE
      variable within the recipe as a workaround. For information on
      addressing race conditions, see the "
      Debugging Parallel Make Races
      " section in the Yocto Project Development Tasks Manual.

   For single socket systems (i.e. one CPU), you should not have to
   override this variable to gain optimal parallelism during builds.
   However, if you have very large systems that employ multiple physical
   CPUs, you might want to make sure the ``PARALLEL_MAKE`` variable is
   not set higher than "-j 20".

   For more information on speeding up builds, see the "`Speeding Up a
   Build <&YOCTO_DOCS_DEV_URL;#speeding-up-a-build>`__" section in the
   Yocto Project Development Tasks Manual.

PARALLEL_MAKEINST
   Extra options passed to the ``make install`` command during the
   ```do_install`` <#ref-tasks-install>`__ task in order to specify
   parallel installation. This variable defaults to the value of
   ```PARALLEL_MAKE`` <#var-PARALLEL_MAKE>`__.

      **Note**

      In order for ``PARALLEL_MAKEINST`` to be effective, ``make`` must
      be called with
      ``${``\ ```EXTRA_OEMAKE`` <#var-EXTRA_OEMAKE>`__\ ``}``. An easy
      way to ensure this is to use the ``oe_runmake`` function.

      If the software being built experiences dependency issues during
      the ``do_install`` task that result in race conditions, you can
      clear the ``PARALLEL_MAKEINST`` variable within the recipe as a
      workaround. For information on addressing race conditions, see the
      "`Debugging Parallel Make
      Races <&YOCTO_DOCS_DEV_URL;#debugging-parallel-make-races>`__"
      section in the Yocto Project Development Tasks Manual.

PATCHRESOLVE
   Determines the action to take when a patch fails. You can set this
   variable to one of two values: "noop" and "user".

   The default value of "noop" causes the build to simply fail when the
   OpenEmbedded build system cannot successfully apply a patch. Setting
   the value to "user" causes the build system to launch a shell and
   places you in the right location so that you can manually resolve the
   conflicts.

   Set this variable in your ``local.conf`` file.

PATCHTOOL
   Specifies the utility used to apply patches for a recipe during the
   ```do_patch`` <#ref-tasks-patch>`__ task. You can specify one of
   three utilities: "patch", "quilt", or "git". The default utility used
   is "quilt" except for the quilt-native recipe itself. Because the
   quilt tool is not available at the time quilt-native is being
   patched, it uses "patch".

   If you wish to use an alternative patching tool, set the variable in
   the recipe using one of the following: PATCHTOOL = "patch" PATCHTOOL
   = "quilt" PATCHTOOL = "git"

PE
   The epoch of the recipe. By default, this variable is unset. The
   variable is used to make upgrades possible when the versioning scheme
   changes in some backwards incompatible way.

   ``PE`` is the default value of the ```PKGE`` <#var-PKGE>`__ variable.

PF
   Specifies the recipe or package name and includes all version and
   revision numbers (i.e. ``glibc-2.13-r20+svnr15508/`` and
   ``bash-4.2-r1/``). This variable is comprised of the following:
   ${`PN <#var-PN>`__}-${`EXTENDPE <#var-EXTENDPE>`__}${`PV <#var-PV>`__}-${`PR <#var-PR>`__}

PIXBUF_PACKAGES
   When inheriting the ```pixbufcache`` <#ref-classes-pixbufcache>`__
   class, this variable identifies packages that contain the pixbuf
   loaders used with ``gdk-pixbuf``. By default, the ``pixbufcache``
   class assumes that the loaders are in the recipe's main package (i.e.
   ``${``\ ```PN`` <#var-PN>`__\ ``}``). Use this variable if the
   loaders you need are in a package other than that main package.

PKG
   The name of the resulting package created by the OpenEmbedded build
   system.

      **Note**

      When using the
      PKG
      variable, you must use a package name override.

   For example, when the ```debian`` <#ref-classes-debian>`__ class
   renames the output package, it does so by setting
   ``PKG_packagename``.

PKG_CONFIG_PATH
   The path to ``pkg-config`` files for the current build context.
   ``pkg-config`` reads this variable from the environment.

PKGD
   Points to the destination directory for files to be packaged before
   they are split into individual packages. This directory defaults to
   the following: ${WORKDIR}/package

   Do not change this default.

PKGDATA_DIR
   Points to a shared, global-state directory that holds data generated
   during the packaging process. During the packaging process, the
   ```do_packagedata`` <#ref-tasks-packagedata>`__ task packages data
   for each recipe and installs it into this temporary, shared area.
   This directory defaults to the following, which you should not
   change: ${STAGING_DIR_HOST}/pkgdata For examples of how this data is
   used, see the "`Automatically Added Runtime
   Dependencies <&YOCTO_DOCS_OM_URL;#automatically-added-runtime-dependencies>`__"
   section in the Yocto Project Overview and Concepts Manual and the
   "`Viewing Package Information with
   ``oe-pkgdata-util`` <&YOCTO_DOCS_DEV_URL;#viewing-package-information-with-oe-pkgdata-util>`__"
   section in the Yocto Project Development Tasks Manual. For more
   information on the shared, global-state directory, see
   ```STAGING_DIR_HOST`` <#var-STAGING_DIR_HOST>`__.

PKGDEST
   Points to the parent directory for files to be packaged after they
   have been split into individual packages. This directory defaults to
   the following: ${WORKDIR}/packages-split

   Under this directory, the build system creates directories for each
   package specified in ```PACKAGES`` <#var-PACKAGES>`__. Do not change
   this default.

PKGDESTWORK
   Points to a temporary work area where the
   ```do_package`` <#ref-tasks-package>`__ task saves package metadata.
   The ``PKGDESTWORK`` location defaults to the following:
   ${WORKDIR}/pkgdata Do not change this default.

   The ```do_packagedata`` <#ref-tasks-packagedata>`__ task copies the
   package metadata from ``PKGDESTWORK`` to
   ```PKGDATA_DIR`` <#var-PKGDATA_DIR>`__ to make it available globally.

PKGE
   The epoch of the package(s) built by the recipe. By default, ``PKGE``
   is set to ```PE`` <#var-PE>`__.

PKGR
   The revision of the package(s) built by the recipe. By default,
   ``PKGR`` is set to ```PR`` <#var-PR>`__.

PKGV
   The version of the package(s) built by the recipe. By default,
   ``PKGV`` is set to ```PV`` <#var-PV>`__.

PN
   This variable can have two separate functions depending on the
   context: a recipe name or a resulting package name.

   ``PN`` refers to a recipe name in the context of a file used by the
   OpenEmbedded build system as input to create a package. The name is
   normally extracted from the recipe file name. For example, if the
   recipe is named ``expat_2.0.1.bb``, then the default value of ``PN``
   will be "expat".

   The variable refers to a package name in the context of a file
   created or produced by the OpenEmbedded build system.

   If applicable, the ``PN`` variable also contains any special suffix
   or prefix. For example, using ``bash`` to build packages for the
   native machine, ``PN`` is ``bash-native``. Using ``bash`` to build
   packages for the target and for Multilib, ``PN`` would be ``bash``
   and ``lib64-bash``, respectively.

PNBLACKLIST
   Lists recipes you do not want the OpenEmbedded build system to build.
   This variable works in conjunction with the
   ```blacklist`` <#ref-classes-blacklist>`__ class, which is inherited
   globally.

   To prevent a recipe from being built, use the ``PNBLACKLIST``
   variable in your ``local.conf`` file. Here is an example that
   prevents ``myrecipe`` from being built: PNBLACKLIST[myrecipe] = "Not
   supported by our organization."

POPULATE_SDK_POST_HOST_COMMAND
   Specifies a list of functions to call once the OpenEmbedded build
   system has created the host part of the SDK. You can specify
   functions separated by semicolons: POPULATE_SDK_POST_HOST_COMMAND +=
   "function; ... "

   If you need to pass the SDK path to a command within a function, you
   can use ``${SDK_DIR}``, which points to the parent directory used by
   the OpenEmbedded build system when creating SDK output. See the
   ```SDK_DIR`` <#var-SDK_DIR>`__ variable for more information.

POPULATE_SDK_POST_TARGET_COMMAND
   Specifies a list of functions to call once the OpenEmbedded build
   system has created the target part of the SDK. You can specify
   functions separated by semicolons: POPULATE_SDK_POST_TARGET_COMMAND
   += "function; ... "

   If you need to pass the SDK path to a command within a function, you
   can use ``${SDK_DIR}``, which points to the parent directory used by
   the OpenEmbedded build system when creating SDK output. See the
   ```SDK_DIR`` <#var-SDK_DIR>`__ variable for more information.

PR
   The revision of the recipe. The default value for this variable is
   "r0". Subsequent revisions of the recipe conventionally have the
   values "r1", "r2", and so forth. When ```PV`` <#var-PV>`__ increases,
   ``PR`` is conventionally reset to "r0".

      **Note**

      The OpenEmbedded build system does not need the aid of
      PR
      to know when to rebuild a recipe. The build system uses the task
      input checksums
      along with the
      stamp
      and
      shared state cache
      mechanisms.

   The ``PR`` variable primarily becomes significant when a package
   manager dynamically installs packages on an already built image. In
   this case, ``PR``, which is the default value of
   ```PKGR`` <#var-PKGR>`__, helps the package manager distinguish which
   package is the most recent one in cases where many packages have the
   same ``PV`` (i.e. ``PKGV``). A component having many packages with
   the same ``PV`` usually means that the packages all install the same
   upstream version, but with later (``PR``) version packages including
   packaging fixes.

      **Note**

      PR
      does not need to be increased for changes that do not change the
      package contents or metadata.

   Because manually managing ``PR`` can be cumbersome and error-prone,
   an automated solution exists. See the "`Working With a PR
   Service <&YOCTO_DOCS_DEV_URL;#working-with-a-pr-service>`__" section
   in the Yocto Project Development Tasks Manual for more information.

PREFERRED_PROVIDER
   If multiple recipes provide the same item, this variable determines
   which recipe is preferred and thus provides the item (i.e. the
   preferred provider). You should always suffix this variable with the
   name of the provided item. And, you should define the variable using
   the preferred recipe's name (```PN`` <#var-PN>`__). Here is a common
   example: PREFERRED_PROVIDER_virtual/kernel ?= "linux-yocto" In the
   previous example, multiple recipes are providing "virtual/kernel".
   The ``PREFERRED_PROVIDER`` variable is set with the name (``PN``) of
   the recipe you prefer to provide "virtual/kernel".

   Following are more examples: PREFERRED_PROVIDER_virtual/xserver =
   "xserver-xf86" PREFERRED_PROVIDER_virtual/libgl ?= "mesa" For more
   information, see the "`Using Virtual
   Providers <&YOCTO_DOCS_DEV_URL;#metadata-virtual-providers>`__"
   section in the Yocto Project Development Tasks Manual.

      **Note**

      If you use a
      virtual/\*
      item with
      PREFERRED_PROVIDER
      , then any recipe that
      PROVIDES
      that item but is not selected (defined) by
      PREFERRED_PROVIDER
      is prevented from building, which is usually desirable since this
      mechanism is designed to select between mutually exclusive
      alternative providers.

PREFERRED_VERSION
   If multiple versions of recipes exist, this variable determines which
   version is given preference. You must always suffix the variable with
   the ```PN`` <#var-PN>`__ you want to select, and you should set the
   ```PV`` <#var-PV>`__ accordingly for precedence.

   The ``PREFERRED_VERSION`` variable supports limited wildcard use
   through the "``%``" character. You can use the character to match any
   number of characters, which can be useful when specifying versions
   that contain long revision numbers that potentially change. Here are
   two examples: PREFERRED_VERSION_python = "3.4.0"
   PREFERRED_VERSION_linux-yocto = "5.0%"

      **Note**

      The use of the "
      %
      " character is limited in that it only works at the end of the
      string. You cannot use the wildcard character in any other
      location of the string.

   The specified version is matched against ```PV`` <#var-PV>`__, which
   does not necessarily match the version part of the recipe's filename.
   For example, consider two recipes ``foo_1.2.bb`` and ``foo_git.bb``
   where ``foo_git.bb`` contains the following assignment: PV =
   "1.1+git${SRCPV}" In this case, the correct way to select
   ``foo_git.bb`` is by using an assignment such as the following:
   PREFERRED_VERSION_foo = "1.1+git%" Compare that previous example
   against the following incorrect example, which does not work:
   PREFERRED_VERSION_foo = "git"

   Sometimes the ``PREFERRED_VERSION`` variable can be set by
   configuration files in a way that is hard to change. You can use
   ```OVERRIDES`` <#var-OVERRIDES>`__ to set a machine-specific
   override. Here is an example: PREFERRED_VERSION_linux-yocto_qemux86 =
   "5.0%" Although not recommended, worst case, you can also use the
   "forcevariable" override, which is the strongest override possible.
   Here is an example: PREFERRED_VERSION_linux-yocto_forcevariable =
   "5.0%"

      **Note**

      The
      \_forcevariable
      override is not handled specially. This override only works
      because the default value of
      OVERRIDES
      includes "forcevariable".

PREMIRRORS
   Specifies additional paths from which the OpenEmbedded build system
   gets source code. When the build system searches for source code, it
   first tries the local download directory. If that location fails, the
   build system tries locations defined by ``PREMIRRORS``, the upstream
   source, and then locations specified by
   ```MIRRORS`` <#var-MIRRORS>`__ in that order.

   Assuming your distribution (```DISTRO`` <#var-DISTRO>`__) is "poky",
   the default value for ``PREMIRRORS`` is defined in the
   ``conf/distro/poky.conf`` file in the ``meta-poky`` Git repository.

   Typically, you could add a specific server for the build system to
   attempt before any others by adding something like the following to
   the ``local.conf`` configuration file in the `Build
   Directory <#build-directory>`__: PREMIRRORS_prepend = "\\
   git://.*/.\* http://www.yoctoproject.org/sources/ \\n \\ ftp://.*/.\*
   http://www.yoctoproject.org/sources/ \\n \\ http://.*/.\*
   http://www.yoctoproject.org/sources/ \\n \\ https://.*/.\*
   http://www.yoctoproject.org/sources/ \\n" These changes cause the
   build system to intercept Git, FTP, HTTP, and HTTPS requests and
   direct them to the ``http://`` sources mirror. You can use
   ``file://`` URLs to point to local directories or network shares as
   well.

PRIORITY
   Indicates the importance of a package.

   ``PRIORITY`` is considered to be part of the distribution policy
   because the importance of any given recipe depends on the purpose for
   which the distribution is being produced. Thus, ``PRIORITY`` is not
   normally set within recipes.

   You can set ``PRIORITY`` to "required", "standard", "extra", and
   "optional", which is the default.

PRIVATE_LIBS
   Specifies libraries installed within a recipe that should be ignored
   by the OpenEmbedded build system's shared library resolver. This
   variable is typically used when software being built by a recipe has
   its own private versions of a library normally provided by another
   recipe. In this case, you would not want the package containing the
   private libraries to be set as a dependency on other unrelated
   packages that should instead depend on the package providing the
   standard version of the library.

   Libraries specified in this variable should be specified by their
   file name. For example, from the Firefox recipe in meta-browser:
   PRIVATE_LIBS = "libmozjs.so \\ libxpcom.so \\ libnspr4.so \\
   libxul.so \\ libmozalloc.so \\ libplc4.so \\ libplds4.so"

   For more information, see the "`Automatically Added Runtime
   Dependencies <&YOCTO_DOCS_OM_URL;#automatically-added-runtime-dependencies>`__"
   section in the Yocto Project Overview and Concepts Manual.

PROVIDES
   A list of aliases by which a particular recipe can be known. By
   default, a recipe's own ``PN`` is implicitly already in its
   ``PROVIDES`` list and therefore does not need to mention that it
   provides itself. If a recipe uses ``PROVIDES``, the additional
   aliases are synonyms for the recipe and can be useful for satisfying
   dependencies of other recipes during the build as specified by
   ``DEPENDS``.

   Consider the following example ``PROVIDES`` statement from the recipe
   file ``eudev_3.2.9.bb``: PROVIDES = "udev" The ``PROVIDES`` statement
   results in the "eudev" recipe also being available as simply "udev".

      **Note**

      Given that a recipe's own recipe name is already implicitly in its
      own
      PROVIDES
      list, it is unnecessary to add aliases with the "+=" operator;
      using a simple assignment will be sufficient. In other words,
      while you could write:
      ::

              PROVIDES += "udev"
                                 

      in the above, the "+=" is overkill and unnecessary.

   In addition to providing recipes under alternate names, the
   ``PROVIDES`` mechanism is also used to implement virtual targets. A
   virtual target is a name that corresponds to some particular
   functionality (e.g. a Linux kernel). Recipes that provide the
   functionality in question list the virtual target in ``PROVIDES``.
   Recipes that depend on the functionality in question can include the
   virtual target in ``DEPENDS`` to leave the choice of provider open.

   Conventionally, virtual targets have names on the form
   "virtual/function" (e.g. "virtual/kernel"). The slash is simply part
   of the name and has no syntactical significance.

   The ```PREFERRED_PROVIDER`` <#var-PREFERRED_PROVIDER>`__ variable is
   used to select which particular recipe provides a virtual target.

      **Note**

      A corresponding mechanism for virtual runtime dependencies
      (packages) exists. However, the mechanism does not depend on any
      special functionality beyond ordinary variable assignments. For
      example, ``VIRTUAL-RUNTIME_dev_manager`` refers to the package of
      the component that manages the ``/dev`` directory.

      Setting the "preferred provider" for runtime dependencies is as
      simple as using the following assignment in a configuration file:

      ::

              VIRTUAL-RUNTIME_dev_manager = "udev"
                                 

PRSERV_HOST
   The network based ```PR`` <#var-PR>`__ service host and port.

   The ``conf/local.conf.sample.extended`` configuration file in the
   `Source Directory <#source-directory>`__ shows how the
   ``PRSERV_HOST`` variable is set: PRSERV_HOST = "localhost:0" You must
   set the variable if you want to automatically start a local `PR
   service <&YOCTO_DOCS_DEV_URL;#working-with-a-pr-service>`__. You can
   set ``PRSERV_HOST`` to other values to use a remote PR service.

PTEST_ENABLED
   Specifies whether or not `Package
   Test <&YOCTO_DOCS_DEV_URL;#testing-packages-with-ptest>`__ (ptest)
   functionality is enabled when building a recipe. You should not set
   this variable directly. Enabling and disabling building Package Tests
   at build time should be done by adding "ptest" to (or removing it
   from) ```DISTRO_FEATURES`` <#var-DISTRO_FEATURES>`__.

PV
   The version of the recipe. The version is normally extracted from the
   recipe filename. For example, if the recipe is named
   ``expat_2.0.1.bb``, then the default value of ``PV`` will be "2.0.1".
   ``PV`` is generally not overridden within a recipe unless it is
   building an unstable (i.e. development) version from a source code
   repository (e.g. Git or Subversion).

   ``PV`` is the default value of the ```PKGV`` <#var-PKGV>`__ variable.

PYTHON_ABI
   When used by recipes that inherit the
   ```distutils3`` <#ref-classes-distutils3>`__,
   ```setuptools3`` <#ref-classes-setuptools3>`__,
   ```distutils`` <#ref-classes-distutils>`__, or
   ```setuptools`` <#ref-classes-setuptools>`__ classes, denotes the
   Application Binary Interface (ABI) currently in use for Python. By
   default, the ABI is "m". You do not have to set this variable as the
   OpenEmbedded build system sets it for you.

   The OpenEmbedded build system uses the ABI to construct directory
   names used when installing the Python headers and libraries in
   sysroot (e.g. ``.../python3.3m/...``).

   Recipes that inherit the ``distutils`` class during cross-builds also
   use this variable to locate the headers and libraries of the
   appropriate Python that the extension is targeting.

PYTHON_PN
   When used by recipes that inherit the
   ```distutils3`` <#ref-classes-distutils3>`__,
   ```setuptools3`` <#ref-classes-setuptools3>`__,
   ```distutils`` <#ref-classes-distutils>`__, or
   ```setuptools`` <#ref-classes-setuptools>`__ classes, specifies the
   major Python version being built. For Python 3.x, ``PYTHON_PN`` would
   be "python3". You do not have to set this variable as the
   OpenEmbedded build system automatically sets it for you.

   The variable allows recipes to use common infrastructure such as the
   following: DEPENDS += "${PYTHON_PN}-native" In the previous example,
   the version of the dependency is ``PYTHON_PN``.

RANLIB
   The minimal command and arguments to run ``ranlib``.

RCONFLICTS
   The list of packages that conflict with packages. Note that packages
   will not be installed if conflicting packages are not first removed.

   Like all package-controlling variables, you must always use them in
   conjunction with a package name override. Here is an example:
   RCONFLICTS_${PN} = "another_conflicting_package_name"

   BitBake, which the OpenEmbedded build system uses, supports
   specifying versioned dependencies. Although the syntax varies
   depending on the packaging format, BitBake hides these differences
   from you. Here is the general syntax to specify versions with the
   ``RCONFLICTS`` variable: RCONFLICTS_${PN} = "package (operator
   version)" For ``operator``, you can specify the following: = < > <=
   >= For example, the following sets up a dependency on version 1.2 or
   greater of the package ``foo``: RCONFLICTS_${PN} = "foo (>= 1.2)"

RDEPENDS
   Lists runtime dependencies of a package. These dependencies are other
   packages that must be installed in order for the package to function
   correctly. As an example, the following assignment declares that the
   package ``foo`` needs the packages ``bar`` and ``baz`` to be
   installed: RDEPENDS_foo = "bar baz" The most common types of package
   runtime dependencies are automatically detected and added. Therefore,
   most recipes do not need to set ``RDEPENDS``. For more information,
   see the "`Automatically Added Runtime
   Dependencies <&YOCTO_DOCS_OM_URL;#automatically-added-runtime-dependencies>`__"
   section in the Yocto Project Overview and Concepts Manual.

   The practical effect of the above ``RDEPENDS`` assignment is that
   ``bar`` and ``baz`` will be declared as dependencies inside the
   package ``foo`` when it is written out by one of the
   ```do_package_write_*`` <#ref-tasks-package_write_deb>`__ tasks.
   Exactly how this is done depends on which package format is used,
   which is determined by
   ```PACKAGE_CLASSES`` <#var-PACKAGE_CLASSES>`__. When the
   corresponding package manager installs the package, it will know to
   also install the packages on which it depends.

   To ensure that the packages ``bar`` and ``baz`` get built, the
   previous ``RDEPENDS`` assignment also causes a task dependency to be
   added. This dependency is from the recipe's
   ```do_build`` <#ref-tasks-build>`__ (not to be confused with
   ```do_compile`` <#ref-tasks-compile>`__) task to the
   ``do_package_write_*`` task of the recipes that build ``bar`` and
   ``baz``.

   The names of the packages you list within ``RDEPENDS`` must be the
   names of other packages - they cannot be recipe names. Although
   package names and recipe names usually match, the important point
   here is that you are providing package names within the ``RDEPENDS``
   variable. For an example of the default list of packages created from
   a recipe, see the ```PACKAGES`` <#var-PACKAGES>`__ variable.

   Because the ``RDEPENDS`` variable applies to packages being built,
   you should always use the variable in a form with an attached package
   name (remember that a single recipe can build multiple packages). For
   example, suppose you are building a development package that depends
   on the ``perl`` package. In this case, you would use the following
   ``RDEPENDS`` statement: RDEPENDS_${PN}-dev += "perl" In the example,
   the development package depends on the ``perl`` package. Thus, the
   ``RDEPENDS`` variable has the ``${PN}-dev`` package name as part of
   the variable.

      **Note**

      RDEPENDS_${PN}-dev
      includes
      ${
      PN
      }
      by default. This default is set in the BitBake configuration file
      (
      meta/conf/bitbake.conf
      ). Be careful not to accidentally remove
      ${PN}
      when modifying
      RDEPENDS_${PN}-dev
      . Use the "+=" operator rather than the "=" operator.

   The package names you use with ``RDEPENDS`` must appear as they would
   in the ``PACKAGES`` variable. The ```PKG`` <#var-PKG>`__ variable
   allows a different name to be used for the final package (e.g. the
   ```debian`` <#ref-classes-debian>`__ class uses this to rename
   packages), but this final package name cannot be used with
   ``RDEPENDS``, which makes sense as ``RDEPENDS`` is meant to be
   independent of the package format used.

   BitBake, which the OpenEmbedded build system uses, supports
   specifying versioned dependencies. Although the syntax varies
   depending on the packaging format, BitBake hides these differences
   from you. Here is the general syntax to specify versions with the
   ``RDEPENDS`` variable: RDEPENDS_${PN} = "package (operator version)"
   For operator, you can specify the following: = < > <= >= For version,
   provide the version number.

      **Note**

      You can use
      EXTENDPKGV
      to provide a full package version specification.

   For example, the following sets up a dependency on version 1.2 or
   greater of the package ``foo``: RDEPENDS_${PN} = "foo (>= 1.2)"

   For information on build-time dependencies, see the
   ```DEPENDS`` <#var-DEPENDS>`__ variable. You can also see the
   "`Tasks <&YOCTO_DOCS_BB_URL;#tasks>`__" and
   "`Dependencies <&YOCTO_DOCS_BB_URL;#dependencies>`__" sections in the
   BitBake User Manual for additional information on tasks and
   dependencies.

REQUIRED_DISTRO_FEATURES
   When inheriting the
   ```distro_features_check`` <#ref-classes-distro_features_check>`__
   class, this variable identifies distribution features that must exist
   in the current configuration in order for the OpenEmbedded build
   system to build the recipe. In other words, if the
   ``REQUIRED_DISTRO_FEATURES`` variable lists a feature that does not
   appear in ``DISTRO_FEATURES`` within the current configuration, an
   error occurs and the build stops.

RM_WORK_EXCLUDE
   With ``rm_work`` enabled, this variable specifies a list of recipes
   whose work directories should not be removed. See the
   "```rm_work.bbclass`` <#ref-classes-rm-work>`__" section for more
   details.

ROOT_HOME
   Defines the root home directory. By default, this directory is set as
   follows in the BitBake configuration file: ROOT_HOME ??= "/home/root"

      **Note**

      This default value is likely used because some embedded solutions
      prefer to have a read-only root filesystem and prefer to keep
      writeable data in one place.

   You can override the default by setting the variable in any layer or
   in the ``local.conf`` file. Because the default is set using a "weak"
   assignment (i.e. "??="), you can use either of the following forms to
   define your override: ROOT_HOME = "/root" ROOT_HOME ?= "/root" These
   override examples use ``/root``, which is probably the most commonly
   used override.

ROOTFS
   Indicates a filesystem image to include as the root filesystem.

   The ``ROOTFS`` variable is an optional variable used with the
   ```image-live`` <#ref-classes-image-live>`__ class.

ROOTFS_POSTINSTALL_COMMAND
   Specifies a list of functions to call after the OpenEmbedded build
   system has installed packages. You can specify functions separated by
   semicolons: ROOTFS_POSTINSTALL_COMMAND += "function; ... "

   If you need to pass the root filesystem path to a command within a
   function, you can use ``${IMAGE_ROOTFS}``, which points to the
   directory that becomes the root filesystem image. See the
   ```IMAGE_ROOTFS`` <#var-IMAGE_ROOTFS>`__ variable for more
   information.

ROOTFS_POSTPROCESS_COMMAND
   Specifies a list of functions to call once the OpenEmbedded build
   system has created the root filesystem. You can specify functions
   separated by semicolons: ROOTFS_POSTPROCESS_COMMAND += "function; ...
   "

   If you need to pass the root filesystem path to a command within a
   function, you can use ``${IMAGE_ROOTFS}``, which points to the
   directory that becomes the root filesystem image. See the
   ```IMAGE_ROOTFS`` <#var-IMAGE_ROOTFS>`__ variable for more
   information.

ROOTFS_POSTUNINSTALL_COMMAND
   Specifies a list of functions to call after the OpenEmbedded build
   system has removed unnecessary packages. When runtime package
   management is disabled in the image, several packages are removed
   including ``base-passwd``, ``shadow``, and ``update-alternatives``.
   You can specify functions separated by semicolons:
   ROOTFS_POSTUNINSTALL_COMMAND += "function; ... "

   If you need to pass the root filesystem path to a command within a
   function, you can use ``${IMAGE_ROOTFS}``, which points to the
   directory that becomes the root filesystem image. See the
   ```IMAGE_ROOTFS`` <#var-IMAGE_ROOTFS>`__ variable for more
   information.

ROOTFS_PREPROCESS_COMMAND
   Specifies a list of functions to call before the OpenEmbedded build
   system has created the root filesystem. You can specify functions
   separated by semicolons: ROOTFS_PREPROCESS_COMMAND += "function; ...
   "

   If you need to pass the root filesystem path to a command within a
   function, you can use ``${IMAGE_ROOTFS}``, which points to the
   directory that becomes the root filesystem image. See the
   ```IMAGE_ROOTFS`` <#var-IMAGE_ROOTFS>`__ variable for more
   information.

RPROVIDES
   A list of package name aliases that a package also provides. These
   aliases are useful for satisfying runtime dependencies of other
   packages both during the build and on the target (as specified by
   ``RDEPENDS``).

      **Note**

      A package's own name is implicitly already in its
      RPROVIDES
      list.

   As with all package-controlling variables, you must always use the
   variable in conjunction with a package name override. Here is an
   example: RPROVIDES_${PN} = "widget-abi-2"

RRECOMMENDS
   A list of packages that extends the usability of a package being
   built. The package being built does not depend on this list of
   packages in order to successfully build, but rather uses them for
   extended usability. To specify runtime dependencies for packages, see
   the ``RDEPENDS`` variable.

   The package manager will automatically install the ``RRECOMMENDS``
   list of packages when installing the built package. However, you can
   prevent listed packages from being installed by using the
   ```BAD_RECOMMENDATIONS`` <#var-BAD_RECOMMENDATIONS>`__,
   ```NO_RECOMMENDATIONS`` <#var-NO_RECOMMENDATIONS>`__, and
   ```PACKAGE_EXCLUDE`` <#var-PACKAGE_EXCLUDE>`__ variables.

   Packages specified in ``RRECOMMENDS`` need not actually be produced.
   However, a recipe must exist that provides each package, either
   through the ```PACKAGES`` <#var-PACKAGES>`__ or
   ```PACKAGES_DYNAMIC`` <#var-PACKAGES_DYNAMIC>`__ variables or the
   ```RPROVIDES`` <#var-RPROVIDES>`__ variable, or an error will occur
   during the build. If such a recipe does exist and the package is not
   produced, the build continues without error.

   Because the ``RRECOMMENDS`` variable applies to packages being built,
   you should always attach an override to the variable to specify the
   particular package whose usability is being extended. For example,
   suppose you are building a development package that is extended to
   support wireless functionality. In this case, you would use the
   following: RRECOMMENDS_${PN}-dev += "wireless_package_name" In the
   example, the package name (``${PN}-dev``) must appear as it would in
   the ``PACKAGES`` namespace before any renaming of the output package
   by classes such as ``debian.bbclass``.

   BitBake, which the OpenEmbedded build system uses, supports
   specifying versioned recommends. Although the syntax varies depending
   on the packaging format, BitBake hides these differences from you.
   Here is the general syntax to specify versions with the
   ``RRECOMMENDS`` variable: RRECOMMENDS_${PN} = "package (operator
   version)" For ``operator``, you can specify the following: = < > <=
   >= For example, the following sets up a recommend on version 1.2 or
   greater of the package ``foo``: RRECOMMENDS_${PN} = "foo (>= 1.2)"

RREPLACES
   A list of packages replaced by a package. The package manager uses
   this variable to determine which package should be installed to
   replace other package(s) during an upgrade. In order to also have the
   other package(s) removed at the same time, you must add the name of
   the other package to the ``RCONFLICTS`` variable.

   As with all package-controlling variables, you must use this variable
   in conjunction with a package name override. Here is an example:
   RREPLACES_${PN} = "other_package_being_replaced"

   BitBake, which the OpenEmbedded build system uses, supports
   specifying versioned replacements. Although the syntax varies
   depending on the packaging format, BitBake hides these differences
   from you. Here is the general syntax to specify versions with the
   ``RREPLACES`` variable: RREPLACES_${PN} = "package (operator
   version)" For ``operator``, you can specify the following: = < > <=
   >= For example, the following sets up a replacement using version 1.2
   or greater of the package ``foo``: RREPLACES_${PN} = "foo (>= 1.2)"

RSUGGESTS
   A list of additional packages that you can suggest for installation
   by the package manager at the time a package is installed. Not all
   package managers support this functionality.

   As with all package-controlling variables, you must always use this
   variable in conjunction with a package name override. Here is an
   example: RSUGGESTS_${PN} = "useful_package another_package"

S
   The location in the `Build Directory <#build-directory>`__ where
   unpacked recipe source code resides. By default, this directory is
   ``${``\ ```WORKDIR`` <#var-WORKDIR>`__\ ``}/${``\ ```BPN`` <#var-BPN>`__\ ``}-${``\ ```PV`` <#var-PV>`__\ ``}``,
   where ``${BPN}`` is the base recipe name and ``${PV}`` is the recipe
   version. If the source tarball extracts the code to a directory named
   anything other than ``${BPN}-${PV}``, or if the source code is
   fetched from an SCM such as Git or Subversion, then you must set
   ``S`` in the recipe so that the OpenEmbedded build system knows where
   to find the unpacked source.

   As an example, assume a `Source Directory <#source-directory>`__
   top-level folder named ``poky`` and a default Build Directory at
   ``poky/build``. In this case, the work directory the build system
   uses to keep the unpacked recipe for ``db`` is the following:
   poky/build/tmp/work/qemux86-poky-linux/db/5.1.19-r3/db-5.1.19 The
   unpacked source code resides in the ``db-5.1.19`` folder.

   This next example assumes a Git repository. By default, Git
   repositories are cloned to ``${WORKDIR}/git`` during
   ```do_fetch`` <#ref-tasks-fetch>`__. Since this path is different
   from the default value of ``S``, you must set it specifically so the
   source can be located: SRC_URI = "git://path/to/repo.git" S =
   "${WORKDIR}/git"

SANITY_REQUIRED_UTILITIES
   Specifies a list of command-line utilities that should be checked for
   during the initial sanity checking process when running BitBake. If
   any of the utilities are not installed on the build host, then
   BitBake immediately exits with an error.

SANITY_TESTED_DISTROS
   A list of the host distribution identifiers that the build system has
   been tested against. Identifiers consist of the host distributor ID
   followed by the release, as reported by the ``lsb_release`` tool or
   as read from ``/etc/lsb-release``. Separate the list items with
   explicit newline characters (``\n``). If ``SANITY_TESTED_DISTROS`` is
   not empty and the current value of
   ```NATIVELSBSTRING`` <#var-NATIVELSBSTRING>`__ does not appear in the
   list, then the build system reports a warning that indicates the
   current host distribution has not been tested as a build host.

SDK_ARCH
   The target architecture for the SDK. Typically, you do not directly
   set this variable. Instead, use ```SDKMACHINE`` <#var-SDKMACHINE>`__.

SDK_DEPLOY
   The directory set up and used by the
   ```populate_sdk_base`` <#ref-classes-populate-sdk>`__ class to which
   the SDK is deployed. The ``populate_sdk_base`` class defines
   ``SDK_DEPLOY`` as follows: SDK_DEPLOY = "${TMPDIR}/deploy/sdk"

SDK_DIR
   The parent directory used by the OpenEmbedded build system when
   creating SDK output. The
   ```populate_sdk_base`` <#ref-classes-populate-sdk-*>`__ class defines
   the variable as follows: SDK_DIR = "${WORKDIR}/sdk"

      **Note**

      The
      SDK_DIR
      directory is a temporary directory as it is part of
      WORKDIR
      . The final output directory is
      SDK_DEPLOY
      .

SDK_EXT_TYPE
   Controls whether or not shared state artifacts are copied into the
   extensible SDK. The default value of "full" copies all of the
   required shared state artifacts into the extensible SDK. The value
   "minimal" leaves these artifacts out of the SDK.

      **Note**

      If you set the variable to "minimal", you need to ensure
      SSTATE_MIRRORS
      is set in the SDK's configuration to enable the artifacts to be
      fetched as needed.

SDK_HOST_MANIFEST
   The manifest file for the host part of the SDK. This file lists all
   the installed packages that make up the host part of the SDK. The
   file contains package information on a line-per-package basis as
   follows: packagename packagearch version

   The ```populate_sdk_base`` <#ref-classes-populate-sdk-*>`__ class
   defines the manifest file as follows: SDK_HOST_MANIFEST =
   "${SDK_DEPLOY}/${TOOLCHAIN_OUTPUTNAME}.host.manifest" The location is
   derived using the ```SDK_DEPLOY`` <#var-SDK_DEPLOY>`__ and
   ```TOOLCHAIN_OUTPUTNAME`` <#var-TOOLCHAIN_OUTPUTNAME>`__ variables.

SDK_INCLUDE_PKGDATA
   When set to "1", specifies to include the packagedata for all recipes
   in the "world" target in the extensible SDK. Including this data
   allows the ``devtool search`` command to find these recipes in search
   results, as well as allows the ``devtool add`` command to map
   dependencies more effectively.

      **Note**

      Enabling the
      SDK_INCLUDE_PKGDATA
      variable significantly increases build time because all of world
      needs to be built. Enabling the variable also slightly increases
      the size of the extensible SDK.

SDK_INCLUDE_TOOLCHAIN
   When set to "1", specifies to include the toolchain in the extensible
   SDK. Including the toolchain is useful particularly when
   ```SDK_EXT_TYPE`` <#var-SDK_EXT_TYPE>`__ is set to "minimal" to keep
   the SDK reasonably small but you still want to provide a usable
   toolchain. For example, suppose you want to use the toolchain from an
   IDE or from other tools and you do not want to perform additional
   steps to install the toolchain.

   The ``SDK_INCLUDE_TOOLCHAIN`` variable defaults to "0" if
   ``SDK_EXT_TYPE`` is set to "minimal", and defaults to "1" if
   ``SDK_EXT_TYPE`` is set to "full".

SDK_INHERIT_BLACKLIST
   A list of classes to remove from the ```INHERIT`` <#var-INHERIT>`__
   value globally within the extensible SDK configuration. The
   ```populate-sdk-ext`` <#ref-classes-populate-sdk-*>`__ class sets the
   default value: SDK_INHERIT_BLACKLIST ?= "buildhistory icecc"

   Some classes are not generally applicable within the extensible SDK
   context. You can use this variable to disable those classes.

   For additional information on how to customize the extensible SDK's
   configuration, see the "`Configuring the Extensible
   SDK <&YOCTO_DOCS_SDK_URL;#sdk-configuring-the-extensible-sdk>`__"
   section in the Yocto Project Application Development and the
   Extensible Software Development Kit (eSDK) manual.

SDK_LOCAL_CONF_BLACKLIST
   A list of variables not allowed through from the OpenEmbedded build
   system configuration into the extensible SDK configuration. Usually,
   these are variables that are specific to the machine on which the
   build system is running and thus would be potentially problematic
   within the extensible SDK.

   By default, ``SDK_LOCAL_CONF_BLACKLIST`` is set in the
   ```populate-sdk-ext`` <#ref-classes-populate-sdk-*>`__ class and
   excludes the following variables:
   `CONF_VERSION <#var-CONF_VERSION>`__
   `BB_NUMBER_THREADS <#var-BB_NUMBER_THREADS>`__
   `BB_NUMBER_PARSE_THREADS <&YOCTO_DOCS_BB_URL;#var-BB_NUMBER_PARSE_THREADS>`__
   `PARALLEL_MAKE <#var-PARALLEL_MAKE>`__
   `PRSERV_HOST <#var-PRSERV_HOST>`__
   `SSTATE_MIRRORS <#var-SSTATE_MIRRORS>`__ `DL_DIR <#var-DL_DIR>`__
   `SSTATE_DIR <#var-SSTATE_DIR>`__ `TMPDIR <#var-TMPDIR>`__
   `BB_SERVER_TIMEOUT <#var-BB_SERVER_TIMEOUT>`__

   For additional information on how to customize the extensible SDK's
   configuration, see the "`Configuring the Extensible
   SDK <&YOCTO_DOCS_SDK_URL;#sdk-configuring-the-extensible-sdk>`__"
   section in the Yocto Project Application Development and the
   Extensible Software Development Kit (eSDK) manual.

SDK_LOCAL_CONF_WHITELIST
   A list of variables allowed through from the OpenEmbedded build
   system configuration into the extensible SDK configuration. By
   default, the list of variables is empty and is set in the
   ```populate-sdk-ext`` <#ref-classes-populate-sdk-*>`__ class.

   This list overrides the variables specified using the
   ```SDK_LOCAL_CONF_BLACKLIST`` <#var-SDK_LOCAL_CONF_BLACKLIST>`__
   variable as well as any variables identified by automatic
   blacklisting due to the "/" character being found at the start of the
   value, which is usually indicative of being a path and thus might not
   be valid on the system where the SDK is installed.

   For additional information on how to customize the extensible SDK's
   configuration, see the "`Configuring the Extensible
   SDK <&YOCTO_DOCS_SDK_URL;#sdk-configuring-the-extensible-sdk>`__"
   section in the Yocto Project Application Development and the
   Extensible Software Development Kit (eSDK) manual.

SDK_NAME
   The base name for SDK output files. The name is derived from the
   ```DISTRO`` <#var-DISTRO>`__, ```TCLIBC`` <#var-TCLIBC>`__,
   ```SDK_ARCH`` <#var-SDK_ARCH>`__,
   ```IMAGE_BASENAME`` <#var-IMAGE_BASENAME>`__, and
   ```TUNE_PKGARCH`` <#var-TUNE_PKGARCH>`__ variables: SDK_NAME =
   "${DISTRO}-${TCLIBC}-${SDK_ARCH}-${IMAGE_BASENAME}-${TUNE_PKGARCH}"

SDK_OS
   Specifies the operating system for which the SDK will be built. The
   default value is the value of ```BUILD_OS`` <#var-BUILD_OS>`__.

SDK_OUTPUT
   The location used by the OpenEmbedded build system when creating SDK
   output. The ```populate_sdk_base`` <#ref-classes-populate-sdk-*>`__
   class defines the variable as follows: SDK_DIR = "${WORKDIR}/sdk"
   SDK_OUTPUT = "${SDK_DIR}/image" SDK_DEPLOY = "${DEPLOY_DIR}/sdk"

      **Note**

      The
      SDK_OUTPUT
      directory is a temporary directory as it is part of
      WORKDIR
      by way of
      SDK_DIR
      . The final output directory is
      SDK_DEPLOY
      .

SDK_PACKAGE_ARCHS
   Specifies a list of architectures compatible with the SDK machine.
   This variable is set automatically and should not normally be
   hand-edited. Entries are separated using spaces and listed in order
   of priority. The default value for ``SDK_PACKAGE_ARCHS`` is "all any
   noarch ${SDK_ARCH}-${SDKPKGSUFFIX}".

SDK_POSTPROCESS_COMMAND
   Specifies a list of functions to call once the OpenEmbedded build
   system creates the SDK. You can specify functions separated by
   semicolons: SDK_POSTPROCESS_COMMAND += "function; ... "

   If you need to pass an SDK path to a command within a function, you
   can use ``${SDK_DIR}``, which points to the parent directory used by
   the OpenEmbedded build system when creating SDK output. See the
   ```SDK_DIR`` <#var-SDK_DIR>`__ variable for more information.

SDK_PREFIX
   The toolchain binary prefix used for ``nativesdk`` recipes. The
   OpenEmbedded build system uses the ``SDK_PREFIX`` value to set the
   ```TARGET_PREFIX`` <#var-TARGET_PREFIX>`__ when building
   ``nativesdk`` recipes. The default value is "${SDK_SYS}-".

SDK_RECRDEP_TASKS
   A list of shared state tasks added to the extensible SDK. By default,
   the following tasks are added: do_populate_lic do_package_qa
   do_populate_sysroot do_deploy Despite the default value of "" for the
   ``SDK_RECRDEP_TASKS`` variable, the above four tasks are always added
   to the SDK. To specify tasks beyond these four, you need to use the
   ``SDK_RECRDEP_TASKS`` variable (e.g. you are defining additional
   tasks that are needed in order to build
   ```SDK_TARGETS`` <#var-SDK_TARGETS>`__).

SDK_SYS
   Specifies the system, including the architecture and the operating
   system, for which the SDK will be built.

   The OpenEmbedded build system automatically sets this variable based
   on ```SDK_ARCH`` <#var-SDK_ARCH>`__,
   ```SDK_VENDOR`` <#var-SDK_VENDOR>`__, and
   ```SDK_OS`` <#var-SDK_OS>`__. You do not need to set the ``SDK_SYS``
   variable yourself.

SDK_TARGET_MANIFEST
   The manifest file for the target part of the SDK. This file lists all
   the installed packages that make up the target part of the SDK. The
   file contains package information on a line-per-package basis as
   follows: packagename packagearch version

   The ```populate_sdk_base`` <#ref-classes-populate-sdk-*>`__ class
   defines the manifest file as follows: SDK_TARGET_MANIFEST =
   "${SDK_DEPLOY}/${TOOLCHAIN_OUTPUTNAME}.target.manifest" The location
   is derived using the ```SDK_DEPLOY`` <#var-SDK_DEPLOY>`__ and
   ```TOOLCHAIN_OUTPUTNAME`` <#var-TOOLCHAIN_OUTPUTNAME>`__ variables.

SDK_TARGETS
   A list of targets to install from shared state as part of the
   standard or extensible SDK installation. The default value is "${PN}"
   (i.e. the image from which the SDK is built).

   The ``SDK_TARGETS`` variable is an internal variable and typically
   would not be changed.

SDK_TITLE
   The title to be printed when running the SDK installer. By default,
   this title is based on the ```DISTRO_NAME`` <#var-DISTRO_NAME>`__ or
   ```DISTRO`` <#var-DISTRO>`__ variable and is set in the
   ```populate_sdk_base`` <#ref-classes-populate-sdk-*>`__ class as
   follows: SDK_TITLE ??= "${@d.getVar('DISTRO_NAME') or
   d.getVar('DISTRO')} SDK" For the default distribution "poky",
   ``SDK_TITLE`` is set to "Poky (Yocto Project Reference Distro)".

   For information on how to change this default title, see the
   "`Changing the Extensible SDK Installer
   Title <&YOCTO_DOCS_SDK_URL;#sdk-changing-the-sdk-installer-title>`__"
   section in the Yocto Project Application Development and the
   Extensible Software Development Kit (eSDK) manual.

SDK_UPDATE_URL
   An optional URL for an update server for the extensible SDK. If set,
   the value is used as the default update server when running
   ``devtool sdk-update`` within the extensible SDK.

SDK_VENDOR
   Specifies the name of the SDK vendor.

SDK_VERSION
   Specifies the version of the SDK. The distribution configuration file
   (e.g. ``/meta-poky/conf/distro/poky.conf``) defines the
   ``SDK_VERSION`` as follows: SDK_VERSION =
   "${@d.getVar('DISTRO_VERSION').replace('snapshot-${DATE}','snapshot')}"

   For additional information, see the
   ```DISTRO_VERSION`` <#var-DISTRO_VERSION>`__ and
   ```DATE`` <#var-DATE>`__ variables.

SDKEXTPATH
   The default installation directory for the Extensible SDK. By
   default, this directory is based on the ```DISTRO`` <#var-DISTRO>`__
   variable and is set in the
   ```populate_sdk_base`` <#ref-classes-populate-sdk-*>`__ class as
   follows: SDKEXTPATH ??= "~/${@d.getVar('DISTRO')}_sdk" For the
   default distribution "poky", the ``SDKEXTPATH`` is set to "poky_sdk".

   For information on how to change this default directory, see the
   "`Changing the Default SDK Installation
   Directory <&YOCTO_DOCS_SDK_URL;#sdk-changing-the-default-sdk-installation-directory>`__"
   section in the Yocto Project Application Development and the
   Extensible Software Development Kit (eSDK) manual.

SDKIMAGE_FEATURES
   Equivalent to ``IMAGE_FEATURES``. However, this variable applies to
   the SDK generated from an image using the following command: $
   bitbake -c populate_sdk imagename

SDKMACHINE
   The machine for which the SDK is built. In other words, the SDK is
   built such that it runs on the target you specify with the
   ``SDKMACHINE`` value. The value points to a corresponding ``.conf``
   file under ``conf/machine-sdk/``.

   You can use "i686" and "x86_64" as possible values for this variable.
   The variable defaults to "i686" and is set in the local.conf file in
   the Build Directory. SDKMACHINE ?= "i686"

      **Note**

      You cannot set the
      SDKMACHINE
      variable in your distribution configuration file. If you do, the
      configuration will not take affect.

SDKPATH
   Defines the path offered to the user for installation of the SDK that
   is generated by the OpenEmbedded build system. The path appears as
   the default location for installing the SDK when you run the SDK's
   installation script. You can override the offered path when you run
   the script.

SDKTARGETSYSROOT
   The full path to the sysroot used for cross-compilation within an SDK
   as it will be when installed into the default
   ```SDKPATH`` <#var-SDKPATH>`__.

SECTION
   The section in which packages should be categorized. Package
   management utilities can make use of this variable.

SELECTED_OPTIMIZATION
   Specifies the optimization flags passed to the C compiler when
   building for the target. The flags are passed through the default
   value of the ```TARGET_CFLAGS`` <#var-TARGET_CFLAGS>`__ variable.

   The ``SELECTED_OPTIMIZATION`` variable takes the value of
   ``FULL_OPTIMIZATION`` unless ``DEBUG_BUILD`` = "1". If that is the
   case, the value of ``DEBUG_OPTIMIZATION`` is used.

SERIAL_CONSOLE
   Defines a serial console (TTY) to enable using
   `getty <https://en.wikipedia.org/wiki/Getty_(Unix)>`__. Provide a
   value that specifies the baud rate followed by the TTY device name
   separated by a space. You cannot specify more than one TTY device:
   SERIAL_CONSOLE = "115200 ttyS0"

      **Note**

      The
      SERIAL_CONSOLE
      variable is deprecated. Please use the
      SERIAL_CONSOLES
      variable.

SERIAL_CONSOLES
   Defines a serial console (TTY) to enable using
   `getty <https://en.wikipedia.org/wiki/Getty_(Unix)>`__. Provide a
   value that specifies the baud rate followed by the TTY device name
   separated by a semicolon. Use spaces to separate multiple devices:
   SERIAL_CONSOLES = "115200;ttyS0 115200;ttyS1"

SERIAL_CONSOLES_CHECK
   Specifies serial consoles, which must be listed in
   ```SERIAL_CONSOLES`` <#var-SERIAL_CONSOLES>`__, to check against
   ``/proc/console`` before enabling them using getty. This variable
   allows aliasing in the format: <device>:<alias>. If a device was
   listed as "sclp_line0" in ``/dev/`` and "ttyS0" was listed in
   ``/proc/console``, you would do the following: SERIAL_CONSOLES_CHECK
   = "slcp_line0:ttyS0" This variable is currently only supported with
   SysVinit (i.e. not with systemd).

SIGGEN_EXCLUDE_SAFE_RECIPE_DEPS
   A list of recipe dependencies that should not be used to determine
   signatures of tasks from one recipe when they depend on tasks from
   another recipe. For example: SIGGEN_EXCLUDE_SAFE_RECIPE_DEPS +=
   "intone->mplayer2"

   In the previous example, ``intone`` depends on ``mplayer2``.

   You can use the special token ``"*"`` on the left-hand side of the
   dependency to match all recipes except the one on the right-hand
   side. Here is an example: SIGGEN_EXCLUDE_SAFE_RECIPE_DEPS +=
   "*->quilt-native"

   In the previous example, all recipes except ``quilt-native`` ignore
   task signatures from the ``quilt-native`` recipe when determining
   their task signatures.

   Use of this variable is one mechanism to remove dependencies that
   affect task signatures and thus force rebuilds when a recipe changes.

      **Note**

      If you add an inappropriate dependency for a recipe relationship,
      the software might break during runtime if the interface of the
      second recipe was changed after the first recipe had been built.

SIGGEN_EXCLUDERECIPES_ABISAFE
   A list of recipes that are completely stable and will never change.
   The ABI for the recipes in the list are presented by output from the
   tasks run to build the recipe. Use of this variable is one way to
   remove dependencies from one recipe on another that affect task
   signatures and thus force rebuilds when the recipe changes.

      **Note**

      If you add an inappropriate variable to this list, the software
      might break at runtime if the interface of the recipe was changed
      after the other had been built.

SITEINFO_BITS
   Specifies the number of bits for the target system CPU. The value
   should be either "32" or "64".

SITEINFO_ENDIANNESS
   Specifies the endian byte order of the target system. The value
   should be either "le" for little-endian or "be" for big-endian.

SKIP_FILEDEPS
   Enables removal of all files from the "Provides" section of an RPM
   package. Removal of these files is required for packages containing
   prebuilt binaries and libraries such as ``libstdc++`` and ``glibc``.

   To enable file removal, set the variable to "1" in your
   ``conf/local.conf`` configuration file in your: `Build
   Directory <#build-directory>`__. SKIP_FILEDEPS = "1"

SOC_FAMILY
   Groups together machines based upon the same family of SOC (System On
   Chip). You typically set this variable in a common ``.inc`` file that
   you include in the configuration files of all the machines.

      **Note**

      You must include
      conf/machine/include/soc-family.inc
      for this variable to appear in
      MACHINEOVERRIDES
      .

SOLIBS
   Defines the suffix for shared libraries used on the target platform.
   By default, this suffix is ".so.*" for all Linux-based systems and is
   defined in the ``meta/conf/bitbake.conf`` configuration file.

   You will see this variable referenced in the default values of
   ``FILES_${PN}``.

SOLIBSDEV
   Defines the suffix for the development symbolic link (symlink) for
   shared libraries on the target platform. By default, this suffix is
   ".so" for Linux-based systems and is defined in the
   ``meta/conf/bitbake.conf`` configuration file.

   You will see this variable referenced in the default values of
   ``FILES_${PN}-dev``.

SOURCE_MIRROR_FETCH
   When you are fetching files to create a mirror of sources (i.e.
   creating a source mirror), setting ``SOURCE_MIRROR_FETCH`` to "1" in
   your ``local.conf`` configuration file ensures the source for all
   recipes are fetched regardless of whether or not a recipe is
   compatible with the configuration. A recipe is considered
   incompatible with the currently configured machine when either or
   both the ```COMPATIBLE_MACHINE`` <#var-COMPATIBLE_MACHINE>`__
   variable and ```COMPATIBLE_HOST`` <#var-COMPATIBLE_HOST>`__ variables
   specify compatibility with a machine other than that of the current
   machine or host.

      **Note**

      Do not set the
      SOURCE_MIRROR_FETCH
      variable unless you are creating a source mirror. In other words,
      do not set the variable during a normal build.

SOURCE_MIRROR_URL
   Defines your own ```PREMIRRORS`` <#var-PREMIRRORS>`__ from which to
   first fetch source before attempting to fetch from the upstream
   specified in ```SRC_URI`` <#var-SRC_URI>`__.

   To use this variable, you must globally inherit the
   ```own-mirrors`` <#ref-classes-own-mirrors>`__ class and then provide
   the URL to your mirrors. Here is the general syntax: INHERIT +=
   "own-mirrors" SOURCE_MIRROR_URL =
   "http://example.com/my_source_mirror"

      **Note**

      You can specify only a single URL in
      SOURCE_MIRROR_URL
      .

SPDXLICENSEMAP
   Maps commonly used license names to their SPDX counterparts found in
   ``meta/files/common-licenses/``. For the default ``SPDXLICENSEMAP``
   mappings, see the ``meta/conf/licenses.conf`` file.

   For additional information, see the ```LICENSE`` <#var-LICENSE>`__
   variable.

SPECIAL_PKGSUFFIX
   A list of prefixes for ```PN`` <#var-PN>`__ used by the OpenEmbedded
   build system to create variants of recipes or packages. The list
   specifies the prefixes to strip off during certain circumstances such
   as the generation of the ```BPN`` <#var-BPN>`__ variable.

SPL_BINARY
   The file type for the Secondary Program Loader (SPL). Some devices
   use an SPL from which to boot (e.g. the BeagleBone development
   board). For such cases, you can declare the file type of the SPL
   binary in the ``u-boot.inc`` include file, which is used in the
   U-Boot recipe.

   The SPL file type is set to "null" by default in the ``u-boot.inc``
   file as follows: # Some versions of u-boot build an SPL (Second
   Program Loader) image that # should be packaged along with the u-boot
   binary as well as placed in the # deploy directory. For those
   versions they can set the following variables # to allow packaging
   the SPL. SPL_BINARY ?= "" SPL_BINARYNAME ?=
   "${@os.path.basename(d.getVar("SPL_BINARY"))}" SPL_IMAGE ?=
   "${SPL_BINARYNAME}-${MACHINE}-${PV}-${PR}" SPL_SYMLINK ?=
   "${SPL_BINARYNAME}-${MACHINE}" The ``SPL_BINARY`` variable helps form
   various ``SPL_*`` variables used by the OpenEmbedded build system.

   See the BeagleBone machine configuration example in the "`Creating a
   new BSP Layer Using the ``bitbake-layers``
   Script <&YOCTO_DOCS_BSP_URL;#creating-a-new-bsp-layer-using-the-bitbake-layers-script>`__"
   section in the Yocto Project Board Support Package Developer's Guide
   for additional information.

SRC_URI
   The list of source files - local or remote. This variable tells the
   OpenEmbedded build system which bits to pull in for the build and how
   to pull them in. For example, if the recipe or append file only needs
   to fetch a tarball from the Internet, the recipe or append file uses
   a single ``SRC_URI`` entry. On the other hand, if the recipe or
   append file needs to fetch a tarball, apply two patches, and include
   a custom file, the recipe or append file would include four instances
   of the variable.

   The following list explains the available URI protocols. URI
   protocols are highly dependent on particular BitBake Fetcher
   submodules. Depending on the fetcher BitBake uses, various URL
   parameters are employed. For specifics on the supported Fetchers, see
   the "`Fetchers <&YOCTO_DOCS_BB_URL;#bb-fetchers>`__" section in the
   BitBake User Manual.

   -  *``file://`` -* Fetches files, which are usually files shipped
      with the `Metadata <#metadata>`__, from the local machine (e.g.
      `patch <&YOCTO_DOCS_OM_URL;#patching-dev-environment>`__ files).
      The path is relative to the ```FILESPATH`` <#var-FILESPATH>`__
      variable. Thus, the build system searches, in order, from the
      following directories, which are assumed to be a subdirectories of
      the directory in which the recipe file (``.bb``) or append file
      (``.bbappend``) resides:

      -  *``${BPN}`` -* The base recipe name without any special suffix
         or version numbers.

      -  *``${BP}`` -* ``${BPN}-${PV}``. The base recipe name and
         version but without any special package name suffix.

      -  *files -* Files within a directory, which is named ``files``
         and is also alongside the recipe or append file.

      ..

         **Note**

         If you want the build system to pick up files specified through
         a
         SRC_URI
         statement from your append file, you need to be sure to extend
         the
         FILESPATH
         variable by also using the
         FILESEXTRAPATHS
         variable from within your append file.

   -  *``bzr://`` -* Fetches files from a Bazaar revision control
      repository.

   -  *``git://`` -* Fetches files from a Git revision control
      repository.

   -  *``osc://`` -* Fetches files from an OSC (OpenSUSE Build service)
      revision control repository.

   -  *``repo://`` -* Fetches files from a repo (Git) repository.

   -  *``ccrc://`` -* Fetches files from a ClearCase repository.

   -  *``http://`` -* Fetches files from the Internet using ``http``.

   -  *``https://`` -* Fetches files from the Internet using ``https``.

   -  *``ftp://`` -* Fetches files from the Internet using ``ftp``.

   -  *``cvs://`` -* Fetches files from a CVS revision control
      repository.

   -  *``hg://`` -* Fetches files from a Mercurial (``hg``) revision
      control repository.

   -  *``p4://`` -* Fetches files from a Perforce (``p4``) revision
      control repository.

   -  *``ssh://`` -* Fetches files from a secure shell.

   -  *``svn://`` -* Fetches files from a Subversion (``svn``) revision
      control repository.

   -  *``npm://`` -* Fetches JavaScript modules from a registry.

   Standard and recipe-specific options for ``SRC_URI`` exist. Here are
   standard options:

   -  *``apply`` -* Whether to apply the patch or not. The default
      action is to apply the patch.

   -  *``striplevel`` -* Which striplevel to use when applying the
      patch. The default level is 1.

   -  *``patchdir`` -* Specifies the directory in which the patch should
      be applied. The default is ``${``\ ```S`` <#var-S>`__\ ``}``.

   Here are options specific to recipes building code from a revision
   control system:

   -  *``mindate`` -* Apply the patch only if
      ```SRCDATE`` <#var-SRCDATE>`__ is equal to or greater than
      ``mindate``.

   -  *``maxdate`` -* Apply the patch only if ``SRCDATE`` is not later
      than ``maxdate``.

   -  *``minrev`` -* Apply the patch only if ``SRCREV`` is equal to or
      greater than ``minrev``.

   -  *``maxrev`` -* Apply the patch only if ``SRCREV`` is not later
      than ``maxrev``.

   -  *``rev`` -* Apply the patch only if ``SRCREV`` is equal to
      ``rev``.

   -  *``notrev`` -* Apply the patch only if ``SRCREV`` is not equal to
      ``rev``.

   Here are some additional options worth mentioning:

   -  *``unpack`` -* Controls whether or not to unpack the file if it is
      an archive. The default action is to unpack the file.

   -  *``destsuffix`` -* Places the file (or extracts its contents) into
      the specified subdirectory of ```WORKDIR`` <#var-WORKDIR>`__ when
      the Git fetcher is used.

   -  *``subdir`` -* Places the file (or extracts its contents) into the
      specified subdirectory of ``WORKDIR`` when the local (``file://``)
      fetcher is used.

   -  *``localdir`` -* Places the file (or extracts its contents) into
      the specified subdirectory of ``WORKDIR`` when the CVS fetcher is
      used.

   -  *``subpath`` -* Limits the checkout to a specific subpath of the
      tree when using the Git fetcher is used.

   -  *``name`` -* Specifies a name to be used for association with
      ``SRC_URI`` checksums when you have more than one file specified
      in ``SRC_URI``.

   -  *``downloadfilename`` -* Specifies the filename used when storing
      the downloaded file.

SRC_URI_OVERRIDES_PACKAGE_ARCH
   By default, the OpenEmbedded build system automatically detects
   whether ``SRC_URI`` contains files that are machine-specific. If so,
   the build system automatically changes ``PACKAGE_ARCH``. Setting this
   variable to "0" disables this behavior.

SRCDATE
   The date of the source code used to build the package. This variable
   applies only if the source was fetched from a Source Code Manager
   (SCM).

SRCPV
   Returns the version string of the current package. This string is
   used to help define the value of ```PV`` <#var-PV>`__.

   The ``SRCPV`` variable is defined in the ``meta/conf/bitbake.conf``
   configuration file in the `Source Directory <#source-directory>`__ as
   follows: SRCPV = "${@bb.fetch2.get_srcrev(d)}"

   Recipes that need to define ``PV`` do so with the help of the
   ``SRCPV``. For example, the ``ofono`` recipe (``ofono_git.bb``)
   located in ``meta/recipes-connectivity`` in the Source Directory
   defines ``PV`` as follows: PV = "0.12-git${SRCPV}"

SRCREV
   The revision of the source code used to build the package. This
   variable applies to Subversion, Git, Mercurial, and Bazaar only. Note
   that if you want to build a fixed revision and you want to avoid
   performing a query on the remote repository every time BitBake parses
   your recipe, you should specify a ``SRCREV`` that is a full revision
   identifier and not just a tag.

      **Note**

      For information on limitations when inheriting the latest revision
      of software using
      SRCREV
      , see the
      AUTOREV
      variable description and the "
      Automatically Incrementing a Binary Package Revision Number
      " section, which is in the Yocto Project Development Tasks Manual.

SSTATE_DIR
   The directory for the shared state cache.

SSTATE_MIRROR_ALLOW_NETWORK
   If set to "1", allows fetches from mirrors that are specified in
   ```SSTATE_MIRRORS`` <#var-SSTATE_MIRRORS>`__ to work even when
   fetching from the network is disabled by setting ``BB_NO_NETWORK`` to
   "1". Using the ``SSTATE_MIRROR_ALLOW_NETWORK`` variable is useful if
   you have set ``SSTATE_MIRRORS`` to point to an internal server for
   your shared state cache, but you want to disable any other fetching
   from the network.

SSTATE_MIRRORS
   Configures the OpenEmbedded build system to search other mirror
   locations for prebuilt cache data objects before building out the
   data. This variable works like fetcher ```MIRRORS`` <#var-MIRRORS>`__
   and ```PREMIRRORS`` <#var-PREMIRRORS>`__ and points to the cache
   locations to check for the shared state (sstate) objects.

   You can specify a filesystem directory or a remote URL such as HTTP
   or FTP. The locations you specify need to contain the shared state
   cache (sstate-cache) results from previous builds. The sstate-cache
   you point to can also be from builds on other machines.

   When pointing to sstate build artifacts on another machine that uses
   a different GCC version for native builds, you must configure
   ``SSTATE_MIRROR`` with a regular expression that maps local search
   paths to server paths. The paths need to take into account
   ```NATIVELSBSTRING`` <#var-NATIVELSBSTRING>`__ set by the
   ```uninative`` <#ref-classes-uninative>`__ class. For example, the
   following maps the local search path ``universal-4.9`` to the
   server-provided path server_url_sstate_path: SSTATE_MIRRORS ?=
   file://universal-4.9/(.*)
   http://server_url_sstate_path/universal-4.8/\1 \\n

   If a mirror uses the same structure as
   ```SSTATE_DIR`` <#var-SSTATE_DIR>`__, you need to add "PATH" at the
   end as shown in the examples below. The build system substitutes the
   correct path within the directory structure. SSTATE_MIRRORS ?= "\\
   file://.\*
   http://someserver.tld/share/sstate/PATH;downloadfilename=PATH \\n \\
   file://.\* file:///some-local-dir/sstate/PATH"

SSTATE_SCAN_FILES
   Controls the list of files the OpenEmbedded build system scans for
   hardcoded installation paths. The variable uses a space-separated
   list of filenames (not paths) with standard wildcard characters
   allowed.

   During a build, the OpenEmbedded build system creates a shared state
   (sstate) object during the first stage of preparing the sysroots.
   That object is scanned for hardcoded paths for original installation
   locations. The list of files that are scanned for paths is controlled
   by the ``SSTATE_SCAN_FILES`` variable. Typically, recipes add files
   they want to be scanned to the value of ``SSTATE_SCAN_FILES`` rather
   than the variable being comprehensively set. The
   ```sstate`` <#ref-classes-sstate>`__ class specifies the default list
   of files.

   For details on the process, see the
   ```staging`` <#ref-classes-staging>`__ class.

STAGING_BASE_LIBDIR_NATIVE
   Specifies the path to the ``/lib`` subdirectory of the sysroot
   directory for the build host.

STAGING_BASELIBDIR
   Specifies the path to the ``/lib`` subdirectory of the sysroot
   directory for the target for which the current recipe is being built
   (```STAGING_DIR_HOST`` <#var-STAGING_DIR_HOST>`__).

STAGING_BINDIR
   Specifies the path to the ``/usr/bin`` subdirectory of the sysroot
   directory for the target for which the current recipe is being built
   (```STAGING_DIR_HOST`` <#var-STAGING_DIR_HOST>`__).

STAGING_BINDIR_CROSS
   Specifies the path to the directory containing binary configuration
   scripts. These scripts provide configuration information for other
   software that wants to make use of libraries or include files
   provided by the software associated with the script.

      **Note**

      This style of build configuration has been largely replaced by
      pkg-config
      . Consequently, if
      pkg-config
      is supported by the library to which you are linking, it is
      recommended you use
      pkg-config
      instead of a provided configuration script.

STAGING_BINDIR_NATIVE
   Specifies the path to the ``/usr/bin`` subdirectory of the sysroot
   directory for the build host.

STAGING_DATADIR
   Specifies the path to the ``/usr/share`` subdirectory of the sysroot
   directory for the target for which the current recipe is being built
   (```STAGING_DIR_HOST`` <#var-STAGING_DIR_HOST>`__).

STAGING_DATADIR_NATIVE
   Specifies the path to the ``/usr/share`` subdirectory of the sysroot
   directory for the build host.

STAGING_DIR
   Helps construct the ``recipe-sysroots`` directory, which is used
   during packaging.

   For information on how staging for recipe-specific sysroots occurs,
   see the ```do_populate_sysroot`` <#ref-tasks-populate_sysroot>`__
   task, the "`Sharing Files Between
   Recipes <&YOCTO_DOCS_DEV_URL;#new-sharing-files-between-recipes>`__"
   section in the Yocto Project Development Tasks Manual, the
   "`Configuration, Compilation, and
   Staging <&YOCTO_DOCS_OM_URL;#configuration-compilation-and-staging-dev-environment>`__"
   section in the Yocto Project Overview and Concepts Manual, and the
   ```SYSROOT_DIRS`` <#var-SYSROOT_DIRS>`__ variable.

      **Note**

      Recipes should never write files directly under the
      STAGING_DIR
      directory because the OpenEmbedded build system manages the
      directory automatically. Instead, files should be installed to
      ${
      D
      }
      within your recipe's
      do_install
      task and then the OpenEmbedded build system will stage a subset of
      those files into the sysroot.

STAGING_DIR_HOST
   Specifies the path to the sysroot directory for the system on which
   the component is built to run (the system that hosts the component).
   For most recipes, this sysroot is the one in which that recipe's
   ```do_populate_sysroot`` <#ref-tasks-populate_sysroot>`__ task copies
   files. Exceptions include ``-native`` recipes, where the
   ``do_populate_sysroot`` task instead uses
   ```STAGING_DIR_NATIVE`` <#var-STAGING_DIR_NATIVE>`__. Depending on
   the type of recipe and the build target, ``STAGING_DIR_HOST`` can
   have the following values:

   -  For recipes building for the target machine, the value is
      "${`STAGING_DIR <#var-STAGING_DIR>`__}/${`MACHINE <#var-MACHINE>`__}".

   -  For native recipes building for the build host, the value is empty
      given the assumption that when building for the build host, the
      build host's own directories should be used.

         **Note**

         ``-native`` recipes are not installed into host paths like such
         as ``/usr``. Rather, these recipes are installed into
         ``STAGING_DIR_NATIVE``. When compiling ``-native`` recipes,
         standard build environment variables such as
         ```CPPFLAGS`` <#var-CPPFLAGS>`__ and
         ```CFLAGS`` <#var-CFLAGS>`__ are set up so that both host paths
         and ``STAGING_DIR_NATIVE`` are searched for libraries and
         headers using, for example, GCC's ``-isystem`` option.

         Thus, the emphasis is that the ``STAGING_DIR*`` variables
         should be viewed as input variables by tasks such as
         ```do_configure`` <#ref-tasks-configure>`__,
         ```do_compile`` <#ref-tasks-compile>`__, and
         ```do_install`` <#ref-tasks-install>`__. Having the real system
         root correspond to ``STAGING_DIR_HOST`` makes conceptual sense
         for ``-native`` recipes, as they make use of host headers and
         libraries.

STAGING_DIR_NATIVE
   Specifies the path to the sysroot directory used when building
   components that run on the build host itself.

STAGING_DIR_TARGET
   Specifies the path to the sysroot used for the system for which the
   component generates code. For components that do not generate code,
   which is the majority, ``STAGING_DIR_TARGET`` is set to match
   ```STAGING_DIR_HOST`` <#var-STAGING_DIR_HOST>`__.

   Some recipes build binaries that can run on the target system but
   those binaries in turn generate code for another different system
   (e.g. cross-canadian recipes). Using terminology from GNU, the
   primary system is referred to as the "HOST" and the secondary, or
   different, system is referred to as the "TARGET". Thus, the binaries
   run on the "HOST" system and generate binaries for the "TARGET"
   system. The ``STAGING_DIR_HOST`` variable points to the sysroot used
   for the "HOST" system, while ``STAGING_DIR_TARGET`` points to the
   sysroot used for the "TARGET" system.

STAGING_ETCDIR_NATIVE
   Specifies the path to the ``/etc`` subdirectory of the sysroot
   directory for the build host.

STAGING_EXECPREFIXDIR
   Specifies the path to the ``/usr`` subdirectory of the sysroot
   directory for the target for which the current recipe is being built
   (```STAGING_DIR_HOST`` <#var-STAGING_DIR_HOST>`__).

STAGING_INCDIR
   Specifies the path to the ``/usr/include`` subdirectory of the
   sysroot directory for the target for which the current recipe being
   built (```STAGING_DIR_HOST`` <#var-STAGING_DIR_HOST>`__).

STAGING_INCDIR_NATIVE
   Specifies the path to the ``/usr/include`` subdirectory of the
   sysroot directory for the build host.

STAGING_KERNEL_BUILDDIR
   Points to the directory containing the kernel build artifacts.
   Recipes building software that needs to access kernel build artifacts
   (e.g. ``systemtap-uprobes``) can look in the directory specified with
   the ``STAGING_KERNEL_BUILDDIR`` variable to find these artifacts
   after the kernel has been built.

STAGING_KERNEL_DIR
   The directory with kernel headers that are required to build
   out-of-tree modules.

STAGING_LIBDIR
   Specifies the path to the ``/usr/lib`` subdirectory of the sysroot
   directory for the target for which the current recipe is being built
   (```STAGING_DIR_HOST`` <#var-STAGING_DIR_HOST>`__).

STAGING_LIBDIR_NATIVE
   Specifies the path to the ``/usr/lib`` subdirectory of the sysroot
   directory for the build host.

STAMP
   Specifies the base path used to create recipe stamp files. The path
   to an actual stamp file is constructed by evaluating this string and
   then appending additional information. Currently, the default
   assignment for ``STAMP`` as set in the ``meta/conf/bitbake.conf``
   file is: STAMP =
   "${STAMPS_DIR}/${MULTIMACH_TARGET_SYS}/${PN}/${EXTENDPE}${PV}-${PR}"

   For information on how BitBake uses stamp files to determine if a
   task should be rerun, see the "`Stamp Files and the Rerunning of
   Tasks <&YOCTO_DOCS_OM_URL;#stamp-files-and-the-rerunning-of-tasks>`__"
   section in the Yocto Project Overview and Concepts Manual.

   See ```STAMPS_DIR`` <#var-STAMPS_DIR>`__,
   ```MULTIMACH_TARGET_SYS`` <#var-MULTIMACH_TARGET_SYS>`__,
   ```PN`` <#var-PN>`__, ```EXTENDPE`` <#var-EXTENDPE>`__,
   ```PV`` <#var-PV>`__, and ```PR`` <#var-PR>`__ for related variable
   information.

STAMPS_DIR
   Specifies the base directory in which the OpenEmbedded build system
   places stamps. The default directory is ``${TMPDIR}/stamps``.

STRIP
   The minimal command and arguments to run ``strip``, which is used to
   strip symbols.

SUMMARY
   The short (72 characters or less) summary of the binary package for
   packaging systems such as ``opkg``, ``rpm``, or ``dpkg``. By default,
   ``SUMMARY`` is used to define the
   ```DESCRIPTION`` <#var-DESCRIPTION>`__ variable if ``DESCRIPTION`` is
   not set in the recipe.

SVNDIR
   The directory in which files checked out of a Subversion system are
   stored.

SYSLINUX_DEFAULT_CONSOLE
   Specifies the kernel boot default console. If you want to use a
   console other than the default, set this variable in your recipe as
   follows where "X" is the console number you want to use:
   SYSLINUX_DEFAULT_CONSOLE = "console=ttyX"

   The ```syslinux`` <#ref-classes-syslinux>`__ class initially sets
   this variable to null but then checks for a value later.

SYSLINUX_OPTS
   Lists additional options to add to the syslinux file. You need to set
   this variable in your recipe. If you want to list multiple options,
   separate the options with a semicolon character (``;``).

   The ```syslinux`` <#ref-classes-syslinux>`__ class uses this variable
   to create a set of options.

SYSLINUX_SERIAL
   Specifies the alternate serial port or turns it off. To turn off
   serial, set this variable to an empty string in your recipe. The
   variable's default value is set in the
   ```syslinux`` <#ref-classes-syslinux>`__ class as follows:
   SYSLINUX_SERIAL ?= "0 115200"

   The class checks for and uses the variable as needed.

SYSLINUX_SPLASH
   An ``.LSS`` file used as the background for the VGA boot menu when
   you use the boot menu. You need to set this variable in your recipe.

   The ```syslinux`` <#ref-classes-syslinux>`__ class checks for this
   variable and if found, the OpenEmbedded build system installs the
   splash screen.

SYSLINUX_SERIAL_TTY
   Specifies the alternate console=tty... kernel boot argument. The
   variable's default value is set in the
   ```syslinux`` <#ref-classes-syslinux>`__ class as follows:
   SYSLINUX_SERIAL_TTY ?= "console=ttyS0,115200"

   The class checks for and uses the variable as needed.

SYSROOT_DESTDIR
   Points to the temporary directory under the work directory (default
   "``${``\ ```WORKDIR`` <#var-WORKDIR>`__\ ``}/sysroot-destdir``")
   where the files populated into the sysroot are assembled during the
   ```do_populate_sysroot`` <#ref-tasks-populate_sysroot>`__ task.

SYSROOT_DIRS
   Directories that are staged into the sysroot by the
   ```do_populate_sysroot`` <#ref-tasks-populate_sysroot>`__ task. By
   default, the following directories are staged: SYSROOT_DIRS = " \\
   ${includedir} \\ ${libdir} \\ ${base_libdir} \\
   ${nonarch_base_libdir} \\ ${datadir} \\ "

SYSROOT_DIRS_BLACKLIST
   Directories that are not staged into the sysroot by the
   ```do_populate_sysroot`` <#ref-tasks-populate_sysroot>`__ task. You
   can use this variable to exclude certain subdirectories of
   directories listed in ```SYSROOT_DIRS`` <#var-SYSROOT_DIRS>`__ from
   staging. By default, the following directories are not staged:
   SYSROOT_DIRS_BLACKLIST = " \\ ${mandir} \\ ${docdir} \\ ${infodir} \\
   ${datadir}/locale \\ ${datadir}/applications \\ ${datadir}/fonts \\
   ${datadir}/pixmaps \\ "

SYSROOT_DIRS_NATIVE
   Extra directories staged into the sysroot by the
   ```do_populate_sysroot`` <#ref-tasks-populate_sysroot>`__ task for
   ``-native`` recipes, in addition to those specified in
   ```SYSROOT_DIRS`` <#var-SYSROOT_DIRS>`__. By default, the following
   extra directories are staged: SYSROOT_DIRS_NATIVE = " \\ ${bindir} \\
   ${sbindir} \\ ${base_bindir} \\ ${base_sbindir} \\ ${libexecdir} \\
   ${sysconfdir} \\ ${localstatedir} \\ "

      **Note**

      Programs built by
      -native
      recipes run directly from the sysroot (
      STAGING_DIR_NATIVE
      ), which is why additional directories containing program
      executables and supporting files need to be staged.

SYSROOT_PREPROCESS_FUNCS
   A list of functions to execute after files are staged into the
   sysroot. These functions are usually used to apply additional
   processing on the staged files, or to stage additional files.

SYSTEMD_AUTO_ENABLE
   When inheriting the ```systemd`` <#ref-classes-systemd>`__ class,
   this variable specifies whether the specified service in
   ```SYSTEMD_SERVICE`` <#var-SYSTEMD_SERVICE>`__ should start
   automatically or not. By default, the service is enabled to
   automatically start at boot time. The default setting is in the
   ```systemd`` <#ref-classes-systemd>`__ class as follows:
   SYSTEMD_AUTO_ENABLE ??= "enable"

   You can disable the service by setting the variable to "disable".

SYSTEMD_BOOT_CFG
   When ```EFI_PROVIDER`` <#var-EFI_PROVIDER>`__ is set to
   "systemd-boot", the ``SYSTEMD_BOOT_CFG`` variable specifies the
   configuration file that should be used. By default, the
   ```systemd-boot`` <#ref-classes-systemd-boot>`__ class sets the
   ``SYSTEMD_BOOT_CFG`` as follows: SYSTEMD_BOOT_CFG ?=
   "${`S <#var-S>`__}/loader.conf"

   For information on Systemd-boot, see the `Systemd-boot
   documentation <http://www.freedesktop.org/wiki/Software/systemd/systemd-boot/>`__.

SYSTEMD_BOOT_ENTRIES
   When ```EFI_PROVIDER`` <#var-EFI_PROVIDER>`__ is set to
   "systemd-boot", the ``SYSTEMD_BOOT_ENTRIES`` variable specifies a
   list of entry files (``*.conf``) to install that contain one boot
   entry per file. By default, the
   ```systemd-boot`` <#ref-classes-systemd-boot>`__ class sets the
   ``SYSTEMD_BOOT_ENTRIES`` as follows: SYSTEMD_BOOT_ENTRIES ?= ""

   For information on Systemd-boot, see the `Systemd-boot
   documentation <http://www.freedesktop.org/wiki/Software/systemd/systemd-boot/>`__.

SYSTEMD_BOOT_TIMEOUT
   When ```EFI_PROVIDER`` <#var-EFI_PROVIDER>`__ is set to
   "systemd-boot", the ``SYSTEMD_BOOT_TIMEOUT`` variable specifies the
   boot menu timeout in seconds. By default, the
   ```systemd-boot`` <#ref-classes-systemd-boot>`__ class sets the
   ``SYSTEMD_BOOT_TIMEOUT`` as follows: SYSTEMD_BOOT_TIMEOUT ?= "10"

   For information on Systemd-boot, see the `Systemd-boot
   documentation <http://www.freedesktop.org/wiki/Software/systemd/systemd-boot/>`__.

SYSTEMD_PACKAGES
   When inheriting the ```systemd`` <#ref-classes-systemd>`__ class,
   this variable locates the systemd unit files when they are not found
   in the main recipe's package. By default, the ``SYSTEMD_PACKAGES``
   variable is set such that the systemd unit files are assumed to
   reside in the recipes main package: SYSTEMD_PACKAGES ?= "${PN}"

   If these unit files are not in this recipe's main package, you need
   to use ``SYSTEMD_PACKAGES`` to list the package or packages in which
   the build system can find the systemd unit files.

SYSTEMD_SERVICE
   When inheriting the ```systemd`` <#ref-classes-systemd>`__ class,
   this variable specifies the systemd service name for a package.

   When you specify this file in your recipe, use a package name
   override to indicate the package to which the value applies. Here is
   an example from the connman recipe: SYSTEMD_SERVICE_${PN} =
   "connman.service"

SYSVINIT_ENABLED_GETTYS
   When using
   `SysVinit <&YOCTO_DOCS_DEV_URL;#new-recipe-enabling-system-services>`__,
   specifies a space-separated list of the virtual terminals that should
   run a `getty <http://en.wikipedia.org/wiki/Getty_%28Unix%29>`__
   (allowing login), assuming ```USE_VT`` <#var-USE_VT>`__ is not set to
   "0".

   The default value for ``SYSVINIT_ENABLED_GETTYS`` is "1" (i.e. only
   run a getty on the first virtual terminal).

T
   This variable points to a directory were BitBake places temporary
   files, which consist mostly of task logs and scripts, when building a
   particular recipe. The variable is typically set as follows: T =
   "${WORKDIR}/temp"

   The ```WORKDIR`` <#var-WORKDIR>`__ is the directory into which
   BitBake unpacks and builds the recipe. The default ``bitbake.conf``
   file sets this variable.

   The ``T`` variable is not to be confused with the
   ```TMPDIR`` <#var-TMPDIR>`__ variable, which points to the root of
   the directory tree where BitBake places the output of an entire
   build.

TARGET_ARCH
   The target machine's architecture. The OpenEmbedded build system
   supports many architectures. Here is an example list of architectures
   supported. This list is by no means complete as the architecture is
   configurable: arm i586 x86_64 powerpc powerpc64 mips mipsel

   For additional information on machine architectures, see the
   ```TUNE_ARCH`` <#var-TUNE_ARCH>`__ variable.

TARGET_AS_ARCH
   Specifies architecture-specific assembler flags for the target
   system. ``TARGET_AS_ARCH`` is initialized from
   ```TUNE_ASARGS`` <#var-TUNE_ASARGS>`__ by default in the BitBake
   configuration file (``meta/conf/bitbake.conf``): TARGET_AS_ARCH =
   "${TUNE_ASARGS}"

TARGET_CC_ARCH
   Specifies architecture-specific C compiler flags for the target
   system. ``TARGET_CC_ARCH`` is initialized from
   ```TUNE_CCARGS`` <#var-TUNE_CCARGS>`__ by default.

      **Note**

      It is a common workaround to append
      LDFLAGS
      to
      TARGET_CC_ARCH
      in recipes that build software for the target that would not
      otherwise respect the exported
      LDFLAGS
      variable.

TARGET_CC_KERNEL_ARCH
   This is a specific kernel compiler flag for a CPU or Application
   Binary Interface (ABI) tune. The flag is used rarely and only for
   cases where a userspace ```TUNE_CCARGS`` <#var-TUNE_CCARGS>`__ is not
   compatible with the kernel compilation. The ``TARGET_CC_KERNEL_ARCH``
   variable allows the kernel (and associated modules) to use a
   different configuration. See the
   ``meta/conf/machine/include/arm/feature-arm-thumb.inc`` file in the
   `Source Directory <#source-directory>`__ for an example.

TARGET_CFLAGS
   Specifies the flags to pass to the C compiler when building for the
   target. When building in the target context,
   ```CFLAGS`` <#var-CFLAGS>`__ is set to the value of this variable by
   default.

   Additionally, the SDK's environment setup script sets the ``CFLAGS``
   variable in the environment to the ``TARGET_CFLAGS`` value so that
   executables built using the SDK also have the flags applied.

TARGET_CPPFLAGS
   Specifies the flags to pass to the C pre-processor (i.e. to both the
   C and the C++ compilers) when building for the target. When building
   in the target context, ```CPPFLAGS`` <#var-CPPFLAGS>`__ is set to the
   value of this variable by default.

   Additionally, the SDK's environment setup script sets the
   ``CPPFLAGS`` variable in the environment to the ``TARGET_CPPFLAGS``
   value so that executables built using the SDK also have the flags
   applied.

TARGET_CXXFLAGS
   Specifies the flags to pass to the C++ compiler when building for the
   target. When building in the target context,
   ```CXXFLAGS`` <#var-CXXFLAGS>`__ is set to the value of this variable
   by default.

   Additionally, the SDK's environment setup script sets the
   ``CXXFLAGS`` variable in the environment to the ``TARGET_CXXFLAGS``
   value so that executables built using the SDK also have the flags
   applied.

TARGET_FPU
   Specifies the method for handling FPU code. For FPU-less targets,
   which include most ARM CPUs, the variable must be set to "soft". If
   not, the kernel emulation gets used, which results in a performance
   penalty.

TARGET_LD_ARCH
   Specifies architecture-specific linker flags for the target system.
   ``TARGET_LD_ARCH`` is initialized from
   ```TUNE_LDARGS`` <#var-TUNE_LDARGS>`__ by default in the BitBake
   configuration file (``meta/conf/bitbake.conf``): TARGET_LD_ARCH =
   "${TUNE_LDARGS}"

TARGET_LDFLAGS
   Specifies the flags to pass to the linker when building for the
   target. When building in the target context,
   ```LDFLAGS`` <#var-LDFLAGS>`__ is set to the value of this variable
   by default.

   Additionally, the SDK's environment setup script sets the
   ```LDFLAGS`` <#var-LDFLAGS>`__ variable in the environment to the
   ``TARGET_LDFLAGS`` value so that executables built using the SDK also
   have the flags applied.

TARGET_OS
   Specifies the target's operating system. The variable can be set to
   "linux" for glibc-based systems (GNU C Library) and to "linux-musl"
   for musl libc. For ARM/EABI targets, "linux-gnueabi" and
   "linux-musleabi" possible values exist.

TARGET_PREFIX
   Specifies the prefix used for the toolchain binary target tools.

   Depending on the type of recipe and the build target,
   ``TARGET_PREFIX`` is set as follows:

   -  For recipes building for the target machine, the value is
      "${`TARGET_SYS <#var-TARGET_SYS>`__}-".

   -  For native recipes, the build system sets the variable to the
      value of ``BUILD_PREFIX``.

   -  For native SDK recipes (``nativesdk``), the build system sets the
      variable to the value of ``SDK_PREFIX``.

TARGET_SYS
   Specifies the system, including the architecture and the operating
   system, for which the build is occurring in the context of the
   current recipe.

   The OpenEmbedded build system automatically sets this variable based
   on ```TARGET_ARCH`` <#var-TARGET_ARCH>`__,
   ```TARGET_VENDOR`` <#var-TARGET_VENDOR>`__, and
   ```TARGET_OS`` <#var-TARGET_OS>`__ variables.

      **Note**

      You do not need to set the
      TARGET_SYS
      variable yourself.

   Consider these two examples:

   -  Given a native recipe on a 32-bit, x86 machine running Linux, the
      value is "i686-linux".

   -  Given a recipe being built for a little-endian, MIPS target
      running Linux, the value might be "mipsel-linux".

TARGET_VENDOR
   Specifies the name of the target vendor.

TCLIBC
   Specifies the GNU standard C library (``libc``) variant to use during
   the build process. This variable replaces ``POKYLIBC``, which is no
   longer supported.

   You can select "glibc", "musl", "newlib", or "baremetal"

TCLIBCAPPEND
   Specifies a suffix to be appended onto the
   ```TMPDIR`` <#var-TMPDIR>`__ value. The suffix identifies the
   ``libc`` variant for building. When you are building for multiple
   variants with the same `Build Directory <#build-directory>`__, this
   mechanism ensures that output for different ``libc`` variants is kept
   separate to avoid potential conflicts.

   In the ``defaultsetup.conf`` file, the default value of
   ``TCLIBCAPPEND`` is "-${TCLIBC}". However, distros such as poky,
   which normally only support one ``libc`` variant, set
   ``TCLIBCAPPEND`` to "" in their distro configuration file resulting
   in no suffix being applied.

TCMODE
   Specifies the toolchain selector. ``TCMODE`` controls the
   characteristics of the generated packages and images by telling the
   OpenEmbedded build system which toolchain profile to use. By default,
   the OpenEmbedded build system builds its own internal toolchain. The
   variable's default value is "default", which uses that internal
   toolchain.

      **Note**

      If
      TCMODE
      is set to a value other than "default", then it is your
      responsibility to ensure that the toolchain is compatible with the
      default toolchain. Using older or newer versions of these
      components might cause build problems. See the Release Notes for
      the Yocto Project release for the specific components with which
      the toolchain must be compatible. To access the Release Notes, go
      to the
      Downloads
      page on the Yocto Project website and click on the "RELEASE
      INFORMATION" link for the appropriate release.

   The ``TCMODE`` variable is similar to ```TCLIBC`` <#var-TCLIBC>`__,
   which controls the variant of the GNU standard C library (``libc``)
   used during the build process: ``glibc`` or ``musl``.

   With additional layers, it is possible to use a pre-compiled external
   toolchain. One example is the Sourcery G++ Toolchain. The support for
   this toolchain resides in the separate Mentor Graphics
   ``meta-sourcery`` layer at
   ` <http://github.com/MentorEmbedded/meta-sourcery/>`__.

   The layer's ``README`` file contains information on how to use the
   Sourcery G++ Toolchain as an external toolchain. In summary, you must
   be sure to add the layer to your ``bblayers.conf`` file in front of
   the ``meta`` layer and then set the ``EXTERNAL_TOOLCHAIN`` variable
   in your ``local.conf`` file to the location in which you installed
   the toolchain.

   The fundamentals used for this example apply to any external
   toolchain. You can use ``meta-sourcery`` as a template for adding
   support for other external toolchains.

TEST_EXPORT_DIR
   The location the OpenEmbedded build system uses to export tests when
   the ```TEST_EXPORT_ONLY`` <#var-TEST_EXPORT_ONLY>`__ variable is set
   to "1".

   The ``TEST_EXPORT_DIR`` variable defaults to
   ``"${TMPDIR}/testimage/${PN}"``.

TEST_EXPORT_ONLY
   Specifies to export the tests only. Set this variable to "1" if you
   do not want to run the tests but you want them to be exported in a
   manner that you to run them outside of the build system.

TEST_LOG_DIR
   Holds the SSH log and the boot log for QEMU machines. The
   ``TEST_LOG_DIR`` variable defaults to ``"${WORKDIR}/testimage"``.

      **Note**

      Actual test results reside in the task log (
      log.do_testimage
      ), which is in the
      ${WORKDIR}/temp/
      directory.

TEST_POWERCONTROL_CMD
   For automated hardware testing, specifies the command to use to
   control the power of the target machine under test. Typically, this
   command would point to a script that performs the appropriate action
   (e.g. interacting with a web-enabled power strip). The specified
   command should expect to receive as the last argument "off", "on" or
   "cycle" specifying to power off, on, or cycle (power off and then
   power on) the device, respectively.

TEST_POWERCONTROL_EXTRA_ARGS
   For automated hardware testing, specifies additional arguments to
   pass through to the command specified in
   ```TEST_POWERCONTROL_CMD`` <#var-TEST_POWERCONTROL_CMD>`__. Setting
   ``TEST_POWERCONTROL_EXTRA_ARGS`` is optional. You can use it if you
   wish, for example, to separate the machine-specific and
   non-machine-specific parts of the arguments.

TEST_QEMUBOOT_TIMEOUT
   The time in seconds allowed for an image to boot before automated
   runtime tests begin to run against an image. The default timeout
   period to allow the boot process to reach the login prompt is 500
   seconds. You can specify a different value in the ``local.conf``
   file.

   For more information on testing images, see the "`Performing
   Automated Runtime
   Testing <&YOCTO_DOCS_DEV_URL;#performing-automated-runtime-testing>`__"
   section in the Yocto Project Development Tasks Manual.

TEST_SERIALCONTROL_CMD
   For automated hardware testing, specifies the command to use to
   connect to the serial console of the target machine under test. This
   command simply needs to connect to the serial console and forward
   that connection to standard input and output as any normal terminal
   program does.

   For example, to use the Picocom terminal program on serial device
   ``/dev/ttyUSB0`` at 115200bps, you would set the variable as follows:
   TEST_SERIALCONTROL_CMD = "picocom /dev/ttyUSB0 -b 115200"

TEST_SERIALCONTROL_EXTRA_ARGS
   For automated hardware testing, specifies additional arguments to
   pass through to the command specified in
   ```TEST_SERIALCONTROL_CMD`` <#var-TEST_SERIALCONTROL_CMD>`__. Setting
   ``TEST_SERIALCONTROL_EXTRA_ARGS`` is optional. You can use it if you
   wish, for example, to separate the machine-specific and
   non-machine-specific parts of the command.

TEST_SERVER_IP
   The IP address of the build machine (host machine). This IP address
   is usually automatically detected. However, if detection fails, this
   variable needs to be set to the IP address of the build machine (i.e.
   where the build is taking place).

      **Note**

      The
      TEST_SERVER_IP
      variable is only used for a small number of tests such as the
      "dnf" test suite, which needs to download packages from
      WORKDIR/oe-rootfs-repo
      .

TEST_TARGET
   Specifies the target controller to use when running tests against a
   test image. The default controller to use is "qemu": TEST_TARGET =
   "qemu"

   A target controller is a class that defines how an image gets
   deployed on a target and how a target is started. A layer can extend
   the controllers by adding a module in the layer's
   ``/lib/oeqa/controllers`` directory and by inheriting the
   ``BaseTarget`` class, which is an abstract class that cannot be used
   as a value of ``TEST_TARGET``.

   You can provide the following arguments with ``TEST_TARGET``:

   -  *"qemu":* Boots a QEMU image and runs the tests. See the
      "`Enabling Runtime Tests on
      QEMU <&YOCTO_DOCS_DEV_URL;#qemu-image-enabling-tests>`__" section
      in the Yocto Project Development Tasks Manual for more
      information.

   -  *"simpleremote":* Runs the tests on target hardware that is
      already up and running. The hardware can be on the network or it
      can be a device running an image on QEMU. You must also set
      ```TEST_TARGET_IP`` <#var-TEST_TARGET_IP>`__ when you use
      "simpleremote".

         **Note**

         This argument is defined in
         meta/lib/oeqa/controllers/simpleremote.py
         .

   For information on running tests on hardware, see the "`Enabling
   Runtime Tests on
   Hardware <&YOCTO_DOCS_DEV_URL;#hardware-image-enabling-tests>`__"
   section in the Yocto Project Development Tasks Manual.

TEST_TARGET_IP
   The IP address of your hardware under test. The ``TEST_TARGET_IP``
   variable has no effect when ```TEST_TARGET`` <#var-TEST_TARGET>`__ is
   set to "qemu".

   When you specify the IP address, you can also include a port. Here is
   an example: TEST_TARGET_IP = "192.168.1.4:2201" Specifying a port is
   useful when SSH is started on a non-standard port or in cases when
   your hardware under test is behind a firewall or network that is not
   directly accessible from your host and you need to do port address
   translation.

TEST_SUITES
   An ordered list of tests (modules) to run against an image when
   performing automated runtime testing.

   The OpenEmbedded build system provides a core set of tests that can
   be used against images.

      **Note**

      Currently, there is only support for running these tests under
      QEMU.

   Tests include ``ping``, ``ssh``, ``df`` among others. You can add
   your own tests to the list of tests by appending ``TEST_SUITES`` as
   follows: TEST_SUITES_append = " mytest" Alternatively, you can
   provide the "auto" option to have all applicable tests run against
   the image. TEST_SUITES_append = " auto" Using this option causes the
   build system to automatically run tests that are applicable to the
   image. Tests that are not applicable are skipped.

   The order in which tests are run is important. Tests that depend on
   another test must appear later in the list than the test on which
   they depend. For example, if you append the list of tests with two
   tests (``test_A`` and ``test_B``) where ``test_B`` is dependent on
   ``test_A``, then you must order the tests as follows: TEST_SUITES = "
   test_A test_B"

   For more information on testing images, see the "`Performing
   Automated Runtime
   Testing <&YOCTO_DOCS_DEV_URL;#performing-automated-runtime-testing>`__"
   section in the Yocto Project Development Tasks Manual.

TESTIMAGE_AUTO
   Automatically runs the series of automated tests for images when an
   image is successfully built. Setting ``TESTIMAGE_AUTO`` to "1" causes
   any image that successfully builds to automatically boot under QEMU.
   Using the variable also adds in dependencies so that any SDK for
   which testing is requested is automatically built first.

   These tests are written in Python making use of the ``unittest``
   module, and the majority of them run commands on the target system
   over ``ssh``. You can set this variable to "1" in your ``local.conf``
   file in the `Build Directory <#build-directory>`__ to have the
   OpenEmbedded build system automatically run these tests after an
   image successfully builds: TESTIMAGE_AUTO = "1" For more information
   on enabling, running, and writing these tests, see the "`Performing
   Automated Runtime
   Testing <&YOCTO_DOCS_DEV_URL;#performing-automated-runtime-testing>`__"
   section in the Yocto Project Development Tasks Manual and the
   "```testimage*.bbclass`` <#ref-classes-testimage*>`__" section.

THISDIR
   The directory in which the file BitBake is currently parsing is
   located. Do not manually set this variable.

TIME
   The time the build was started. Times appear using the hour, minute,
   and second (HMS) format (e.g. "140159" for one minute and fifty-nine
   seconds past 1400 hours).

TMPDIR
   This variable is the base directory the OpenEmbedded build system
   uses for all build output and intermediate files (other than the
   shared state cache). By default, the ``TMPDIR`` variable points to
   ``tmp`` within the `Build Directory <#build-directory>`__.

   If you want to establish this directory in a location other than the
   default, you can uncomment and edit the following statement in the
   ``conf/local.conf`` file in the `Source
   Directory <#source-directory>`__: #TMPDIR = "${TOPDIR}/tmp" An
   example use for this scenario is to set ``TMPDIR`` to a local disk,
   which does not use NFS, while having the Build Directory use NFS.

   The filesystem used by ``TMPDIR`` must have standard filesystem
   semantics (i.e. mixed-case files are unique, POSIX file locking, and
   persistent inodes). Due to various issues with NFS and bugs in some
   implementations, NFS does not meet this minimum requirement.
   Consequently, ``TMPDIR`` cannot be on NFS.

TOOLCHAIN_HOST_TASK
   This variable lists packages the OpenEmbedded build system uses when
   building an SDK, which contains a cross-development environment. The
   packages specified by this variable are part of the toolchain set
   that runs on the ```SDKMACHINE`` <#var-SDKMACHINE>`__, and each
   package should usually have the prefix ``nativesdk-``. For example,
   consider the following command when building an SDK: $ bitbake -c
   populate_sdk imagename In this case, a default list of packages is
   set in this variable, but you can add additional packages to the
   list. See the "`Adding Individual Packages to the Standard
   SDK <&YOCTO_DOCS_SDK_URL;#sdk-adding-individual-packages>`__" section
   in the Yocto Project Application Development and the Extensible
   Software Development Kit (eSDK) manual for more information.

   For background information on cross-development toolchains in the
   Yocto Project development environment, see the "`Cross-Development
   Toolchain
   Generation <&YOCTO_DOCS_OM_URL;#cross-development-toolchain-generation>`__"
   section in the Yocto Project Overview and Concepts Manual. For
   information on setting up a cross-development environment, see the
   `Yocto Project Application Development and the Extensible Software
   Development Kit (eSDK) <&YOCTO_DOCS_SDK_URL;>`__ manual.

TOOLCHAIN_OUTPUTNAME
   This variable defines the name used for the toolchain output. The
   ```populate_sdk_base`` <#ref-classes-populate-sdk-*>`__ class sets
   the ``TOOLCHAIN_OUTPUTNAME`` variable as follows:
   TOOLCHAIN_OUTPUTNAME ?= "${SDK_NAME}-toolchain-${SDK_VERSION}" See
   the ```SDK_NAME`` <#var-SDK_NAME>`__ and
   ```SDK_VERSION`` <#var-SDK_VERSION>`__ variables for additional
   information.

TOOLCHAIN_TARGET_TASK
   This variable lists packages the OpenEmbedded build system uses when
   it creates the target part of an SDK (i.e. the part built for the
   target hardware), which includes libraries and headers. Use this
   variable to add individual packages to the part of the SDK that runs
   on the target. See the "`Adding Individual Packages to the Standard
   SDK <&YOCTO_DOCS_SDK_URL;#sdk-adding-individual-packages>`__" section
   in the Yocto Project Application Development and the Extensible
   Software Development Kit (eSDK) manual for more information.

   For background information on cross-development toolchains in the
   Yocto Project development environment, see the "`Cross-Development
   Toolchain
   Generation <&YOCTO_DOCS_OM_URL;#cross-development-toolchain-generation>`__"
   section in the Yocto Project Overview and Concepts Manual. For
   information on setting up a cross-development environment, see the
   `Yocto Project Application Development and the Extensible Software
   Development Kit (eSDK) <&YOCTO_DOCS_SDK_URL;>`__ manual.

TOPDIR
   The top-level `Build Directory <#build-directory>`__. BitBake
   automatically sets this variable when you initialize your build
   environment using ````` <#structure-core-script>`__.

TRANSLATED_TARGET_ARCH
   A sanitized version of ```TARGET_ARCH`` <#var-TARGET_ARCH>`__. This
   variable is used where the architecture is needed in a value where
   underscores are not allowed, for example within package filenames. In
   this case, dash characters replace any underscore characters used in
   ``TARGET_ARCH``.

   Do not edit this variable.

TUNE_ARCH
   The GNU canonical architecture for a specific architecture (i.e.
   ``arm``, ``armeb``, ``mips``, ``mips64``, and so forth). BitBake uses
   this value to setup configuration.

   ``TUNE_ARCH`` definitions are specific to a given architecture. The
   definitions can be a single static definition, or can be dynamically
   adjusted. You can see details for a given CPU family by looking at
   the architecture's ``README`` file. For example, the
   ``meta/conf/machine/include/mips/README`` file in the `Source
   Directory <#source-directory>`__ provides information for
   ``TUNE_ARCH`` specific to the ``mips`` architecture.

   ``TUNE_ARCH`` is tied closely to
   ```TARGET_ARCH`` <#var-TARGET_ARCH>`__, which defines the target
   machine's architecture. The BitBake configuration file
   (``meta/conf/bitbake.conf``) sets ``TARGET_ARCH`` as follows:
   TARGET_ARCH = "${TUNE_ARCH}"

   The following list, which is by no means complete since architectures
   are configurable, shows supported machine architectures: arm i586
   x86_64 powerpc powerpc64 mips mipsel

TUNE_ASARGS
   Specifies architecture-specific assembler flags for the target
   system. The set of flags is based on the selected tune features.
   ``TUNE_ASARGS`` is set using the tune include files, which are
   typically under ``meta/conf/machine/include/`` and are influenced
   through ```TUNE_FEATURES`` <#var-TUNE_FEATURES>`__. For example, the
   ``meta/conf/machine/include/x86/arch-x86.inc`` file defines the flags
   for the x86 architecture as follows: TUNE_ASARGS +=
   "${@bb.utils.contains("TUNE_FEATURES", "mx32", "-x32", "", d)}"

      **Note**

      Board Support Packages (BSPs) select the tune. The selected tune,
      in turn, affects the tune variables themselves (i.e. the tune can
      supply its own set of flags).

TUNE_CCARGS
   Specifies architecture-specific C compiler flags for the target
   system. The set of flags is based on the selected tune features.
   ``TUNE_CCARGS`` is set using the tune include files, which are
   typically under ``meta/conf/machine/include/`` and are influenced
   through ```TUNE_FEATURES`` <#var-TUNE_FEATURES>`__.

      **Note**

      Board Support Packages (BSPs) select the tune. The selected tune,
      in turn, affects the tune variables themselves (i.e. the tune can
      supply its own set of flags).

TUNE_LDARGS
   Specifies architecture-specific linker flags for the target system.
   The set of flags is based on the selected tune features.
   ``TUNE_LDARGS`` is set using the tune include files, which are
   typically under ``meta/conf/machine/include/`` and are influenced
   through ```TUNE_FEATURES`` <#var-TUNE_FEATURES>`__. For example, the
   ``meta/conf/machine/include/x86/arch-x86.inc`` file defines the flags
   for the x86 architecture as follows: TUNE_LDARGS +=
   "${@bb.utils.contains("TUNE_FEATURES", "mx32", "-m elf32_x86_64", "",
   d)}"

      **Note**

      Board Support Packages (BSPs) select the tune. The selected tune,
      in turn, affects the tune variables themselves (i.e. the tune can
      supply its own set of flags).

TUNE_FEATURES
   Features used to "tune" a compiler for optimal use given a specific
   processor. The features are defined within the tune files and allow
   arguments (i.e. ``TUNE_*ARGS``) to be dynamically generated based on
   the features.

   The OpenEmbedded build system verifies the features to be sure they
   are not conflicting and that they are supported.

   The BitBake configuration file (``meta/conf/bitbake.conf``) defines
   ``TUNE_FEATURES`` as follows: TUNE_FEATURES ??=
   "${TUNE_FEATURES_tune-${DEFAULTTUNE}}" See the
   ```DEFAULTTUNE`` <#var-DEFAULTTUNE>`__ variable for more information.

TUNE_PKGARCH
   The package architecture understood by the packaging system to define
   the architecture, ABI, and tuning of output packages. The specific
   tune is defined using the "_tune" override as follows:
   TUNE_PKGARCH_tune-tune = "tune"

   These tune-specific package architectures are defined in the machine
   include files. Here is an example of the "core2-32" tuning as used in
   the ``meta/conf/machine/include/tune-core2.inc`` file:
   TUNE_PKGARCH_tune-core2-32 = "core2-32"

TUNEABI
   An underlying Application Binary Interface (ABI) used by a particular
   tuning in a given toolchain layer. Providers that use prebuilt
   libraries can use the ``TUNEABI``,
   ```TUNEABI_OVERRIDE`` <#var-TUNEABI_OVERRIDE>`__, and
   ```TUNEABI_WHITELIST`` <#var-TUNEABI_WHITELIST>`__ variables to check
   compatibility of tunings against their selection of libraries.

   If ``TUNEABI`` is undefined, then every tuning is allowed. See the
   ```sanity`` <#ref-classes-sanity>`__ class to see how the variable is
   used.

TUNEABI_OVERRIDE
   If set, the OpenEmbedded system ignores the
   ```TUNEABI_WHITELIST`` <#var-TUNEABI_WHITELIST>`__ variable.
   Providers that use prebuilt libraries can use the
   ``TUNEABI_OVERRIDE``, ``TUNEABI_WHITELIST``, and
   ```TUNEABI`` <#var-TUNEABI>`__ variables to check compatibility of a
   tuning against their selection of libraries.

   See the ```sanity`` <#ref-classes-sanity>`__ class to see how the
   variable is used.

TUNEABI_WHITELIST
   A whitelist of permissible ```TUNEABI`` <#var-TUNEABI>`__ values. If
   ``TUNEABI_WHITELIST`` is not set, all tunes are allowed. Providers
   that use prebuilt libraries can use the ``TUNEABI_WHITELIST``,
   ```TUNEABI_OVERRIDE`` <#var-TUNEABI_OVERRIDE>`__, and ``TUNEABI``
   variables to check compatibility of a tuning against their selection
   of libraries.

   See the ```sanity`` <#ref-classes-sanity>`__ class to see how the
   variable is used.

TUNECONFLICTS[feature]
   Specifies CPU or Application Binary Interface (ABI) tuning features
   that conflict with feature.

   Known tuning conflicts are specified in the machine include files in
   the `Source Directory <#source-directory>`__. Here is an example from
   the ``meta/conf/machine/include/mips/arch-mips.inc`` include file
   that lists the "o32" and "n64" features as conflicting with the "n32"
   feature: TUNECONFLICTS[n32] = "o32 n64"

TUNEVALID[feature]
   Specifies a valid CPU or Application Binary Interface (ABI) tuning
   feature. The specified feature is stored as a flag. Valid features
   are specified in the machine include files (e.g.
   ``meta/conf/machine/include/arm/arch-arm.inc``). Here is an example
   from that file: TUNEVALID[bigendian] = "Enable big-endian mode."

   See the machine include files in the `Source
   Directory <#source-directory>`__ for these features.

UBOOT_CONFIG
   Configures the ```UBOOT_MACHINE`` <#var-UBOOT_MACHINE>`__ and can
   also define ```IMAGE_FSTYPES`` <#var-IMAGE_FSTYPES>`__ for individual
   cases.

   Following is an example from the ``meta-fsl-arm`` layer. UBOOT_CONFIG
   ??= "sd" UBOOT_CONFIG[sd] = "mx6qsabreauto_config,sdcard"
   UBOOT_CONFIG[eimnor] = "mx6qsabreauto_eimnor_config"
   UBOOT_CONFIG[nand] = "mx6qsabreauto_nand_config,ubifs"
   UBOOT_CONFIG[spinor] = "mx6qsabreauto_spinor_config" In this example,
   "sd" is selected as the configuration of the possible four for the
   ``UBOOT_MACHINE``. The "sd" configuration defines
   "mx6qsabreauto_config" as the value for ``UBOOT_MACHINE``, while the
   "sdcard" specifies the ``IMAGE_FSTYPES`` to use for the U-boot image.

   For more information on how the ``UBOOT_CONFIG`` is handled, see the
   ```uboot-config`` <http://git.yoctoproject.org/cgit/cgit.cgi/poky/tree/meta/classes/uboot-config.bbclass>`__
   class.

UBOOT_ENTRYPOINT
   Specifies the entry point for the U-Boot image. During U-Boot image
   creation, the ``UBOOT_ENTRYPOINT`` variable is passed as a
   command-line parameter to the ``uboot-mkimage`` utility.

UBOOT_LOADADDRESS
   Specifies the load address for the U-Boot image. During U-Boot image
   creation, the ``UBOOT_LOADADDRESS`` variable is passed as a
   command-line parameter to the ``uboot-mkimage`` utility.

UBOOT_LOCALVERSION
   Appends a string to the name of the local version of the U-Boot
   image. For example, assuming the version of the U-Boot image built
   was "2013.10", the full version string reported by U-Boot would be
   "2013.10-yocto" given the following statement: UBOOT_LOCALVERSION =
   "-yocto"

UBOOT_MACHINE
   Specifies the value passed on the ``make`` command line when building
   a U-Boot image. The value indicates the target platform
   configuration. You typically set this variable from the machine
   configuration file (i.e. ``conf/machine/machine_name.conf``).

   Please see the "Selection of Processor Architecture and Board Type"
   section in the U-Boot README for valid values for this variable.

UBOOT_MAKE_TARGET
   Specifies the target called in the ``Makefile``. The default target
   is "all".

UBOOT_SUFFIX
   Points to the generated U-Boot extension. For example, ``u-boot.sb``
   has a ``.sb`` extension.

   The default U-Boot extension is ``.bin``

UBOOT_TARGET
   Specifies the target used for building U-Boot. The target is passed
   directly as part of the "make" command (e.g. SPL and AIS). If you do
   not specifically set this variable, the OpenEmbedded build process
   passes and uses "all" for the target during the U-Boot building
   process.

UNKNOWN_CONFIGURE_WHITELIST
   Specifies a list of options that, if reported by the configure script
   as being invalid, should not generate a warning during the
   ```do_configure`` <#ref-tasks-configure>`__ task. Normally, invalid
   configure options are simply not passed to the configure script (e.g.
   should be removed from ```EXTRA_OECONF`` <#var-EXTRA_OECONF>`__ or
   ```PACKAGECONFIG_CONFARGS`` <#var-PACKAGECONFIG_CONFARGS>`__).
   However, common options, for example, exist that are passed to all
   configure scripts at a class level that might not be valid for some
   configure scripts. It follows that no benefit exists in seeing a
   warning about these options. For these cases, the options are added
   to ``UNKNOWN_CONFIGURE_WHITELIST``.

   The configure arguments check that uses
   ``UNKNOWN_CONFIGURE_WHITELIST`` is part of the
   ```insane`` <#ref-classes-insane>`__ class and is only enabled if the
   recipe inherits the ```autotools`` <#ref-classes-autotools>`__ class.

UPDATERCPN
   For recipes inheriting the
   ```update-rc.d`` <#ref-classes-update-rc.d>`__ class, ``UPDATERCPN``
   specifies the package that contains the initscript that is enabled.

   The default value is "${PN}". Given that almost all recipes that
   install initscripts package them in the main package for the recipe,
   you rarely need to set this variable in individual recipes.

UPSTREAM_CHECK_GITTAGREGEX
   You can perform a per-recipe check for what the latest upstream
   source code version is by calling ``bitbake -c checkpkg`` recipe. If
   the recipe source code is provided from Git repositories, the
   OpenEmbedded build system determines the latest upstream version by
   picking the latest tag from the list of all repository tags.

   You can use the ``UPSTREAM_CHECK_GITTAGREGEX`` variable to provide a
   regular expression to filter only the relevant tags should the
   default filter not work correctly. UPSTREAM_CHECK_GITTAGREGEX =
   "git_tag_regex"

UPSTREAM_CHECK_REGEX
   Use the ``UPSTREAM_CHECK_REGEX`` variable to specify a different
   regular expression instead of the default one when the package
   checking system is parsing the page found using
   ```UPSTREAM_CHECK_URI`` <#var-UPSTREAM_CHECK_URI>`__.
   UPSTREAM_CHECK_REGEX = "package_regex"

UPSTREAM_CHECK_URI
   You can perform a per-recipe check for what the latest upstream
   source code version is by calling ``bitbake -c checkpkg`` recipe. If
   the source code is provided from tarballs, the latest version is
   determined by fetching the directory listing where the tarball is and
   attempting to find a later tarball. When this approach does not work,
   you can use ``UPSTREAM_CHECK_URI`` to provide a different URI that
   contains the link to the latest tarball. UPSTREAM_CHECK_URI =
   "recipe_url"

USE_DEVFS
   Determines if ``devtmpfs`` is used for ``/dev`` population. The
   default value used for ``USE_DEVFS`` is "1" when no value is
   specifically set. Typically, you would set ``USE_DEVFS`` to "0" for a
   statically populated ``/dev`` directory.

   See the "`Selecting a Device
   Manager <&YOCTO_DOCS_DEV_URL;#selecting-dev-manager>`__" section in
   the Yocto Project Development Tasks Manual for information on how to
   use this variable.

USE_VT
   When using
   `SysVinit <&YOCTO_DOCS_DEV_URL;#new-recipe-enabling-system-services>`__,
   determines whether or not to run a
   `getty <http://en.wikipedia.org/wiki/Getty_%28Unix%29>`__ on any
   virtual terminals in order to enable logging in through those
   terminals.

   The default value used for ``USE_VT`` is "1" when no default value is
   specifically set. Typically, you would set ``USE_VT`` to "0" in the
   machine configuration file for machines that do not have a graphical
   display attached and therefore do not need virtual terminal
   functionality.

USER_CLASSES
   A list of classes to globally inherit. These classes are used by the
   OpenEmbedded build system to enable extra features (e.g.
   ``buildstats``, ``image-mklibs``, and so forth).

   The default list is set in your ``local.conf`` file: USER_CLASSES ?=
   "buildstats image-mklibs image-prelink" For more information, see
   ``meta-poky/conf/local.conf.sample`` in the `Source
   Directory <#source-directory>`__.

USERADD_ERROR_DYNAMIC
   If set to ``error``, forces the OpenEmbedded build system to produce
   an error if the user identification (``uid``) and group
   identification (``gid``) values are not defined in any of the files
   listed in ```USERADD_UID_TABLES`` <#var-USERADD_UID_TABLES>`__ and
   ```USERADD_GID_TABLES`` <#var-USERADD_GID_TABLES>`__. If set to
   ``warn``, a warning will be issued instead.

   The default behavior for the build system is to dynamically apply
   ``uid`` and ``gid`` values. Consequently, the
   ``USERADD_ERROR_DYNAMIC`` variable is by default not set. If you plan
   on using statically assigned ``gid`` and ``uid`` values, you should
   set the ``USERADD_ERROR_DYNAMIC`` variable in your ``local.conf``
   file as follows: USERADD_ERROR_DYNAMIC = "error" Overriding the
   default behavior implies you are going to also take steps to set
   static ``uid`` and ``gid`` values through use of the
   ```USERADDEXTENSION`` <#var-USERADDEXTENSION>`__,
   ```USERADD_UID_TABLES`` <#var-USERADD_UID_TABLES>`__, and
   ```USERADD_GID_TABLES`` <#var-USERADD_GID_TABLES>`__ variables.

      **Note**

      There is a difference in behavior between setting
      USERADD_ERROR_DYNAMIC
      to
      error
      and setting it to
      warn
      . When it is set to
      warn
      , the build system will report a warning for every undefined
      uid
      and
      gid
      in any recipe. But when it is set to
      error
      , it will only report errors for recipes that are actually built.
      This saves you from having to add static IDs for recipes that you
      know will never be built.

USERADD_GID_TABLES
   Specifies a password file to use for obtaining static group
   identification (``gid``) values when the OpenEmbedded build system
   adds a group to the system during package installation.

   When applying static group identification (``gid``) values, the
   OpenEmbedded build system looks in ```BBPATH`` <#var-BBPATH>`__ for a
   ``files/group`` file and then applies those ``uid`` values. Set the
   variable as follows in your ``local.conf`` file: USERADD_GID_TABLES =
   "files/group"

      **Note**

      Setting the
      USERADDEXTENSION
      variable to "useradd-staticids" causes the build system to use
      static
      gid
      values.

USERADD_PACKAGES
   When inheriting the ```useradd`` <#ref-classes-useradd>`__ class,
   this variable specifies the individual packages within the recipe
   that require users and/or groups to be added.

   You must set this variable if the recipe inherits the class. For
   example, the following enables adding a user for the main package in
   a recipe: USERADD_PACKAGES = "${PN}"

      **Note**

      It follows that if you are going to use the
      USERADD_PACKAGES
      variable, you need to set one or more of the
      USERADD_PARAM
      ,
      GROUPADD_PARAM
      , or
      GROUPMEMS_PARAM
      variables.

USERADD_PARAM
   When inheriting the ```useradd`` <#ref-classes-useradd>`__ class,
   this variable specifies for a package what parameters should pass to
   the ``useradd`` command if you add a user to the system when the
   package is installed.

   Here is an example from the ``dbus`` recipe: USERADD_PARAM_${PN} =
   "--system --home ${localstatedir}/lib/dbus \\ --no-create-home
   --shell /bin/false \\ --user-group messagebus" For information on the
   standard Linux shell command ``useradd``, see
   ` <http://linux.die.net/man/8/useradd>`__.

USERADD_UID_TABLES
   Specifies a password file to use for obtaining static user
   identification (``uid``) values when the OpenEmbedded build system
   adds a user to the system during package installation.

   When applying static user identification (``uid``) values, the
   OpenEmbedded build system looks in ```BBPATH`` <#var-BBPATH>`__ for a
   ``files/passwd`` file and then applies those ``uid`` values. Set the
   variable as follows in your ``local.conf`` file: USERADD_UID_TABLES =
   "files/passwd"

      **Note**

      Setting the
      USERADDEXTENSION
      variable to "useradd-staticids" causes the build system to use
      static
      uid
      values.

USERADDEXTENSION
   When set to "useradd-staticids", causes the OpenEmbedded build system
   to base all user and group additions on a static ``passwd`` and
   ``group`` files found in ```BBPATH`` <#var-BBPATH>`__.

   To use static user identification (``uid``) and group identification
   (``gid``) values, set the variable as follows in your ``local.conf``
   file: USERADDEXTENSION = "useradd-staticids"

      **Note**

      Setting this variable to use static
      uid
      and
      gid
      values causes the OpenEmbedded build system to employ the
      useradd-staticids
      class.

   If you use static ``uid`` and ``gid`` information, you must also
   specify the ``files/passwd`` and ``files/group`` files by setting the
   ```USERADD_UID_TABLES`` <#var-USERADD_UID_TABLES>`__ and
   ```USERADD_GID_TABLES`` <#var-USERADD_GID_TABLES>`__ variables.
   Additionally, you should also set the
   ```USERADD_ERROR_DYNAMIC`` <#var-USERADD_ERROR_DYNAMIC>`__ variable.

VOLATILE_LOG_DIR
   Specifies the persistence of the target's ``/var/log`` directory,
   which is used to house postinstall target log files.

   By default, ``VOLATILE_LOG_DIR`` is set to "yes", which means the
   file is not persistent. You can override this setting by setting the
   variable to "no" to make the log directory persistent.

WARN_QA
   Specifies the quality assurance checks whose failures are reported as
   warnings by the OpenEmbedded build system. You set this variable in
   your distribution configuration file. For a list of the checks you
   can control with this variable, see the
   "```insane.bbclass`` <#ref-classes-insane>`__" section.

WKS_FILE_DEPENDS
   When placed in the recipe that builds your image, this variable lists
   build-time dependencies. The ``WKS_FILE_DEPENDS`` variable is only
   applicable when Wic images are active (i.e. when
   ```IMAGE_FSTYPES`` <#var-IMAGE_FSTYPES>`__ contains entries related
   to Wic). If your recipe does not create Wic images, the variable has
   no effect.

   The ``WKS_FILE_DEPENDS`` variable is similar to the
   ```DEPENDS`` <#var-DEPENDS>`__ variable. When you use the variable in
   your recipe that builds the Wic image, dependencies you list in the
   ``WIC_FILE_DEPENDS`` variable are added to the ``DEPENDS`` variable.

   With the ``WKS_FILE_DEPENDS`` variable, you have the possibility to
   specify a list of additional dependencies (e.g. native tools,
   bootloaders, and so forth), that are required to build Wic images.
   Following is an example: WKS_FILE_DEPENDS = "some-native-tool" In the
   previous example, some-native-tool would be replaced with an actual
   native tool on which the build would depend.

WKS_FILE
   Specifies the location of the Wic kickstart file that is used by the
   OpenEmbedded build system to create a partitioned image
   (image``.wic``). For information on how to create a partitioned
   image, see the "`Creating Partitioned Images Using
   Wic <&YOCTO_DOCS_DEV_URL;#creating-partitioned-images-using-wic>`__"
   section in the Yocto Project Development Tasks Manual. For details on
   the kickstart file format, see the "`OpenEmbedded Kickstart
   (``.wks``) Reference <#ref-kickstart>`__" Chapter.

WORKDIR
   The pathname of the work directory in which the OpenEmbedded build
   system builds a recipe. This directory is located within the
   ```TMPDIR`` <#var-TMPDIR>`__ directory structure and is specific to
   the recipe being built and the system for which it is being built.

   The ``WORKDIR`` directory is defined as follows:
   ${TMPDIR}/work/${MULTIMACH_TARGET_SYS}/${PN}/${EXTENDPE}${PV}-${PR}
   The actual directory depends on several things:

   -  TMPDIR
      : The top-level build output directory
   -  MULTIMACH_TARGET_SYS
      : The target system identifier
   -  PN
      : The recipe name
   -  EXTENDPE
      : The epoch - (if
      PE
      is not specified, which is usually the case for most recipes, then
      EXTENDPE
      is blank)
   -  PV
      : The recipe version
   -  PR
      : The recipe revision

   As an example, assume a Source Directory top-level folder name
   ``poky``, a default Build Directory at ``poky/build``, and a
   ``qemux86-poky-linux`` machine target system. Furthermore, suppose
   your recipe is named ``foo_1.3.0-r0.bb``. In this case, the work
   directory the build system uses to build the package would be as
   follows: poky/build/tmp/work/qemux86-poky-linux/foo/1.3.0-r0

XSERVER
   Specifies the packages that should be installed to provide an X
   server and drivers for the current machine, assuming your image
   directly includes ``packagegroup-core-x11-xserver`` or, perhaps
   indirectly, includes "x11-base" in
   ```IMAGE_FEATURES`` <#var-IMAGE_FEATURES>`__.

   The default value of ``XSERVER``, if not specified in the machine
   configuration, is "xserver-xorg xf86-video-fbdev xf86-input-evdev".
