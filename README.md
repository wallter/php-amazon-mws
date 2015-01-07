PHP Amazon MWS Intergration
============
## Now with a better config system (dynamic) and PHP 5.5.14 compatible 

A library to connect to Amazon's Merchant Web Services (MWS) in an object-oriented manner, with a focus on intuitive usage.  

This is __NOT__ for Amazon Web Services (AWS) - Cloud Computing Services.


## Example Usage
Here are a couple of examples of the library in use.
All of the technical details required by the API are handled behind the scenes,
so users can easily build code for sending requests to Amazon
without having to jump hurdles such as parameter URL formatting and token management. 

Here is an example of a function used to get all warehouse-fulfilled orders from Amazon updated in the past 24 hours:
```php
function getAmazonOrders() {
    $config = array(
            'amazon.com' => array(
                'merchant_id' => '',
                'marketplace_id' => '', // not needed or used
                'access_key_id' => '',
                'secret_key' => '',
                'service_url' => 'https://mws.amazonservices.ca/',
                'log_path' => 'logs/amazon.ca/log.txt',
                'log_funciton' => '',
                'mute_log' => false
            ),
            'amazon.ca' => array(
                'merchant_id' => '',
                'marketplace_id' => '', // not needed or used
                'access_key_id' => '',
                'secret_key' => '',
                'service_url' => 'https://mws.amazonservices.com/',
                'log_path' => 'logs/amazon.com/log.txt',
                'log_funciton' => '',
                'mute_log' => false
            );

    $amz = new AmazonOrderList($store, false, null, $config);
    $amz->setLimits('Modified', "- 24 hours");
    $amz->setFulfillmentChannelFilter("MFN"); //no Amazon-fulfilled orders
    $amz->setOrderStatusFilter(
        array("Unshipped", "PartiallyShipped", "Canceled", "Unfulfillable")
        ); //no shipped or pending
    $amz->setUseToken(); //Amazon sends orders 100 at a time, but we want them all
    $amz->fetchOrders();
    return $amz->getList();
}
```

## NOT TESTED / UPDATED for PHP 5.3.3:
This example shows a function used to send a previously-created XML feed to Amazon to update Inventory numbers:
```php
function sendInventoryFeed($feed) {
    $amz=new AmazonFeed(); //if there is only one store in config, it can be omitted
    $amz->setFeedType("_POST_INVENTORY_AVAILABILITY_DATA_"); //feed types listed in documentation
    $amz->setFeedContent($feed);
    $amz->submitFeed();
    return $amz->getResponse();
}
```
