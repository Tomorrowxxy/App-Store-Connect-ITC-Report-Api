# [最近更新✔️]
当前版本现在使用[在Apple网站上]描述的访问令牌（https://help.apple.com/itc/appsreporterguide/?lang=en#/apd2f1f1cfa3)
# App Store Connect ITC Report Api介绍
App Store Connect ITC Report Api是获取ios应用在App Store中的销售报告的API(PHP版本)

## 特点
- 用JSON返回App Store Connect销售报告的简单PHP类
- 直接从App Store Connect获取每日、每周、每月或每年的销售报告（以及免费的应用程序下载）

## 要求 ##
* PHP 
* 有效的App Store Connect帐户<br>

## 局限性(Apple官方就是这样要求的😢) ##
* 无法得到今天的每日报告（昨天是最早的）
* 最近的周报是上周
* 最近的月报是上个月
* 最近的年度报告是去年的

## 教你第一步😁 ##
```php
require_once("class/iTunesSalesApi.php");

// $reporter = new iTunesSalesApi();
// $reporter = new iTunesSalesApi("reporter-access-token","myVendorId");
$reporter->setAccessToken("reporter-access-token")->setVendor("myVendorId");
```
## 简单使用😀 ##
### 获取帐户列表 ###
```php
try{
    // 返回为数组或false
    // 如果为false，则可以通过调用getErrorsAsString()或$reporter->errors来获取错误。
    if ($accounts = $reporter->getAccounts()) {
        print_r(json_encode($accounts));
    } else {
        echo "Api  Errors : ".$reporter->getErrorsAsString();
    }
}catch (Exception $e){
    echo $e->getMessage();
}
```
### 设置特定帐户 ###
```php
$accounts = $reporter->getAccounts();

// 从列表返回第一个帐户的示例
// 帐户是数字，但setaccount解析getaccounts()方法的原始返回
$reporter->setAccount($accounts[0]);

```
### 获取供应商ID的列表 ###
```php
try{
    // 返回为数组或false
    // 如果为false，则可以通过调用getErrorsAsString()或$reporter->errors来获取错误。
    if ($vendors = $reporter->getVendors()) {
        print_r(json_encode($vendors));
        
        // 设置供应商ID
        $reporter->setVendor($vendors[0]);
    } else {
        echo "Api  Errors : ".$reporter->getErrorsAsString();
    }
}catch (Exception $e){
    echo $e->getMessage();
}
```
### 日报 ###
```php
try{
    // 返回为数组或false
    // 如果为false，则可以通过调用getErrorsAsString()或$reporter->errors来获取错误。
    // 日期为YYYYMMDD格式
    if ($data = $reporter->getSalesDailyReport()) { // 默认为昨天
        print_r(json_encode($data));
    } else {
        echo "Api  Errors : ".$reporter->getErrorsAsString();
    }
}catch (Exception $e){
    echo $e->getMessage();
}
```
### 周报 ###
```php
try{
    //YYYYMMDD
    if ($data = $reporter->getSalesWeeklyReport()) { // 默认为上周
        print_r(json_encode($data));
    } else {
        echo "Api  Errors : ".$reporter->getErrorsAsString();
    }
}catch (Exception $e){
    echo $e->getMessage();
}
```
### 月报 ###
```php
try{
    //YYYYMM
    if($data = $reporter->getSalesMonthlyReport()) { // 默认上个月
        //Do something with data your're good to go
        print_r(json_encode($data));
    }
    else{
        echo "Api  Errors : ".$reporter->getErrorsAsString();
    }
}catch (Exception $e){
    echo $e->getMessage();
}
```
### 年报 ###
```php
try{
    // YYYY 
    if($data = $reporter->getSalesYearlyReport()) { // 默认上一年
        print_r(json_encode($data));
    } else {
        echo "Api  Errors : ".$reporter->getErrorsAsString();
    }
}catch (Exception $e){
    echo $e->getMessage();
}
```
## 可选设置 ##
如果要在本地保存报告，可以指定要将其保存到的文件夹：<br>
```php
$reporter->setFolder("/path/to/my/folder"); // 默认情况下不在本地保存（也没有缓存）
```
如果要在遇到非关键错误时停止脚本，请将throwerrors设置为true:<br>
```php
$reporter->throwErrors = true; // 默认为false
```
如果指定了文件夹，API将加载以前缓存的文件（如果可用）。如果不想使用缓存文件，但想要一组新的数据，可以强制刷新请求：<br>
```php
$reporter->setUseCache(false); // 默认为true
```
如果只对自己赚了多少钱感兴趣，可以通过将报告模式设置为仅盈利来跳过免费的销售项目：<br>
```php
$reporter->setReportModeEarningsOnly();  //Default is $reporter->setReportModeAll();
```
## 返回示例 ##
```php
{
  "success": true,
  "response": {
    "report_start_date": "20190212",
    "report_end_date": "20190212",
    "number_sales": 35,
    "app_downloads": 808,
    "revenues": {
      "USD": {
        "turnover": 35.88,
        "earnings": 25.2,
        "sales": 25
      },
      "PHP": {
        "turnover": 99,
        "earnings": 69.3,
        "sales": 1
      },
      "EUR": {
        "turnover": 7.97,
        "earnings": 4.62,
        "sales": 4
      },
      "CHF": {
        "turnover": 4,
        "earnings": 2.6,
        "sales": 2
      },
      "CAD": {
        "turnover": 2.79,
        "earnings": 1.95,
        "sales": 3
      }
    },
    "details": [
      {
        "Provider": "APPLE",
        "Provider Country": "US",
        "SKU": "AppSKU",
        "Developer": "Developer Name",
        "Title": "App Name",
        "Version": "1.4",
        "Product Type Identifier": "1F",
        "Units": "6",
        "Developer Proceeds": "0",
        "Begin Date": "02/12/2017",
        "End Date": "02/12/2017",
        "Customer Currency": "USD",
        "Country Code": "US",
        "Currency of Proceeds": "USD",
        "Apple Identifier": "1111111111",
        "Customer Price": "0",
        "Promo Code": " ",
        "Parent Identifier": " ",
        "Subscription": " ",
        "Period": " ",
        "Category": "Health & Fitness",
        "CMB": "",
        "Device": "iPad",
        "Supported Platforms": "iOS",
        "Proceeds Reason": " ",
        "Preserved Pricing": " ",
        "Client": " "
      },(...)
```
## Todo list ##
- 管理其他类型的报告（订阅、订阅事件和NewStand)