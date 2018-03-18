.. _guides/webhooks:

Webhooks
========

Process realtime status updates
-------------------------------
A webhook is a URL Mollie will call when an object's status changes, for example when a payment changes from ``open`` to
``paid``. You should put a script behind this URL that – when it's called –
:ref:`fetches the payment status <v2/payments-get>` and processes it if it's status has changed.

In the example case of a payment changing to ``paid``, you should mark the order belonging to the payment as paid
accordingly.

The webhook will be called with a single POST-parameter called ``id``, which for example will contain the value
``tr_d0b0E3EA3v``. You should use that ID to actively fetch the payment to find out about it's status. This step seems a
little cumbersome, but :ref:`proper security <security>` dictates this flow. Since the status is not transmitted in the
webhook, fake calls to your webhook will never result in orders being processed without being actually paid.

If an endpoint supports webhooks, you can specify the webhook URL you want to receive status changes on by providing the
parameter ``webhookUrl``.

Webhooks for v2 API endpoints
-----------------------------
TODO: Add documentation on v2 webhooks.

Webhooks for v1 API endpoints
-----------------------------
The v1 API only supports webhooks for payment status changes. Please refer to the
:ref:`list of payment status changes <guides/payment-status-changes>` for all supported payment statuses.

What IPs will the webhook requests be originating from?
-------------------------------------------------------
Read `our support article <https://help.mollie.com/hc/en-us/articles/213470829>`_ for more information on the IP
addresses that Mollie uses.

Retry schema
------------
In response to Mollie calling your webhook, you only have to return the HTTP-status ``200 OK``. Mollie then knows your
system correctly processed the request. In case you return a different status – let's say because there's a temporary
problem with your hosting service – we'll keep trying for a few hours, allowing you to process the request later on,
after your hosting service is restored.

Example
-------
To get started with webhooks, please refer to the documentation of the
`Mollie API client <https://www.mollie.com/en/modules>`_ you are using.

The most important task your webhook script has to complete is to process orders whenever the status of a payment turns
out to be ``paid``. Therefore, please note the exact working of this process really depends on your product, your
business and your website.