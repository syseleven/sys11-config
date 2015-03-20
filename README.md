Description
===========

This repository contains default configuration for Openstack instances.

Structure
=========

Currently, all configuration resides in the puppet/hieradata subdirectory. This
directory contains three *type directories*, grouping configuration by
configuration type: classes.d, config.d and repos.d.

These directories in turn contain *topic directories* (e.g. sensu-server/ for
deploying a sensu server or ssh/ for rolling out SSH keys and configuring the
SSH server). For any given topic there may be a topic subdirectories in any of
the type directories. These topic subdirectories must be named identically.

Type directories contain the following configuration data:

classes.d
---------

This directory's topic directories contain *classes* arrays for Hiera as their
only content. These *classes* arrays list all puppet classes that need to be
invoked to deploy the topic configuration in question. This directory must not
contain any configuration other than *classes* arrays.

config.d
--------

This directory's topic directories contain all configuration relevant to the
topic in question. This directory must not contain *classes* arrays or
puppet-repodeploy configuration. Configuration that is common to multiple
topics should go into a file named common.yaml in one of the topic directories
involved that is symlinked from the other topic directories.

repos.d
-------

This directory's topic directories contain puppet-repodeyloy repository
configuration (i.e. the *repodeploy::repos hash*) specifiying all repositories
required to configure the topic configuration in question. This directory must
not contain any other configuration.


Naming Conventions
==================

This section contains naming conventions for the content of topic directories.

* All configuration files containing a *classes* array should be named
  *classes.yaml*, regardless of which type directory they are located in.

* Configuration files that should appear in Hiera's hierarchy in a given order
  (useful for config files that override each other's variables) should be
  named such that a directory listing shows them in that order. To that end
  they should be prefixed with a number and a dash, e.g. *00-overrides.yaml*
  (files with lower numbers will then appear first in the hierarchy, thus
  overriding those with lower numbers). In particular this applies to
  repository configurations in under repos.d which may need to be overriden
  temporarily until local fixes are merged upstream. To this end all permanent
  repository configuration files should start with '10-' to allow for easy
  override insertion.
