---
comments: true
date: 2008-10-03 16:18:43
layout: post
title: Unit tests with Automatic Transaction Management
categories: [castle, testing]
---

In our projects we're using Castle's Automatic Transaction Management facility (ATM). This facility allows to declaratively define transaction boundaries: by marking a method by a specific attribute the call to this method will be wrapped in a transaction, so that a transaction will be opened before calling the method and committed right after finishing method execution.

In code, it looks like this:

    [Transactional]
    public class Foo
    {
        [Transaction]
        public virtual void Bar()
        {
            // Do something in a transaction
        }
    }


Let's suppose that later, we want to test this code. Say, we want to verify that code in Foo.Bar() method is executed in a transaction. Moreover, we want to assure that the marking attributes will not be deleted accidentally.

We can test that within two aspects. First - an integration test: a real application part will be spawned and a call will be performed. After, we should verify that changes was committed by transaction. It takes time to run such a test (integration test usually runs slower) and effort to write & verify such a test. Second way to test that - is to check presence or required attributes. This will be a fast unit test that will just check that attribute is still applied to a method.

Those methods complements each other. Ideally, it should be both written.

Let's see how we can verify that attributes required by ATM are applied to required methods.

First, we've defined the API that we want to use to do our check:

    CustomAssertions.ShouldUseATM<Foo>(foo => foo.Bar());

Yes, you see it right, we want to make it type save, refactor-able and nice looking.

This helper class should verify next assumptions, all required by ATM:
  * Tested class should be marked with TransactionalAttribute; 
  * Method should be marked by TransactionAttribute; 
  * Method should be virtual.

Solution we came to this problem is the next class:
    
    public static partial class CustomAssertions
    {
        public static void ShouldUseATM<T>(Expression<Action<T>> methodExpression) where T : class 
        {
            ThrowIfTypeNotMarkedAsTransactional(typeof (T));

            var methodInfo = ((MethodCallExpression) methodExpression.Body).Method;

            ThrowIfMethodIsNotMarkedWithTransactionAttribute(methodInfo);
            ThrowIfMethodMarkedWithTransactionAttributeIsVirtual(methodInfo);
        }

        private static void ThrowIfTypeNotMarkedAsTransactional(Type classType)
        {
            object[] foundAttributes = classType.GetCustomAttributes(typeof(TransactionalAttribute), false);
            if  (foundAttributes.Length == 0)
                Assert.Fail("The type '{0}' is not marked with [Transactional] attribute", classType.Name);
        }

        private static void ThrowIfMethodIsNotMarkedWithTransactionAttribute(MethodInfo info)
        {
            var found = info.GetCustomAttributes(typeof(TransactionAttribute), false);
            if (found.Length == 0)
                Assert.Fail("Method '{0}' from '{1}' type is not marked with [Transaction] attribute",
                           info.Name,
                           info.DeclaringType.Name);
        }

        private static void ThrowIfMethodMarkedWithTransactionAttributeIsVirtual(MethodBase methodInfo)
        {
            if  (methodInfo.IsVirtual && !methodInfo.IsFinal)
                return;

            Assert.Fail("Method '{0}' from '{1}' type is marked with [Transaction] attribute, but is not declared as virtual.",
                        methodInfo.Name,
                        methodInfo.DeclaringType.Name);
        }
    }

So far, we're pleased with how it works for us. 
