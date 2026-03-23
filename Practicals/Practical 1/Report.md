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
### API Design

The author keeps things simple busing just two API endpoints.

1. The first endpoint accepts a POST request, which takes a URL and provides a shortened link.

2. The second endpoint operates as a GET request, which accepts a URL and delivers the full original URL.

This method allows the system to determine the appropriate user redirection path.The system presents a simple design that enables users to perform two fundamental tasks without experiencing any complications.

### URL Redirecting Strategy

A key decision is whether to use 301 or 302 redirects.A 301 redirect makes the browser treat the mapping as permanent.The browser saves it in its cache.After the user clicks the link, all following requests will go directly to the initial destination.

This method decreases the amount of work that servers must perform.The ability to track link clicks will be lost to you.

The shortener service never sees those requests.A 302 redirect makes the browser check the shortener service first.This process requires additional time to complete.

The system provides total tracking capabilities for each individual user interaction.The system allows you to monitor user interactions while tracking their sources together with analytic data.

The author shows that both methods perform equally well.The choice depends on which factors you consider more important. 

### Hash Function Approaches

The author presents two ways to generate URLs.

#### Method 1: Hash Plus Collision Resolution

The first method uses base 62 conversion.
* CRC32 and MD5 and SHA-1 are common examples of hash functions.
* The long URL is run through the hash function.
* The first seven characters are taken to keep things short.
* Truncating a hash raises the possibility of multiple hash values.

Two different long URLs might end up with the same short URL.

The system adds extra content to the hash until it reaches a new short URL.

The process continues until the system discovers a distinct short URL.

Bloom filters enable efficient URL checking through their ability to track short URLs and existing short URLs. 

Bloom filters provide a fast method to determine whether a short URL has been claimed.


### Method 2: Base 62 Conversion

The second method uses base 62 conversion.

Websites receive unique numeric identifiers, which function as their URLs.

The ID is converted into a string using base 62.

Base 62 uses numbers, including lowercase letters and uppercase letters.

*An ID like 2009215674938 becomes "zn9edcu".*

The underlying ID is guaranteed to be unique.
Collisions cannot happen.

The author compares the two methods.

* Hash plus collision gives a fixed seven-character URL.
* The system does not depend on an ID generator for its operation.
* Collisions are possible.
* Handling collisions adds complexity.
* Base 62 conversion makes collisions impossible.
* It flows naturally from an ID generator.

However, the short URL length can grow over time.
There is a security tradeoff.With base 62, if someone knows how the system works,  they could guess the short URL.This could be a problem with private links.
