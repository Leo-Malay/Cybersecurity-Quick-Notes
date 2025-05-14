# 01 Information Gathering

In this section we are trying to gather as much information as we can about the web appliction in focus.

## Search Engine Discovery Reconnaissance

For search engine to work, there are [web crawlers](https://en.wikipedia.org/wiki/Web_crawler) which visit billions of websites and other web content. Their main goal is to index the whole Web. Not all the pages on our webstes are for public. Somethings unintentionally, the developer forgots to restrict this crawlers from crawling these private webpages which may possilby leak important information.

**Objective:** Identify sensitive information and configuration information of system or organization that is exposed on website directly or indirectly.

**Procedure:**

- Use search Engines to search for potential sensitive information
- Make use of multiple search engines

**Methodology:** [Google Dorking](https://www.splunk.com/en_us/blog/learn/google-dorking.html), [Google Dorking Examples](https://www.exploit-db.com/google-hacking-database)

**Defence:**

- Peroidically review the existing design and configuration.
- Keep `robots.txt` file updated

## Web Server Fingerprinting

Web servers send current version and other information like timestamp and type of web server. We might be able to use this information to find out any known vulnerability and then try to exploit those discovered vulnerabilities.

**Objective:** Determine the version and type of web server hosting the application which will enable further discovery of known vulnerability.

**Procedure:**

- Perform [banner grabbing](https://en.wikipedia.org/wiki/Banner_grabbing) using different tools.
- Send HTTP request to the server and inspect it's response.
- Try to look for `Date`,`Server`,`Last-Modified`,`ETag`,`Accept-Ranges`,`Content-Length`,`Connection`,`Content-Type` etc.

**Methodology:** [banner grabbing](https://en.wikipedia.org/wiki/Banner_grabbing)

**Defence:**

- Update web servers to latest versions.
- Obscure web server information in headers.
- Create additional layer of security between webserver and internet using hardened reverse proxy server.
