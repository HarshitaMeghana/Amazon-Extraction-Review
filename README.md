amazon-scraper-python

Travis Coveralls github PyPI Docker Build Status License
Description

This package allows you to search for products on Amazon and extract some useful information (ratings, number of comments).

I wrote a French blog post about it here
Requirements

    Python 3
    pip3

Installation

pip3 install -U amazonscraper

Command line tool amazon2csv.py

After the package installation, you can use the amazon2csv.py command in the terminal.

After passing a search request to the command (and an optional maximum number of products), it will return the results as csv :

amazon2csv.py --keywords="Python programming" --maxproductnb=2

Product title,Rating,Number of customer reviews,Product URL,Image URL,ASIN
"Python Crash Course: A Hands-On, Project-Based Introduction to Programming",4.5,370,https://www.amazon.com/Python-Crash-Course-Hands-Project-Based/dp/1593276036,https://images-na.ssl-images-amazon.com/images/I/51F48HFHq6L.jpg,1593276036
"A Smarter Way to Learn Python: Learn it faster. Remember it longer.",4.7,384,https://www.amazon.com/Smarter-Way-Learn-Python-Remember-ebook/dp/B077Z55G3B,https://images-na.ssl-images-amazon.com/images/I/51fNZfTUPXL.jpg,B077Z55G3

You can also pass a search url (if you added complex filters for example), and save it to a file :

amazon2csv.py --url="https://www.amazon.com/s/ref=nb_sb_noss_2?url=search-alias%3Daps&field-keywords=python+scraping" > output.csv

You can then open it with your favorite spreadsheet editor (and play with the filters) :

snapshot amazon2csv

More info about the command in the help :

amazon2csv.py --help

Using the amazonscraper Python package

# -*- coding: utf-8 -*-
import amazonscraper

results = amazonscraper.search("Python programming", max_product_nb=2)

for result in results:
    print("{}".format(result.title))
    print("  - ASIN : {}".format(result.asin))
    print("  - {} out of 5 stars, {} customer reviews".format(result.rating, result.review_nb))
    print("  - {}".format(result.url))
    print("  - Image : {}".format(result.img))
    print()

print("Number of results : %d" % (len(results)))

Which will output :

Python Crash Course: A Hands-On, Project-Based Introduction to Programming
  - ASIN : 1593276036
  - 4.5 out of 5 stars, 370 customer reviews
  - https://www.amazon.com/Python-Crash-Course-Hands-Project-Based/dp/1593276036
  - Image : https://images-na.ssl-images-amazon.com/images/I/51F48HFHq6L.jpg

A Smarter Way to Learn Python: Learn it faster. Remember it longer.
  - ASIN : B077Z55G3B
  - 4.7 out of 5 stars, 384 customer reviews
  - https://www.amazon.com/Smarter-Way-Learn-Python-Remember-ebook/dp/B077Z55G3B
  - Image : https://images-na.ssl-images-amazon.com/images/I/51fNZfTUPXL.jpg

Number of results : 2

Attributes of the Product object
Attribute name 	Description
title 	Product title
rating 	Rating of the products (number between 0 and 5, False if missing)
review_nb 	Number of customer reviews (False if missing)
url 	Product URL
img 	Image URL
asin 	Product ASIN (Amazon Standard Identification Number)
Docker

You can use the amazon2csv tool with the Docker image

You may execute :

docker run -it --rm thibdct/amazon2csv --keywords="Python programming" --maxproductnb=2
🤘 The easy way 🤘

I also built a bash wrapper to execute the Docker container easily.

Install it with :

curl -s https://raw.githubusercontent.com/tducret/amazon-scraper-python/master/amazon2csv \
> /usr/local/bin/amazon2csv && chmod +x /usr/local/bin/amazon2csv

You may replace /usr/local/bin with another folder that is in your $PATH

Check that it works :

On the first execution, the script will download the Docker image, so please be patient

amazon2csv --help
amazon2csv --keywords="Python programming" --maxproductnb=2

You can upgrade the app with :

amazon2csv --upgrade

and even uninstall with :

amazon2csv --uninstall

TODO

    If no product was found with the CSS selectors, it may be a new Amazon page style => change user agent and get the new page. Loop on all the user agents and check all the CSS selectors again
    Find a way to get the products without css selectors

Footer
