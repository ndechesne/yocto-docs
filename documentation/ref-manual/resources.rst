.. _resources:

****************************************
Contributions and Additional Information
****************************************

.. _resources-intro:

Introduction
============

The Yocto Project team is happy for people to experiment with the Yocto
Project. A number of places exist to find help if you run into
difficulties or find bugs. This presents information about contributing
and participating in the Yocto Project.

.. _resources-contributions:

Contributions
=============

The Yocto Project gladly accepts contributions. You can submit changes
to the project either by creating and sending pull requests, or by
submitting patches through email. For information on how to do both as
well as information on how to identify the maintainer for each area of
code, see the "`Submitting a Change to the Yocto
Project <&YOCTO_DOCS_DEV_URL;#how-to-submit-a-change>`__" section in the
Yocto Project Development Tasks Manual.

.. _resources-bugtracker:

Yocto Project Bugzilla
======================

The Yocto Project uses its own implementation of
`Bugzilla <&YOCTO_BUGZILLA_URL;>`__ to track defects (bugs).
Implementations of Bugzilla work well for group development because they
track bugs and code changes, can be used to communicate changes and
problems with developers, can be used to submit and review patches, and
can be used to manage quality assurance.

Sometimes it is helpful to submit, investigate, or track a bug against
the Yocto Project itself (e.g. when discovering an issue with some
component of the build system that acts contrary to the documentation or
your expectations).

A general procedure and guidelines exist for when you use Bugzilla to
submit a bug. For information on how to use Bugzilla to submit a bug
against the Yocto Project, see the following:

-  The "`Submitting a Defect Against the Yocto
   Project <&YOCTO_DOCS_DEV_URL;#submitting-a-defect-against-the-yocto-project>`__"
   section in the Yocto Project Development Tasks Manual.

-  The Yocto Project `Bugzilla wiki
   page <&YOCTO_WIKI_URL;/wiki/Bugzilla_Configuration_and_Bug_Tracking>`__

For information on Bugzilla in general, see
` <http://www.bugzilla.org/about/>`__.

.. _resources-mailinglist:

Mailing lists
=============

A number of mailing lists maintained by the Yocto Project exist as well
as related OpenEmbedded mailing lists for discussion, patch submission
and announcements. To subscribe to one of the following mailing lists,
click on the appropriate URL in the following list and follow the
instructions:

-  ` <&YOCTO_LISTS_URL;/listinfo/yocto>`__ - General Yocto Project
   discussion mailing list.

-  ` <&OE_LISTS_URL;/listinfo/openembedded-core>`__ - Discussion mailing
   list about OpenEmbedded-Core (the core metadata).

-  ` <&OE_LISTS_URL;/listinfo/openembedded-devel>`__ - Discussion
   mailing list about OpenEmbedded.

-  ` <&OE_LISTS_URL;/listinfo/bitbake-devel>`__ - Discussion mailing
   list about the `BitBake <#bitbake-term>`__ build tool.

-  ` <&YOCTO_LISTS_URL;/listinfo/poky>`__ - Discussion mailing list
   about `Poky <#poky>`__.

-  ` <&YOCTO_LISTS_URL;/listinfo/yocto-announce>`__ - Mailing list to
   receive official Yocto Project release and milestone announcements.

For more Yocto Project-related mailing lists, see the
Yocto Project Website
.
.. _resources-irc:

Internet Relay Chat (IRC)
=========================

Two IRC channels on freenode are available for the Yocto Project and
Poky discussions:

-  ``#yocto``

-  ``#poky``

.. _resources-links-and-related-documentation:

Links and Related Documentation
===============================

Here is a list of resources you might find helpful:

-  `The Yocto Project website <&YOCTO_HOME_URL;>`__\ *:* The home site
   for the Yocto Project.

-  `The Yocto Project Main Wiki
   Page <&YOCTO_WIKI_URL;/wiki/Main_Page>`__\ *:* The main wiki page for
   the Yocto Project. This page contains information about project
   planning, release engineering, QA & automation, a reference site map,
   and other resources related to the Yocto Project.

-  `OpenEmbedded <&OE_HOME_URL;>`__\ *:* The build system used by the
   Yocto Project. This project is the upstream, generic, embedded
   distribution from which the Yocto Project derives its build system
   (Poky) and to which it contributes.

