# NGROK/Magento Usage Guide

This repository contains instructions and resources for setting up NGROK with Magento2.

## Getting Started

Before you begin, ensure you have an NGROK account. If not, sign up at [ngrok.com](https://ngrok.com/).

### Installation

Follow the platform-specific installation instructions provided in the [NGROK Documentation](https://ngrok.com/docs/getting-started/).

## Static Domain Setup

NGROK offers the ability to create a one static domains for your projects (FREE). This allows for easier access to your tunnels. Follow the steps outlined in the [NGROK Blog Post](https://ngrok.com/blog-post/free-static-domains-ngrok-users) to set up a static domain for your NGROK account.

## Cloud Edges and Tunnel Creation

NGROK provides cloud edges for all users, enhancing performance and reliability. Learn how to utilize this feature in the [NGROK Blog Post](https://ngrok.com/blog-post/cloud-edges-for-all-ngrok-users).

## Modify the hosts file:
![image](https://github.com/inajaf/ngrok-magento/assets/43228234/0f2fe6ed-2399-4ba7-b151-f6ce2017d184)

Ensure that you specify the port number correctly. Instead of using 9555, you have the option to use 8080 Magento NGINX config.

## To create a tunnel using cloud edges:

Run the following command (replace YOUR_EDGE_HASH and the port number of magento project):
```bash
ngrok tunnel --label edge=YOUR_EDGE_HASH http://localhost:9555
```
![image](https://github.com/inajaf/ngrok-magento/assets/43228234/cd43289a-8ece-48cb-ad1f-c5564ab25998)

## Instert your static ngrok url to the tables via the sql:
```bash
UPDATE `core_config_data` SET `value` = 'https://YOUR-STATIC-URL-PROVIDED-FROM-NGROK.ngrok-free.app/' WHERE `core_config_data`.`path` = 'web/unsecure/base_url';
UPDATE `core_config_data` SET `value` = 'https://YOUR-STATIC-URL-PROVIDED-FROM-NGROK.ngrok-free.app/' WHERE `core_config_data`.`path` = 'web/secure/base_url';
UPDATE `core_config_data` SET `value` = 'https://YOUR-STATIC-URL-PROVIDED-FROM-NGROK.ngrok-free.app/' WHERE `core_config_data`.`path` = 'web/unsecure/base_link_url';
UPDATE `core_config_data` SET `value` = 'https://YOUR-STATIC-URL-PROVIDED-FROM-NGROK.ngrok-free.app/' WHERE `core_config_data`.`path` = 'web/secure/base_link_url';
```
## Just in case if admin url is showing 404 page:
REPLACE to yours admin path.
```bash
INSERT INTO `core_config_data`(`scope`, `scope_id`, `path`, `value`)
VALUES(
    'default', -- Or whatever your Scope is
    0,         -- Or whatever your Scope ID is
    'admin/url/custom',
    'https://YOUR-STATIC-URL-PROVIDED-FROM-NGROK.ngrok-free.app/magento_admin/'
);
```

```bash
php bin/magento setup:config:set --backend-frontname="magento_admin"
sudo rm -r pub/static/*/* && sudo rm -r var/view_preprocessed/* && bin/magento cache:f && bin/magento cache:c
bin/magento setup:upgrade && bin/magento setup:di:compile && bin/magento s:s:d -f
```
## Disable the default restriction page in browser:
![image](https://github.com/inajaf/ngrok-magento/assets/43228234/87075b95-ac42-400b-bc94-7371708e1c69)

1. Open the edges and the your static app domain:

![image](https://github.com/inajaf/ngrok-magento/assets/43228234/1f4e7ccd-0e5c-4361-9093-caa036d60054)

2. Go to Request Headers and add the option with value
```bash
ngrok-skip-browser-warning  true
```
