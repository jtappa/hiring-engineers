# A better Laravel log experience with Datadog

Does anyone actually enjoy scrolling through logs in a terminal to find out what went wrong? Something broke, and you have to look through potentially thousands of lines to find the ONE thing that broke it all. Then, chances are the problem exists someplace else entirely and you have to go sift through more logs there.

Thanks to [`laravel-datadog-logger`](https://github.com/myLocalInfluence/laravel-datadog-logger), you don't have to scroll needlessly! With this tool, you can view Laravel logs in a more user friendly interface, make correlations between logs and other infrastructure issues, and set up warnings from Datadog to alert you when things go sideways!

This post will walk through installation and configuration for scraping Laravel logs with the Datadog Agent. 
You can also configure this tool to use Datadog API calls, but because of the size and number of logs a Laravel project can create, that method may have negative performance implications.

Before you get started, you need an environment with:

* the Datadog Agent
* a Laravel project

1. To set up this log manager, start from your Laravel project's root directory, and run:

```
composer require myli/laravel-datadog-logger
```

2. In your Laravel `.env`, update or add these parameters:

```
DATADOG_STORAGE_PATH="logs/laravel-json-datadog.log"
DATADOG_PERMISSIONS=0644 
DATADOG_LEVEL="info"
DATADOG_BUBBLE=true
LOG_CHANNEL="datadog-agent"
```

3. In Datadog, update your main configuration file, `datadog.yaml` to enable logs:

```
logs_enabled: true
```

4. Depending on which logs you want to see in Datadog, choose and place the corresponding conf.yaml file in `/etc/datadog-agent/laravel.d` (You may need to create the `laravel.d` directory.)

    * [Only php-cli](https://github.com/myLocalInfluence/laravel-datadog-logger/blob/master/config/agent/cli-only/conf.yaml)
    * [Only php-fpm](https://github.com/myLocalInfluence/laravel-datadog-logger/blob/master/config/agent/fpm-only/conf.yaml)
    * [Both php-cli and php-fpm](https://github.com/myLocalInfluence/laravel-datadog-logger/blob/master/config/agent/cli-fpm/conf.yaml)

5. Restart your Datadog Agent to start seeing logs!

Once you have installed and configured `laravel-datadog-logger` you can leverage Datadog's built-in logging tools to know more about your infrastructure faster.
You can also filter logs for specific flags, and see correlations across your entire stack with other logging integrations such as PostgreSQL or Apache.

A big thanks to Aurélien SCHILTZ for creating and maintaining `laravel-datadog-logger`. If you have created a Datadog library or tool you want featured, we'd love to know about it at opensource@datadoghq.com.