-  `BitBake <http://www.openembedded.org/wiki/BitBake>`__\ *:* The tool
   used to process metadata.

-  `BitBake User Manual <&YOCTO_DOCS_BB_URL;>`__\ *:* A comprehensive
   guide to the BitBake tool. If you want information on BitBake, see
   this manual.

-  `Yocto Project Quick Build <&YOCTO_DOCS_BRIEF_URL;>`__\ *:* This
   short document lets you experience building an image using the Yocto
   Project without having to understand any concepts or details.

-  `Yocto Project Overview and Concepts
   Manual <&YOCTO_DOCS_OM_URL;>`__\ *:* This manual provides overview
   and conceptual information about the Yocto Project.

-  `Yocto Project Development Tasks
   Manual <&YOCTO_DOCS_DEV_URL;>`__\ *:* This manual is a "how-to" guide
   that presents procedures useful to both application and system
   developers who use the Yocto Project.

-  `Yocto Project Application Development and the Extensible Software
   Development Kit (eSDK) <&YOCTO_DOCS_SDK_URL;>`__\ *manual:* This
   guide provides information that lets you get going with the standard
   or extensible SDK. An SDK, with its cross-development toolchains,
   allows you to develop projects inside or outside of the Yocto Project
   environment.

-  `Yocto Project Board Support Package (BSP) Developer's
   Guide <&YOCTO_DOCS_BSP_URL;>`__\ *:* This guide defines the structure
   for BSP components. Having a commonly understood structure encourages
   standardization.

-  `Yocto Project Linux Kernel Development
   Manual <&YOCTO_DOCS_KERNEL_DEV_URL;>`__\ *:* This manual describes
   how to work with Linux Yocto kernels as well as provides a bit of
   conceptual information on the construction of the Yocto Linux kernel
   tree.

-  `Yocto Project Reference Manual <&YOCTO_DOCS_REF_URL;>`__\ *:* This
   manual provides reference material such as variable, task, and class
   descriptions.

-  `Yocto Project Mega-Manual <&YOCTO_DOCS_MM_URL;>`__\ *:* This manual
   is simply a single HTML file comprised of the bulk of the Yocto
   Project manuals. The Mega-Manual primarily exists as a vehicle by
   which you can easily search for phrases and terms used in the Yocto
   Project documentation set.

-  `Yocto Project Profiling and Tracing
   Manual <&YOCTO_DOCS_PROF_URL;>`__\ *:* This manual presents a set of
   common and generally useful tracing and profiling schemes along with
   their applications (as appropriate) to each tool.

-  `Toaster User Manual <&YOCTO_DOCS_TOAST_URL;>`__\ *:* This manual
   introduces and describes how to set up and use Toaster. Toaster is an
   Application Programming Interface (API) and web-based interface to
   the `OpenEmbedded Build System <#build-system-term>`__, which uses
   BitBake, that reports build information.

-  `FAQ <&YOCTO_WIKI_URL;/wiki/FAQ>`__\ *:* A list of commonly asked
   questions and their answers.

-  *Release Notes:* Features, updates and known issues for the current
   release of the Yocto Project. To access the Release Notes, go to the
   `Downloads <&YOCTO_HOME_URL;/software-overview/downloads/>`__ page on
   the Yocto Project website and click on the "RELEASE INFORMATION" link
   for the appropriate release.

-  `Bugzilla <&YOCTO_BUGZILLA_URL;>`__\ *:* The bug tracking application
   the Yocto Project uses. If you find problems with the Yocto Project,
   you should report them using this application.

-  `Bugzilla Configuration and Bug Tracking Wiki
   Page <&YOCTO_WIKI_URL;/wiki/Bugzilla_Configuration_and_Bug_Tracking>`__\ *:*
   Information on how to get set up and use the Yocto Project
   implementation of Bugzilla for logging and tracking Yocto Project
   defects.

-  *Internet Relay Chat (IRC):* Two IRC channels on freenode are
   available for Yocto Project and Poky discussions: ``#yocto`` and
   ``#poky``, respectively.

-  `Quick EMUlator (QEMU) <http://wiki.qemu.org/Index.html>`__\ *:* An
   open-source machine emulator and virtualizer.
