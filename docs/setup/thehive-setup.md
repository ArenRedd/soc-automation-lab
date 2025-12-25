## TheHive Setup

This guide provides a step-by-step guide for setting up TheHive for a local SOC lab.

### Constraints

-   **Deployment**: Local VM deployment
-   **Backend**: OpenSearch backend, Cassandra backend
-   **Focus**: SOC use, not generic installation

### Installation

1.  **Install TheHive**:
    ```
    sudo apt-get install thehive4
    ```
2.  **Configure TheHive**:
    -   Edit the `/etc/thehive/application.conf` file to configure the database and other settings.
    -   For the OpenSearch backend, you will need to provide the URL of your OpenSearch instance.
    -   For the Cassandra backend, you will need to provide the contact points and keyspace for your Cassandra cluster.
3.  **Enable and start TheHive**:
    ```
    sudo systemctl enable thehive
    sudo systemctl start thehive
    ```

### Troubleshooting Notes

-   **Database connection issues**: If you are having trouble connecting to the database, make sure that the database is running and that the connection settings in the `application.conf` file are correct.
-   **Indexing issues**: If you are having trouble with indexing, make sure that the indexer is running and that the indexer settings in the `application.conf` file are correct.
-   **Log files**: TheHive log files are located in the `/var/log/thehive` directory. These logs can be helpful for troubleshooting any issues that you may be having.
