---
layout: default
title: OAM Management
id: oam-m
---

{% include blocks/h1.html title="OAM Management" %}

OAM Management is a OAM component which provides tools, utilities and helpers for managing OAM components. In a nutshell, OAM-M simplifies interaction with OAM APIs.

{% include blocks/h2.html title="OAM-C Management" %}

* `query_catalog` --- executes a query using the local OAM-C Index
* `update_index` --- fetches changes from the Master OAM-C Node and updates local OAM-C Index
* `start_browser` --- starts local version of the OAM-C Browser
* `dataset_info` --- shows information about a dataset
* `pull_dataset` --- pulls dataset from a remote OAM Node
* `push_dataset` --- push local dataset to a remote OAM Node
* 