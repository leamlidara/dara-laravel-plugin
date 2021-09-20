# Dara Laravel Plugin

## វិធឺតម្លើង
1. ទាញយក និងចម្លងថតឯកសារ "Dara" ទៅកាន់ថតឯកសារ "app" នៃគម្រោង laravel របស់អ្នក.

2. កែប្រែឯកសារ "**public/index.php**" ដោយបន្ថែមឃ្លា `include_once __DIR__.'/../app/Dara/helper.php';` នៅមុន `vendor/autoload.php`<br/>ឩទាហរណ៍:<br/>
```php
...
include_once __DIR__.'/../app/Dara/helper.php';
include_once __DIR__.'/../vendor/autoload.php';
...
```

3. កែប្រែឯកសារ "**app/Http/kernel.php**" ដោយបន្ថែមឃ្លា `\App\Dara\Middleware\DMinifyResponse::class,` ដូចខាងក្រោម
```php
protected $middlewareGroups = [
  'web' => [
    ...
    \App\Dara\Middleware\DMinifyResponse::class,
  ]
];
```

4. កែប្រែឯកសារ "**config/app.php**" ដោយបន្ថែមឃ្លា `App\Dara\Providers\DAppServiceProvider::class,` ដូចខាងក្រោម
```php
'providers' => [
    ...
    App\Dara\Providers\DAppServiceProvider::class,
],
```

5. កែប្រែឯកសារ "**routes/web.php**" ដោយបន្ថែមឃ្លា `include_once __DIR__ . '/../app/Dara/routes.php';`

6. កែប្រែឯកសារ "**.env**" ដោយបន្ថែមឃ្លាដូចខាងក្រោម
```
CDN_URL="${APP_URL}"
APP_VERSION=1.0.0
```


## វិធឺប្រើប្រាស់
**Global Functions**
1. **durl($url = '')** ស្រដៀងទៅនឹង url របស់ laravel ដែរ តែ durl វា return url ចេញពី APP_URL នៅក្នុង .env ដោយលុប http ឬ https ចេញ។

2. **cdn_url($url, $needVersion = 1)** នឹង return url ចេញពី CDN_URL ដែលនៅក្នុង .env។ ប្រសិនបើ parameter **$needVersion = 1** នោះវានឹង return ដោយភ្ជាប់មកជាមួយ APP_VERSION ដែលនៅក្នុង .env ។ (ធ្វើបែបនេះ ដើម្បីឲ្យយើងអាច force refresh ឯកសារដោយគ្រាន់តែប្តូរ APP_VERSION នៅក្នុង .env)

3. **dcleanxxs($html_text) ឬ @dcleanxxs($html_text) `សម្រាប់ View`** អនុញ្ញាតឲ្យអ្នកធ្វើការ សម្អាតកូដមួយចំនួនដែលមានហានិភ័យទៅលើ HTML នៅពេលដែលលោកអ្នកបង្ហាញ HTML ចូលទៅក្នុង view

4. **dRandom($length, $randomType)** អនុញ្ញាតឲ្យអ្នកធ្វើការ random អក្សរ ឬ លេខ តាមចំនួនតួរអក្សរ, $randomType អាចជា `DRANDOM_TYPE_NUMERIC`, `DRANDOM_TYPE_ALPHABET` ឬ `DRANDOM_TYPE_BOTH`

5. **dGetString($parameter_name)** អនុញ្ញាតឲ្យអ្នក ធ្វើការទាញយក តម្លៃដែល parse ចូលដោយ user។ ការទាញយកនេះ នឹងទាញយកចេញពី `$_POST` ឬ `$_GET`

6. **dEncrypt($string, $additional = '')** អនុញ្ញាតឲ្យអ្នកធ្វើការ encrypt string។ លទ្ធិផលដែលចេញមក មិនអាចប្រើឆ្លង session បានទេ។ មានន័យថាលោកអ្នកមិនអាចធ្វើការរក្សាទុកលទ្ធផលនេះ សម្រាប់ប្រើប្រាស់លើកក្រោយទេ។

