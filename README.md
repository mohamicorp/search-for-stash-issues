# Search for Stash

**Search for Stash** is a code, file and commit search service for Atlassian's Stash. It's powered by an embedded [Elasticsearch](https://www.elastic.co/products/elasticsearch) node.

**Table of Contents**

- [Installation Requirements](#installation-requirements)
- [Installing](#installing)
- [Features](#features)
- [Administration](#administration)
- [Repository Settings](#repository-settings)
- [Contact Us](#contact-us)

### Installation Requirements
**Search for Stash** is powered by an internal Elasticsearch node. This node handles all search requests and all repository indexing. The initial indexing of large repositories can potentially require a large portion of RAM. It is **recommended to allocate  Stash 6GB of RAM** for optimal performance. For instructions look at Altassian's documentation [here](https://confluence.atlassian.com/display/STASH/Scaling+Stash).

As an example benchmark, **Search for Stash** indexed the Linux code base which is approximately 15 million lines of code and half a million commits in 2-3 minutes. That said, it is **recommended** that for large codebases you experiment on a staging environment to test performance. For any concerns or questions, feel free to [contact us](mailto:mohamicorp@gmail.com).
### Installing

After installing the plugin in your stash instance, you must enable indexing and trigger a reindex:

 1. Go to `Search for Stash Global Settings` page in the Stash admin panel.
 2. Enable `Indexing` by clicking the check box.
 3. Click `Save and Reindex` to save the settings and subsequently reindex all repositories.

**Search for Stash** will then start indexing all of your stash repositories in the background. It will take a few seconds to a few minutes to finish depending on the number and size of your repositories. For large Stash instances, it is recommend indexing to be done during non peak hours.

By default, only the master and develop branches are indexed. Individual repo admins may modify these settings. See [below](#repository-settings) for instructions.

### Features
Since **Search for Stash** is powered by Elasticsearch, it utilizes Elasticsearch's powerful [Query String Syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax). This includes support for wildcards, regular expressions, fuzziness, and much more.

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


### Administration
Search for Stash has advanced configuration settings. These settings should only be adjusted by advanced users.

| Setting  | Description |
| ------------- | ------------- |
| Indexing  | Check this box to enable indexing of new source code & commits  |
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


### Repository Settings

By default, only master and develop are indexed. Individual repo admins may modify these settings as follows:

- Go to the `Search for Stash Repository Settings` page in your repository settings panel.
- Change the ref regex to match your desired branches.
- Click `Save` or  `Save and Reindex` to save the settings and subsequently reindex all repositories.

For example if we wanted to add the branch `my-branch`, we would modify the regex to look like :

    HEAD|(refs/heads/(master|develop|my-branch))

### Contact Us

If you want to get help, request a feature or report a bug, please email us at  [contact us](mailto:mohamicorp@gmail.com) or open an issue. **We'd love to hear from you!**


