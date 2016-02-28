---
layout: post
title: "Internal DSLs and Fluent APIs in your domain"
date: 2013-08-24 16:31
comments: true
categories:  [community, development]
published: false
---
This repository contains the code from a short presentation I gave in March 2012 at Geneva .NET User Group.

I've talked about how internal dsls (Domain Specific Languages) implemented using a Fluent API can serve to a better separation of concerns, code maintainability and clarity.

```
	public class Customer
	{
		public Guid Id { get; set; }
		public string FirstName { get; set; }
		public string LastName { get; set; }
		public int Age { get; set; }
		public string CreditCardNumber { get; set; }
	}
```

An example of domain classes where various infrastructure concerns are defined using attributes on domain classes:

```
	[Class]
	[Indexed(Index = "LuceneIndex")]
	public class Customer
	{
		[Id(Column = "CUSTOMER_ID")]
		[Generator(1, Class = "guid")] 
		[CsvField(Ignore = false, Name = "Customer ID", Index = 1)]
		[DocumentId(Name = "customerId")]
		[FieldBridge(typeof(GuidBridge))]
		public Guid Id { get; set; }

		[Property(Column = "FIRST_NAME")]
		[Length(Min = 3, Max = 20)]
		[CsvField(Index = 2, Name = "First name")]
		[Field(Index.Tokenized, Store = Store.Yes)]
		public string FirstName { get; set; }

		[Property]
		[Length(Min = 3, Max = 20)]
		[CsvField(Index = 3, Name = "Last name")]
		[Field(Index.Tokenized, Store = Store.Yes)]
		public string LastName { get; set; }

		[Property]
		[Range(Min = 16, Max = 65)]
		[CsvField(Ignore = true)]
		public int Age { get; set; }

		[Property]
		[CreditCardNumber]
		[CsvField(Ignore = true)]
		[Field(Index.UnTokenized, Store = Store.No)]
		[FieldBridge(typeof(CreditCardEncriptedFieldBridge))]
		public string CreditCardNumber { get; set; }
	}
```

