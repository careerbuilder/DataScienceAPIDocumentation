Data Science API Versioning
================

Contents
----------
- [Overview](#overview)
- [Contract Versioning](#contract-versioning)
- [Data Versioning](#data-versioning)
- [Errors and HTTP Status Codes](#errors-and-http-status-codes)
- [Alpha and Beta Versions](#alpha-and-beta-versions)
- [Sunsetting Versions](#sunsetting-versions)
- [Version Specification](#version-specification)


# Overview
This document attempts to explain the Data Science approach to versioning our APIs and data.  This is intended as a general document and anything can be superceded by specific API documentation.  Each API has a Version section in its documentation that lists any variances with this document. In general, large changes result in a new major version and small changes result in a new minor version.

# Contract Versioning
Our APIs contracts are versioned.  No breaking changes (field removal or renaming) will be made to either the input or the response without versioning.  Field additions are not versioned.  Ordering of fields and items within lists is unversioned and may change without warning unless noted in the API's documentation. Small changes will be given minor versions; large changes will be given major versions.  Even small changes may be breaking changes (such as renaming a field).

# Data Versioning
The data returned from our APIs may or may not be versioned.  This will be indicated in each API's documentation.  If an API's data is versioned it means that the same input and version will always yield the same output. If an API's data is not versioned there will be a discussion in the API documentation about what can change and the expected reasons and rate of change. In APIs that have a set taxonomy for outputs, all versions' taxonomies will be listed in the documentation.  Any additions or deletions from a set taxonomy will result in a new major version.  Other changes to the output such as reranking or renaming may result in a minor version. Note that a data versioned API may add new fields to the output or new optional inputs without a new version. Fixing a deterministic bug in a data versioned API will be done in a new version.

# Errors and HTTP Status Codes
The contract of how errors are returned is versioned. Several APIs return errors in non-standard ways; we are working towards standardizing them which will result in new versions. The HTTP Status Codes and contents of errors such as messages and error numbers is unversioned and may change without notice.

# Alpha and Beta Versions
Occasionally we may release new versions of APIs in an Alpha or Beta state.  Availability of Alpha or Beta versions may not be widely communicated; we may let a single customer test an API. Alpha and Beta APIs may be changed without notification, including breaking changes, and consumers should code appropriately. In addition, Alpha APIs are not yet ready for production scale traffic and should not be hit at scale.

# Sunsetting Versions
Old versions of APIs will be sunset as appropriate.  All consumers of APIs are committing to do the work necessary to move from sunset APIs to newer versions. Any API sunset dates will be communicated with all users of the API.  Our goal is to announce sunset dates at least 6 weeks before the sunset date to allow consumers time to react, though tighter timelines may be required in exceptional situations. For data versioned APIs our intention is to support two major versions at all times, and to announce the sunset of the third newest major version upon the release of the newest major version. For example, when Version 5.0 is released, Versions 3.x sunset date is announced.

# Version Specification
Per CB standard, version is passed in through the Accept header, and if no version is specified the newest version is used. Defaulting to the latest version creates the possibility of unexpected failure in your application due to breaking changes resulting from version upgrades. API consumers are strongly encouraged to specify a version when interacting with APIs. 
