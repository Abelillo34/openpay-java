openpay-java
===============

Java client for Openpay services

This is a client implementing the payment services for Openpay at openpay.mx

What's new
----------------

- **API incompatibility**: Removed the option to look a transfer up using only the transactionId.
- **API incompatibility**: Customer charges can't be looked up only with the transactionId anymore, customerId is required. 
- Added management of plans and subscriptions
- Fixed bug that made Java 7 required.


Compatibility
----------------

As of now Java 6 is required.

Examples
----------------

#### Starting the API ####

```java
		OpenpayAPI api = new OpenpayAPI("https://sandbox-api.openpay.mx", privateKey, merchantId);
```

#### Creating a customer ####

```java
		Address address = new Address();
		address.setLine1("Calle Morelos #12 - 11");
		address.setLine2("Colonia Centro");				// Optional
		address.setLine3("Cuauhtémoc");				// Optional
		address.setCity("Distrito Federal");
        address.setPostalCode("12345");	
        address.setState("Queretaro");
        address.setCountryCode("MX");					// ISO 3166-1 two-letter code
        		    
		Customer customer = api.customers()
					.create("John", "Doe", "johndoe@example.com", "554-170-3567", address);
```

#### Charging ####
		
```java
		Card card = new Card();
		card.setCardNumber("5555555555554444");			// No dashes or spaces
        card.setHolderName("Juan Pérez Nuñez");
        card.setCvv2("422");
        card.setExpirationMonth("09");					// Month to two digits
        card.setExpirationYear("14");
		
		String description = "Service charge";
		String orderId = "Charge0001";					// Optional transaction identifier
		BigDecimal amount = new BigDecimal("200.00");
		
		Charge charge = api.charges()
					.create(customer.getId(), card, amount, description, orderId);
```

#### Payout ####

Currently Payouts are only allowed to bank accounts within Mexico.

```java
		BankAccount bankAccount = new BankAccount();
	  	bankAccount.setClabe("032180000118359719");		// Clave Bancaria Estandarizada
        bankAccount.setHolderName("Juan Pérez");
        bankAccount.setAlias("Juan's deposit account");	// Optional
        
        
		String description = "Payment to Juan";
		String orderId = "Payout0001";					// Optional transaction identifier
		BigDecimal amount = new BigDecimal("150.00");
		
		Payout transaction = api.payouts()
					.createForCustomer(customer.getId(), bankAccount, amount, description, orderId);
```

#### Subscriptions ####

Subscriptions allow you to make recurrent charges to your customers. First you need to define a subscription plan:

_(TODO)_

Installation
----------------

To install, add the following dependency to your pom.xml:

```xml
	<dependency>
		<groupId>mx.openpay</groupId>
		<artifactId>openpay-api-client</artifactId>
		<version>1.0.0</version>
	</dependency>
```


