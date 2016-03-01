---
comments: true
date: 2009-06-05 09:17:50
layout: post
title: Why so noisy?
categories: [development]
---

A snippet from a project I've been reading:
    
    // -- FILE ------------------------------------------------------------------
    // name       : UserConfig.cs
    // created    : 
    // language   : c#
    // environment: .NET 2.0
    // --------------------------------------------------------------------------
    using System;

    namespace TheNamespace.Configuration
    {

        // ------------------------------------------------------------------------
        public class UserConfig
        {

            // ----------------------------------------------------------------------
            public UserConfig( System.Configuration.Configuration configuration )
            {
                if ( configuration == null )
                {
                    throw new ArgumentNullException( "configuration" );
                }

                this.configuration = configuration;
            } // UserConfig

            // ----------------------------------------------------------------------
            public System.Configuration.Configuration Configuration
            {
                get { return this.configuration; }
            } // Configuration

            // ----------------------------------------------------------------------
            public string FilePath
            {
                get { return this.configuration.FilePath; }
            } // FilePath


            // ----------------------------------------------------------------------
            // members
            private readonly System.Configuration.Configuration configuration;

        } // class UserConfig

    } // namespace TheNamespace.Configuration
    // -- EOF -------------------------------------------------------------------

It's a matter of taste, of course, but I'm wondering, what is the reason to decorate the code with a such ceremony?

Much of the time we, developers, are spending **reading** the code. That's why the code that is clean, easy to read and understand is so much appreciated.
