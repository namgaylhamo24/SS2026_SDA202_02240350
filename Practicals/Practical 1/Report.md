# Design Report: URL Shortener System
In this guide we are building a url shortener design like TinyURL, in which it takes time, the unclean and long url were turned into a short one and when the client clicks on the short url it redirects to the original long url.

## 1. Problem Analysis

The primary challenge of this project is to build a system that can handle an amount of data quickly. We get 100 million URLs every day. * Building a system needs to be able to handle a lot of data without slowing down.

* We need to ensure that our system can handle a lot of requests at the time without getting slow.

* The system wilpredominantly  asked to redirect often than create new URLs because most people will read the URLs more than create new ones.

* The ratio of reads to writes is 10 to 1 so we must guarantee that the system can handle a lot of reads quickly.

The URLs need to be short, with a maximum length of 62 characters. This implies that we need to find a way to create IDs that are short but also don't get confused with each other.We need to find a balance between simplicity and making it able to handle a lot of data.Fortunately, we don't need to worry about deleting or updating records.This makes things simpler.Our system only needs to ensure that new records are correct.We must find ways to give IDs to all the records across all the servers.

* Creating unique URLs for a vast amount of data. 365 Billion records.

We also need to handle a lot of requests. 11,600 Reads per second.

The main problems we need to solve are:

* Creating URLs for a huge amount of data
* Developing a data model that can handle hundreds of billions of records
* Building a system that can handle a lot of traffic without getting slow.

## 2. Author's Solution Analysis

