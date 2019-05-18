## Weather

基于 [高德开放平台](https://lbs.amap.com/dev/id/newuser) 的 PHP
天气信息组件。

## 安装

```shell
$ composer require overtrue/weather -vvv
```

## 配置

在使用本扩展之前，你需要去 [高德开放平台](https://lbs.amap.com/dev/id/newuser) 注册账号，然后创建应用，获取应用的
API Key。

## 使用

```php
use RealZark\Weather\Weather;

$key = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx';

$weather = new Weather($key);
```

## 获取实时天气

```php
$response = $weather->getWeather('深圳');
或者
$response = $weather->getLiveWeather('深圳');
```

示例：

```json
{
    "status": "1",
    "count": "1",
    "info": "OK",
    "infocode": "10000",
    "lives": [
        {
            "province": "广东",
            "city": "深圳市",
            "adcode": "440300",
            "weather": "中雨",
            "temperature": "27",
            "winddirection": "西南",
            "windpower": "5",
            "humidity": "94",
            "reporttime": "2018-08-21 16:00:00"
        }
    ]
}
```

## 获取近期天气预报

```php
$response = $weather->getWeather('深圳', 'forecast');
或者
$response = $weather->getForecastsWeather('深圳');
```

示例：

```json

    "status": "1", 
    "count": "1", 
    "info": "OK", 
    "infocode": "10000", 
    "forecasts": [
        {
            "city": "深圳市", 
            "adcode": "440300", 
            "province": "广东", 
            "reporttime": "2018-08-21 11:00:00", 
            "casts": [
                {
                    "date": "2018-08-21", 
                    "week": "2", 
                    "dayweather": "雷阵雨", 
                    "nightweather": "雷阵雨", 
                    "daytemp": "31", 
                    "nighttemp": "26", 
                    "daywind": "无风向", 
                    "nightwind": "无风向", 
                    "daypower": "≤3", 
                    "nightpower": "≤3"
                }, 
                {
                    "date": "2018-08-22", 
                    "week": "3", 
                    "dayweather": "雷阵雨", 
                    "nightweather": "雷阵雨", 
                    "daytemp": "32", 
                    "nighttemp": "27", 
                    "daywind": "无风向", 
                    "nightwind": "无风向", 
                    "daypower": "≤3", 
                    "nightpower": "≤3"
                }, 
                {
                    "date": "2018-08-23", 
                    "week": "4", 
                    "dayweather": "雷阵雨", 
                    "nightweather": "雷阵雨", 
                    "daytemp": "32", 
                    "nighttemp": "26", 
                    "daywind": "无风向", 
                    "nightwind": "无风向", 
                    "daypower": "≤3", 
                    "nightpower": "≤3"
                }, 
                {
                    "date": "2018-08-24", 
                    "week": "5", 
                    "dayweather": "雷阵雨", 
                    "nightweather": "雷阵雨", 
                    "daytemp": "31", 
                    "nighttemp": "26", 
                    "daywind": "无风向", 
                    "nightwind": "无风向", 
                    "daypower": "≤3", 
                    "nightpower": "≤3"
                }
            ]
        }
    ]
}
```

## 获取 XML 格式返回值

第三个参数为返回值类型，可选 `json` 与 `xml`，默认 `json`：

```php
$response = $weather->getWeather('深圳', 'live', 'xml');
$response = $weather->getLiveWeather('深圳', 'xml');
$response = $weather->getForecastsWeather('深圳', 'xml');
```

示例：

```xml
<response>
    <status>1</status>
    <count>1</count>
    <info>OK</info>
    <infocode>10000</infocode>
    <lives type="list">
        <live>
            <province>广东</province>
            <city>深圳市</city>
            <adcode>440300</adcode>
            <weather>中雨</weather>
            <temperature>27</temperature>
            <winddirection>西南</winddirection>
            <windpower>5</windpower>
            <humidity>94</humidity>
            <reporttime>2018-08-21 16:00:00</reporttime>
        </live>
    </lives>
</response>
```

## 参数说明

```php
// 通用方法，$type 可选值 'live'(实时天气), 'forecast'(近期天气预报)
array|string getWeather(string $city, string $type = 'live', string $format = 'json')
// 获取实时天气
array|string getLiveWeather(string $city, string $format = 'json')
// 获取近期天气预报
array|string getForecastsWeather(string $city, string $format = 'json')
```

* `$city` - 城市名，比如：“深圳”；
* `$type` - 返回内容类型：`base`: 返回实况天气 / `all`: 返回预报天气；
* `$formt` - 输出的数据格式，默认为 json 格式，当 output 设置为 “`xml`”
  时，输出的为 XML 格式的数据。
  
## 在 Laravel 中使用

在 Laravel 中使用也是同样的安装方式，配置写在 `config/services.php` 中：

```php
    .
    .
    .
     'weather' => [
        'key' => env('WEATHER_API_KEY'),
    ],
```

然后在 `.env` 中配置 `WEATHER_API_KEY` ：

```dotenv
WEATHER_API_KEY=xxxxxxxxxxxxxxxxxxxxx
```

可以用两种方式来获取 `RealZark\Weather\Weather` 实例：

### 方法参数注入

```php
    .
    .
    .
    public function edit(Weather $weather) 
    {
        $response = $weather->getWeather('深圳');
    }
    .
    .
    .
```

### 服务名访问

```php
    .
    .
    .
    public function edit() 
    {
        $response = app('weather')->getWeather('深圳');
    }
    .
    .
    .
```

## 参考

* [高德开放平台天气接口](https://lbs.amap.com/api/webservice/guide/api/weatherinfo/)

## License

MIT