7. **dDecrypt($string, $additional = '')** អនុញ្ញាតឲ្យអ្នកធ្វើការ decrypt string ដែលជាលទ្ធិផលនៃការ encrypt ដោយប្រើ dEncrypt។ ប្រសិនបើ dEncrypt បានប្រើប្រាស់ $additional, នោះលោកអ្នកត្រូវបន្ថែម ជាចាំបាច់។ ប្រសិនបើ $additional នៅក្នុង dEncrypt ខុសពី dDecrypt នោះលទ្ធផលដែលផ្តល់ឲ្យគឺ NULL។

**Classes**
1. **DCaptcha** សម្រាប់ធ្វើការជាមួយ Captcha
    - **@dcaptcha() `នៅក្នុង View`** សម្រាប់ធ្វើការ echo url របស់ captcha
    - **\App\Dara\DCaptcha::getCode()** សម្រាប់ចាប់យក code របស់ captcha ជា string

2. **DImage** សម្រាប់ធ្វើការជាមួយ រូបភាព
    - **\App\Dara\DImage::createThumbnail($srcFile, $destFile, $width=null, $height=null)** សម្រាប់ធ្វើការបង្កើត Thumbnail របស់រូបភាព
    - **\App\Dara\DImage::optimize($file, $quality=70)** សម្រាប់ធ្វើការ optimize រូបភាព
    - **\App\Dara\DImage::resize($srcFile, $destFile, $width, $height)** សម្រាប់ធ្វើការ resize រូបភាព
    - **\App\Dara\DImage::crop($srcFile, $destFile, $width, $height)** សម្រាប់ធ្វើការ crop រូបភាព

3. **DDateTime** សម្រាប់ធ្វើការជាមួយ ថ្ងៃខែ
    - **\App\Dara\DDateTime::isValid($date, $format = 'Y-m-d')** សម្រាប់ធ្វើការ ពិនិត្យមើលថាតើថ្ងៃខែដែលផ្តល់ឲ្យ ត្រឹមត្រូវតាម ទម្រង់ដែលចង់បានឬទេ
    - **\App\Dara\DDateTime::convert($fromDate, $fromFormat, $toFormat)** សម្រាប់ធ្វើការ convert ថ្ងៃខែពីទម្រង់មួយ ទៅទម្រង់មួយ
> ចំណាំ៖ ចំពោះ datetime format សូមចុចតំណរភ្ជាប់នេះ https://www.php.net/manual/en/datetime.format.php

4. **DNumber** សម្រាប់ធ្វើការជាមួយ លេខ
    - **\App\Dara\DNumber::numberFormat($number, $precision = 2)** សម្រាប់ធ្វើការ format លេខ ដោយប្រើក្បៀស និងតាម precision ដែលកំណត់
    - **\App\Dara\DNumber::roundDown($number, $precision = 4, $formated = false)** សម្រាប់ធ្វើការ បង្គត់លេខចុះ តាម precision ដែលកំណត់
    - **\App\Dara\DNumber::hexToDec($hex)** សម្រាប់ធ្វើការ បម្លែងពី Hexadecimal ទៅកាន់ Decimal
    - **\App\Dara\DNumber::decToHex($dec)** សម្រាប់ធ្វើការ បម្លែងពី Decimal ទៅកាន់ Hexadecimal

5. **DUtil** សម្រាប់ធ្វើការជាមួយ ផ្សេងៗ
    - **\App\Dara\DUtil::getClientIP()** សម្រាប់ធ្វើការចាប់យក IP address របស់ client
    - **\App\Dara\DUtil::getClientCountryCode()** សម្រាប់ធ្វើការចាប់យក Country Code របស់ client
    - **\App\Dara\DUtil::isFileSupport($controlName, $supportExtension = array('pdf'))** សម្រាប់ធ្វើការពិនិត្យមើលថាតើ ឯកសារដែលផ្តល់ឲ្យ ត្រូវបានអនុញ្ញាតឬទេ

6. **DPaginate** សម្រាប់ធ្វើការជាមួយ Pagination (សម្រាប់ ធ្វើការជាមួយ Pagination ដោយប្រើ Raw SQL)
