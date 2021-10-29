# reproduce-elasticLogstashHandler-bugsearch

Start Server and run http://127.0.0.1:8000/foo

Without passing the http_client we get slow 4-5 error message that there is no route whats fine for this test.
[2021-10-29T07:29:23.205486+00:00] php.CRITICAL: Uncaught Exception: Idle timeout reached for "http://localhost:8000/bar/_bulk". {"exception":"[object] (Symfony\\Component\\HttpClient\\Exception\\TimeoutException(code: 0): Idle timeout reached for \"http://localhost:8000/bar/_bulk\". at reproduce-elasticLogstashHandler-bugsearch/vendor/symfony/http-client/Chunk/ErrorChunk.php:65)"} []

passing the http_client give us hundreds of request until memoryLimit is reached. debug.log is over 6MB after 1 request
[2021-10-29T07:30:22.406323+00:00] http_client.INFO: Request: "POST http://localhost:8000/bar/_bulk" [] []

Error Msg
````
Fatal error: Allowed memory size of 134217728 bytes exhausted (tried to allocate 20480 bytes) in /reproduce-elasticLogstashHandler-bugsearch/vendor/symfony/monolog-bridge/Handler/ElasticsearchLogstashHandler.php on line 112
Fatal error: Allowed memory size of 134217728 bytes exhausted (tried to allocate 32768 bytes) in /reproduce-elasticLogstashHandler-bugsearch/vendor/symfony/error-handler/Error/OutOfMemoryError.php on line 1
````

