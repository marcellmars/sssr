# sssr
SslSeflSigneR

## run it a from command line

Let's say we want to do wildcard self-signed SSL certificate for example.com.
If we run:

        sssr.py -d example.com

it will generate three files in secrets/ directory:
 - example.com.key (a private key of the generated SSL certificate. if used e.g. with ngins it could be copied to /etc/ssl/private/example.com.key)
 - example.com.crt (a SSL certificate which e.g. nginx will serve to its clients and usually it gets copied to /etc/ssl/certs/example.com.crt)
 - example.com_cacert.pem (this is the Certificate Authority file which (self)signed the certificate. This is the file which could be imported to web browsers' SSL settings. It won't bother anymore anyone who does it.)

Adding more info looks more trustworthy:

        sssr.py --domain-name example.com --common-name "Example Inc" --country US --state NY --location "New York" --organization "Example Inc" --organization-unit "SSL Unit of Example Inc"

## write it into file
If one prefers to hardcode the info for SSL certificate into sssr.py it should find DISTINGUISHED_NAME at the top of the sssr.py:

        DISTINGUISHED_NAME = {"domain": "example.com",
                      "C": "US",
                      "ST": "Maryland",
                      "L": "Baltimore",
                      "O": "Foobars of the World",
                      "OU": "[let's generate certs]",
                      "CN": "example.com"}

If that's the case every time one runs:

        sssr.py

it will make new self-signed wildcard SSL certificate being its own Certifitace Authority. 

## use it as a library

        >>> import sssr
        >>> sssr.DISTINGUISHED_NAME['domain'] = "example.com"
        >>> sssr.DISTINGUISHED_NAME['CN'] = "Example Inc"
        >>> sssr.generate_certs()
        >>> sssr.remove_files() ### only three files are left: the private key, SSL cert and CA certifiace

All kudos to http://stackoverflow.com/users/608639/jww who sent the answer at stackoverflow: http://stackoverflow.com/questions/21297139/how-do-you-sign-certificate-signing-request-with-your-certification-authority/21340898#21340898.
