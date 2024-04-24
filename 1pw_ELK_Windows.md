# Setting up Elastic Search.
  1. Download [ElasticSearch](https://www.elastic.co/downloads/elasticsearch).
  3. Extrac the `ElasticSearch` zip file and run `bin/elasticsearch` or `bin\elasticsearch.bat` on Windows.
  4. On Windows, we can run the `elasticsearch-service.bat` file to make it run in the background without having our terminal open. After running the command, we can start `elasticsearch` using terminal with `elasticsearch-service start` and we can now close the terminal if we want. `ElasticSearch` is running
  5. I get `ERROR: Failed to determine the health of the cluster. , with exit code 69` when running `elasticsearch.bat` file directly in the `bin` folder so make sure to cd out of the `bin` folder and run `bin/elasticsearch-reset-password -u elastic`
  6. (Not recommended for 1Password integration) If encounter `received plaintext http traffic on an https channel, closing connection Netty4HttpChannel` error. We can set `xpack` security enable to `false` in config file or run this command when starting `ElasticSearch`. (This step, however will make Integrations in Kibana unusable because it will not work without proper security setup)
  > bin/elasticsearch -E xpack.security.enabled=false
  7. (Not recommended for 1Password integration) Open browser, go to `http://localhost:9200` or run `curl hhtp://localhost:9200/` or `Invoke-RestMethod http://localhost:9200` with Powershell. If it returns JSON file with our cluster information, everything is working correctly.
  10. So far, so good. However, if we follow the step 5 and 6 to setup ElasticSearchand Kibana, we will encounter this error `Kibana cannot connect to the Elastic Package Registry, which provides Elastic Agent integrations` when trying to setup 1Password. To fix it we will need to try these steps.
  11. Configure x-pack to true in elasticsearc.yml, restart both elasticsearch and kibana
  12. You will need the following commands on terminal.
  
  ```
  bin\elasticsearch-create-enrollment-token --scope kibana
  bin\kibana-verification-code // ***do not clear the terminal before copying the code. We will need it for Kibana***
  bin\elasticsearch-reset-password -u elastic -i //Remember your password. It will not show up again
  ```

  After this, we can access our cluster on a browser by going to `https://localhost:9200` instead of http and enter username and password.

  13. Sign in to Kibana, go do Dev tool in Stack Management and run `GET _cluster/health` to check cluster health. If it return the correct name of Node and Cluster, Kibana now correctly connect with ElasticSearch.

  # Elastic Agent.
  1. Elastic Agent helps running ELK.
  2. Download Elastic agent [here](https://www.elastic.co/downloads/elastic-agent).
  3. Unzip, open and edit the elastic-agent.yml file to our ElasticSearch.
  4. Run `elastic-agent.exe install`.
  5. Check if agent is running by go to Kibana => Stack Management => Index management => Data Stream. If we have data on the list, then our agent is running.


  
