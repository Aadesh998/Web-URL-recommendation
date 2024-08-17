# Web URL Recommendation 
URL Dataset:
•	We start with a dataset that contains URLs.
 ![image](https://github.com/user-attachments/assets/425469bf-3a3a-4097-af9d-568a066562fd)

Extracting Text from Websites:
•	The goal is to extract the text from a website using its URL. Here's a sample code:
headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.162 Safari/537.36'}
def get_text(url):
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        # Parse the HTML content
        soup = BeautifulSoup(response.content, "html.parser")
        text = soup.get_text()
        # Extract and clean the text
        cleaned_text = ' '.join(text.split())
        cleaned_text = cleaned_text[:2000]
        return cleaned_text
    else:
        return f"Error: {response.status_code}"
Extracting Website Information:
•	We need to extract key pieces of information from the URLs, including the protocol, domain name, network location, and domain age.
•	Protocol:

def get_info(web_url):
    parsed_url = urlparse(web_url)
    protocol = parsed_url.scheme
    return protocol

•	Domain Name:
def domain_info(web_url):
    parsed_url = urlparse(web_url)
    url = parsed_url.netloc
    domain_name = url.split('.')[-1]
    
    return domain_name

•	Net location:

def net_loc(web_url):
    parsed_url = urlparse(web_url)
    net_loc = parsed_url.netloc
    
    return net_loc

•	Domain name:

from datetime import datetime

def domain_age(domain_name):
    domain_name = whois.whois(domain)
    creation_date = domain_name.creation_date
    expiration_date = domain_name.expiration_date
    if isinstance(creation_date, str) or isinstance(expiration_date, str):
        try:
            creation_date = datetime.strptime(creation_date, '%Y-%m-%d')
            expiration_date = datetime.strptime(expiration_date, "%Y-%m-%d")
        except:
            return -1  # Indicates an error in date parsing
    if creation_date is None or expiration_date is None:
        return -1  # Indicates missing date information
    if isinstance(creation_date, list) or isinstance(expiration_date, list):
        return -1  # Indicates date is a list rather than a single datetime object
    try:
        age_of_domain = (expiration_date - creation_date).days
        return age_of_domain
    except:
        return -1  # Indicates an error in date calculation


# Get domain age in days
age = domain_age(df['domain'][3])
print(f"Domain age in days: {age}")

 

Recommending Secure URLs:
•	Given a URL, our system will recommend secure alternatives based on security features such as the protocol, domain name, and domain age.

Website Recommendation Based on Security:
•	Our system will recommend websites based on security features. We will use a vector database for semantic search based on contextual meaning.
Verifying Originality of Recommended URLs:
•	The system will verify the recommended URLs to ensure they are original and not phishing websites.
Model Accuracy:
 

MODEL Features: -
•  Have_IP:
•	Description: Checks if the URL contains an IP address instead of a domain name. URLs with IP addresses are often associated with malicious websites.
•	Example:
o	URL: http://192.168.1.1/login
o	Feature Value: 1 (indicating the presence of an IP address)
•  Have_At:
•	Description: Checks if the URL contains the "@" symbol. URLs with "@" are often used in phishing attacks to obscure the true destination.
•	Example:
o	URL: http://example.com@malicious.com
o	Feature Value: 1 (indicating the presence of "@" in the URL)
•  URL_Length:
•	Description: Measures the length of the URL. Longer URLs can be a sign of phishing or malicious activity.
•	Example:
o	URL: http://example.com/a/very/long/path/to/a/resource
o	Feature Value: 50 (indicating the URL's length)
•  URL_Depth:
•	Description: Measures the depth of the URL by counting the number of slashes ("/") in the path. A deep URL can indicate a suspicious or complex structure.
•	Example:
o	URL: http://example.com/a/b/c/d/e
o	Feature Value: 5 (indicating the depth of the URL)
•  Redirection:
•	Description: Checks if the URL contains a redirection using "//" after the protocol. URLs with redirections can be used to deceive users.
•	Example:
o	URL: http://example.com//redirect
o	Feature Value: 1 (indicating the presence of a redirection)
•  https_Domain:
•	Description: Checks if the domain in the URL uses HTTPS. Secure websites typically use HTTPS, while insecure ones may not.
•	Example:
o	URL: https://secure.example.com
o	Feature Value: 1 (indicating the presence of HTTPS in the domain)
•  Prefix/Suffix:
•	Description: Checks for the presence of a hyphen ("-") in the domain. Hyphens are often used in malicious URLs to create domains similar to legitimate ones.
•	Example:
o	URL: http://secure-example.com
o	Feature Value: 1 (indicating the presence of a hyphen in the domain)
•  DNS_Record:
•	Description: Checks if the DNS record for the domain exists. The absence of a DNS record can indicate that the domain is fake or invalid.
•	Example:
o	Domain: fakeexample.com
o	Feature Value: 0 (indicating the absence of DNS records)
•  Domain_Age:
•	Description: Measures the age of the domain in days. Newly registered domains are often used in phishing or other malicious activities.
•	Example:
o	Domain: newlyregistered.com
o	Feature Value: 30 (indicating the domain is 30 days old)


