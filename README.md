## Register service provider.

    ```php
    'providers' => [
    //  ...
        Kali\InfluxDB\Providers\ServiceProvider::class,
    ]
    ```
    ```php
    'aliases' => [
    //  ...
        'InfluxDB' => Kali\InfluxDB\Facades\InfluxDB::class,
    ]
    ```

* Define env variables to connect to InfluxDB

```ini
INFLUXDB_HOST=
INFLUXDB_PORT=
INFLUXDB_TOKEN=
INFLUXDB_BUCKET=
INFLUXDB_ORG=
```

* Write this into your terminal inside your project
    ```ini
    php artisan vendor:publish


## Reading Data

```php
<?php
use Kali\InfluxDB\Facades\InfluxDB;

// Get query client
$queryApi = InfluxDB::createQueryApi();

// Synchronously executes query and return result as unprocessed String
$result = $queryApi->queryRaw(
    "from(bucket: \"my-bucket\")
                |> range(start: 0)
                |> filter(fn: (r) => r[\"_measurement\"] == \"weather\"
                                 and r[\"_field\"] == \"temperature\"
                                 and r[\"location\"] == \"Sydney\")"
);

InfluxDB::close();
```

## Writing Data

```php
<?php

$writeApi = InfluxDB::createWriteApi();

// create an array of points
$result = $writeApi->write([
    Point::measurement("blog_posts")
      ->addTag("post_id", $post->id)
      ->addField("likes", 6)
      ->addField("comments", 3)
      ->time(time())
]);

InfluxDB::close();
```
