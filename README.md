<?php

// Step 1: include the PayPal API library
require_once("PayPal-PHP-SDK/paypal/rest-api-sdk-php/sample/bootstrap.php");

// Step 2: Create a payment object
$payment = new PayPal\Api\Payment();

// Step 3: Set the payment details
$payment->setIntent("sale")
  ->setPayer($payer)
  ->setTransactions(array($transaction))
  ->setRedirectUrls($redirectUrls);

// Step 4: Create a payment transaction
$transaction = new PayPal\Api\Transaction();
$amount = new PayPal\Api\Amount();
$details = new PayPal\Api\Details();

$details->setShipping(2.2)
  ->setTax(1.3)
  ->setSubtotal(17.50);

$amount->setCurrency("USD")
  ->setTotal(21)
  ->setDetails($details);

$transaction->setAmount($amount)
  ->setDescription("Payment description")
  ->setInvoiceNumber(uniqid());

// Step 5: Create a payer object
$payer = new PayPal\Api\Payer();
$payer->setPaymentMethod("paypal");

// Step 6: Create redirect URLS
$redirectUrls = new PayPal\Api\RedirectUrls();
$redirectUrls->setReturnUrl("http://www.example.com/success.php")
  ->setCancelUrl("http://www.example.com/failure.php");

// Step 7: Execute the payment
try {
  $payment->create($apiContext);
  echo "Payment Created Successfully";
} catch (PayPal\Exception\PayPalConnectionException $ex) {
  echo "Exception: " . $ex->getMessage() . PHP_EOL;
  echo "Payment Failed";
}

// Step 8: Get the payment approval URL
$approvalUrl = $payment->getApprovalLink();

// Step 9: Redirect the user to the payment approval URL
header("Location: " . $approvalUrl);

?>
