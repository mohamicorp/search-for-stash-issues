# Search for Bitbucket

**Search for Bitbucket** is a code, file and commit search service for Atlassian's Stash. It's powered by [Elasticsearch](https://www.elastic.co/products/elasticsearch), which can be configured to be an external or internal Bitbucket service.

**Table of Contents**

- [Installation Requirements](#installation-requirements)
- [Installing](#installing)
- [Features](#features)
- [Administration](#administration)
- [Repository Settings](#repository-settings)
- [Contact Us](#contact-us)


----------


### Installation Requirements
**Search for Bitbucket** requires that your server use git 1.8.5 or higher.

**Search for Bitbucket** is powered by Elasticsearch. Either an internal or external node can be used. This node handles all search requests and all repository indexing. 

####Internal Node
By default, Search for Bitbucket spawns an internal elasticsearch node. The initial indexing of large repositories can potentially require a large portion of RAM. It is **recommended to allocate Bitbucket 6GB of RAM** for optimal performance. For instructions on changing the amount of memory available to Bitbucket, take a look at Altassian's documentation [here](https://confluence.atlassian.com/display/STASH/Scaling+Stash).

As an example benchmark, **Search for Bitbucket** indexed the Linux code base which is approximately 15 million lines of code and half a million commits in 2-3 minutes with **6 GB** of ram on non SSD harddives with an internal node. That said, it is **recommended** that for large codebases you use an external Elasticsearch node. For any concerns or questions, feel free to [contact us](mailto:mohammed@mohamicorp.com).

####External Node
For Bitbucket instances with large codebases and a lot of indexing, it's recommended to setup a separate Elasticsearch service. This will reduce the strain on Bitbucket for indexing, and should significantly improve performance.



----------


### Installing

####Internal Node
After installing the plugin in your bitbucket instance, you must enable indexing and trigger a reindex:

 1. Go to `Search for Bitbucket Global Settings` page in the Bitbucket admin panel.
 2. Enable `Indexing` by clicking the check box.
 3. Click `Save and Reindex` to save the settings and subsequently reindex all repositories.

####External Node
You must first setup an external Elasticsearch cluster. For help on that, look [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-configuration.html). **Search for Bitbucket** will listen for a transport on `localhost:9300` with the cluster name `stash-codesearch`.

For testing purposes, we've provided some scripts for installing and running an elasticsearch instance. You can obtain an instance of Elasticsearch by running the provided `bin/install-elasticsearch-instance.sh` script. To run an Elasticsearch instance, run `bin/invoke-es.sh`. Make sure that `elasticearch.yml` is in the directory you are invoking Elasticsearch in so that the Elasticsearch configuration is picked up.

Once the node is setup, you must configure **Search for Bitbucket**: 

 1. Go to `Search for Bitbucket Global Settings` page in the Bitbucket admin panel.
 2. Enable `Indexing` by clicking the check box.
 3. Uncheck the `Internal ES Node` checkbox.
 3. Click `Save and Reindex` to save the settings and subsequently reindex all repositories.
 
**Search for Bitbucket** will then start indexing all of your bitbucket repositories in the background. It will take a few seconds to a few minutes to finish depending on the number and size of your repositories. For large Bitbucket instances, it is recommend indexing to be done during non peak hours.

By default, only the master and develop branches are indexed. Individual repo admins may modify these settings. See [below](#repository-settings) for instructions.


----------


### Features
Since **Search for Bitbucket** is powered by Elasticsearch. It utilizes Elasticsearch's powerful [Query String Syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax). This includes support for wildcards, regular expressions, fuzziness, and much more.

There are also many advanced filtering options. Here is a brief summary:

| Option | Description |
| ------------- | ------------- |
| Search code, filenames and commits | Filter results to only code, commits, or filenames | 
| Project keys | Filter results to specific projects |
| Repository slugs | Filter results to certain [slugs](https://confluence.atlassian.com/display/BITBUCKET/What+is+a+Slug). | 
| Ref names | Filter results to certain refs|
| File extensions | Filter results to certain file extentions |
| Author/editor names or emails | Filter results to a specific author |
| Committed on or after/before (Commits only) | Filter results to a certain date range.|


----------


### Administration
Search for Bitbucket has advanced configuration settings. These settings should only be adjusted by advanced users.

| Setting  | Description |
| ------------- | ------------- |
| Internal ES Node | Check this box to use an internal elasticsearch service. |
| Indexing  | Check this box to enable indexing of new source code & commits |
| Indexing Concurrency Limit  | Maximum number of concurrent indexing jobs|
| Max Filesize| Maximum size (in bytes) of source code files to index
| Search Timeout| Timeout (in ms) of all search requests
| Unhighlighted Extensions | Comma-separated list of file extensions to exclude from syntax highlighting
| Preview Limit | Maximum number of lines to display for file previews
| Match Limit | Maximum number of lines to display for file matches
| Fragment Limit | Maximum number of match fragments to display for file matches
| Page Size | Number of results to display per page
| Commit Hash Boost | Boosting factor of results with matching commit hashes (relative to source code matches)
| Commit Subject Boost | Boosting factor of results with matching commit subjects
| Commit Body Boost | Boosting factor of results with matching commit message bodies
| Filename Boost | Boosting factor of results with matching file names


----------


### Repository Settings

By default, only master and develop are indexed. Individual repo admins may modify these settings as follows:

- Go to the `Search for Bitbucket Repository Settings` page in your repository settings panel.
- Change the ref regex to match your desired branches.
- Click `Save` or  `Save and Reindex` to save the settings and subsequently reindex all repositories.

For example if we wanted to add the branch `my-branch`, we would modify the regex to look like :

    HEAD|(refs/heads/(master|develop|my-branch))


----------


### Contact Us

If you want to get help, request a feature or report a bug, please email us at  [contact us](mailto:mohamicorp@gmail.com) or open an issue. **We'd love to hear from you!**


