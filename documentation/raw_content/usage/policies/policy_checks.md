## Anchore Policies Checks

In this document, we describe the current anchore gates (and related triggers/parameters) that are supported within anchore policy bundles.  If you have a running anchore engine, this information can also be obtained using the CLI:

`# anchore-cli policy describe (--gate <gatename> ( --trigger <triggername))`

### Gate: secret_scans

Checks for secrets found in the image using configured regexes found in the "secret_search" section of analyzer_config.yaml.

| Trigger Name | Description | Parameter | Description | Example |
| :----------- | :---------- | :-------- | :---------- | :------ |
| content_regex_checks | Triggers if the content search analyzer has found any matches with the configured and named regexes. Matches are filtered by the content_regex_name and filename_regex if they are set. The content_regex_name shoud be a value from the "secret_search" section of the analyzer_config.yaml. | content_regex_name | Name of content regexps configured in the analyzer that should trigger if found in the image, instead of triggering for any match. Names available by default are: ['AWS_ACCESS_KEY', 'AWS_SECRET_KEY', 'PRIV_KEY', 'DOCKER_AUTH', 'API_KEY']. | AWS_ACCESS_KEY |
| content_regex_checks | Triggers if the content search analyzer has found any matches with the configured and named regexes. Matches are filtered by the content_regex_name and filename_regex if they are set. The content_regex_name shoud be a value from the "secret_search" section of the analyzer_config.yaml. | filename_regex | Regexp to filter the content matched files by. | /etc/.* |