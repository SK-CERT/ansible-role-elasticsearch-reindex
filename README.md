Elasticsearch-reindex
=========

Abstraction for Elasticsearch reindex API.

Possible use cases: 
- Migrate or rename indices between remote elasticsearch instances or on single host. (may be asynchronous)
- Update indices' with changed templates.

Requirements
------------

Reindexing between remote hosts requires additional configuration on the destination host. Take a peek at `https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-remote.html`.

Add this `reindex.remote.whitelist: <your_elasticsearch_host>:9200` to you 'elasticserch.yml' configuration file. 

Role Variables
--------------

    suffix: <str, temporary index suffix>
    keep: <boolean, whether the temporary indices are preserved>
    timeout: <int, reindex request timeout in seconds defaults to 1800 (1/2 hour)>
    async: <boolean, if true, the reindex requests are asynchronous>
    validate_certs <boolean, URI module validate SSL certificates option>
    source:
        host: <str, hostname>
        indices: <list of index names>
    destination:
        host: <str, hostname>
        indices: <list of index names for renaming purposes>

Dependencies
------------

None

Example Playbook
----------------

Playbook for migrating indices:

    - hosts: localhost
      roles:
        - role: elasticsearch-reindex
          vars:
            source:
              host: "elasticsearch.server.sk:9200"
              indices:
                - test-2019
                - test-2018
                - test-2017
            destination:
              host: "elasticsearch.server.sk:9201"

Playbook for updating indices:

    - hosts: localhost
      roles:
        - role: elasticsearch-reindex
          vars:
            keep: yes
            suffix: "-backup"
            source:
              host: "elasticsearch.server.sk:9200"
              indices:
                - test-2019
                - test-2018
                - test-2017

Playbook for renaming & migrating indices:

    - hosts: localhost
      roles:
        - role: elasticsearch-reindex
          vars:
            async: true
            source:
              host: "elasticsearch.server.sk:9200"
              indices:
                - test-2019
                - test-2018
                - test-2017
            destination:
              host: "elasticsearch2.server.sk:9200"
              indices:
                - test-renamed-2019
                - test-renamed-2018
                - test-renamed-2017
License
-------

MIT

Author Information
------------------

Tomas Bellus
