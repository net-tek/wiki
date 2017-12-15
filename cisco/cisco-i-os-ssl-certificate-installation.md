<!-- TITLE: Cisco I Os Ssl Certificate Installation -->

This brief note covers getting an SSL certificate registered with Thawte onto a Cisco router running IOS.

# Create the Trustpoint
This binds the SSL cert to the CA (Certificate Authority) which in this case is Thawte.  The subject is where you will specify all the usual bits you need in the cert. Also ensure that fqdn defined and is that same as the common name. If you don’t the name of the router will be used instead.


```text
Router# conf t
Router(config)# crypto pki trustpoint thawte.com
Router(ca-trustpoint)# enroll terminal
Router(ca-trustpoint)# serial-number none
Router(ca-trustpoint)# fqdn hostname.domain.com
Router(ca-trustpoint)# ip-address none
Router(ca-trustpoint)# subject-name CN=hostname.domain.com,O=Organisation, OU=Department,L=Location,ST=State,C=Country
Router(ca-trustpoint)# revocation-check none
Router(ca-trustpoint)# end
Router# wr mem 
```

Note the ’subject-name’ is all on one line – due to the width of this page and spaces it is wrapping.

# Authenticate the CA with the trustpoint
This means loading Thawte’s Premium signing certificate into the router.
 
It took quite a while to locate Thawte’s Premium Signing Certificate from their website so there is nothing to stop you cut’n'pasting from this post.
 
If you wish to get your own copy then you can download the complete set from http://www.thawte.com/roots/. Accept their terms (assuming you do) then download and unpack the zip file.
 
The file you need is : Thawte SSLWEB Server Roots\thawte Premium Server CA\Thawte Premium Server CA.pem
 
It is really important you get the right CA Certificate file on your router. Unfortunately the process won’t fail until you try and import your new certificate if you get the wrong one !!!!
 
Open the file in a text editor and you can then cut and paste at the appropriate time.


```text
Router# conf t
Router(config)# crypto pki authenticate thawte.com
Enter the base 64 encoded CA certificate.
End with a blank line or the word "quit" on a line by itself
-----BEGIN CERTIFICATE-----
MIIDJzCCApCgAwIBAgIBATANBgkqhkiG9w0BAQQFADCBzjELMAkGA1UEBhMCWkEx
FTATBgNVBAgTDFdlc3Rlcm4gQ2FwZTESMBAGA1UEBxMJQ2FwZSBUb3duMR0wGwYD
VQQKExRUaGF3dGUgQ29uc3VsdGluZyBjYzEoMCYGA1UECxMfQ2VydGlmaWNhdGlv
biBTZXJ2aWNlcyBEaXZpc2lvbjEhMB8GA1UEAxMYVGhhd3RlIFByZW1pdW0gU2Vy
dmVyIENBMSgwJgYJKoZIhvcNAQkBFhlwcmVtaXVtLXNlcnZlckB0aGF3dGUuY29t
MB4XDTk2MDgwMTAwMDAwMFoXDTIwMTIzMTIzNTk1OVowgc4xCzAJBgNVBAYTAlpB
MRUwEwYDVQQIEwxXZXN0ZXJuIENhcGUxEjAQBgNVBAcTCUNhcGUgVG93bjEdMBsG
A1UEChMUVGhhd3RlIENvbnN1bHRpbmcgY2MxKDAmBgNVBAsTH0NlcnRpZmljYXRp
b24gU2VydmljZXMgRGl2aXNpb24xITAfBgNVBAMTGFRoYXd0ZSBQcmVtaXVtIFNl
cnZlciBDQTEoMCYGCSqGSIb3DQEJARYZcHJlbWl1bS1zZXJ2ZXJAdGhhd3RlLmNv
bTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEA0jY2aovXwlue2oFBYo847kkE
VdbQ7xwblRZH7xhINTpS9CtqBo87L+pW46+GjZ4X9560ZXUCTe/LCaIhUdib0GfQ
ug2SBhRz1JPLlyoAnFxODLz6FVL88kRu2hFKbgifLy3j+ao6hnO2RlNYyIkFvYMR
uHM/qgeN9EJN50CdHDcCAwEAAaMTMBEwDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG
9w0BAQQFAAOBgQAmSCwWwlj66BZ0DKqqX1Q/8tfJeGBeXm43YyJ3Nn6yF8Q0ufUI
hfzJATj/Tb7yFkJD57taRvvBxhEf8UqwKEbJw8RCfbz6q1lu1bdRiBHjpIUZa4JM
pAwSremkrj/xw0llmozFyD4lt5SZu5IycQfwhl7tUCemDaYj+bvLpgcUQg==
-----END CERTIFICATE-----
quit
Certificate has the following attributes:
Fingerprint MD5: 069F6979 16669002 1B8C8CA2 C3076F3A
Fingerprint SHA1: 627F8D78 27656399 D27D7F90 44C9FEB3 F33EFA9A
% Do you accept this certificate? [yes/no]: yes
Trustpoint CA certificate accepted.
% Certificate successfully imported
Router(config)# end
Router(config)# wr mem 
You can now check this certificate
 Router#show crypto pki certificate
CA Certificate
 Status: Available
 Certificate Serial Number: 0x1
 Certificate Usage: General Purpose
 Issuer:
 e=premium-server@thawte.com
 cn=Thawte Premium Server CA
 ou=Certification Services Division
 o=Thawte Consulting cc
 l=Cape Town
 st=Western Cape
 c=ZA
 Subject:
 e=premium-server@thawte.com
 cn=Thawte Premium Server CA
 ou=Certification Services Division
 o=Thawte Consulting cc
 l=Cape Town
 st=Western Cape
 c=ZA
 Validity Date:
 start date: 01:00:00 BST Aug 1 1996
 end   date: 23:59:59 GMT Dec 31 2020
 Associated Trustpoints: thawte.com 
```

#  Generate CSR – Begin Certificate enrollment.
This starts the process of getting your own certificate by generating a CSR or Certificate Request.


```text
Router# conf t
Router(config)# crypto pki enroll thawte.com
% Start certificate enrollment ..
% The subject name in the certificate will include: CN=hostname.domain.com,O=Organisation,OU=Department,L=Location,ST=State,C=Country
% The subject name in the certificate will include: hostname.domain.com
Display Certificate Request to terminal? [yes/no]: yes
Certificate Request follows:
MIICDjCCAXcCAQAwgawxCzAJBgNVBAYTAkdCMQ8wDQYDVQQIEwZMb25kb24xDzAN
BgNVBAcTBkxvbmRvbjEcMBoGA1UECxMTQWNjb3VudHMgRGVwYXJ0bWVudDEdMBsG
A1UEChMUT3VyIfsjkfjsdkfhksdjfklssdfsdfsdfdsfdsWQxGzAZBgNVBAMTEnNzbHZwbi5wb2Jv
eC5jby51azEhMB8GCSqGSIb3DQEJAhYSc3NsdnBuLnBvYm94LmNvLnVrMIGfMA0G
CSqGSIb3DQEBAQUAA4GNADCBiQKBgQCzlVpHnVWmZK+krq6J/R3fQwf9kceyLB8u
iis91j5EON4pMvVcKiCpJDa+kGLTSzalmKERHO4Tz6Nm53HLmCo3JGGkox+Pnv7C
oVlZu23ukAZmF0/fzfsaJkrDeWWagsDgdFseee+ffDse4XfbWVnIVqYGoWtyxdGQm3vSJ
569/tKQV5QIDAQABoCEwHwYJKoZIhvcNAQkOMRIwEDAOBgNVHQ8BAf8EBAMCBaAw
DQYJKoZIhvcNAQEEBQADgYEApaSo522bW34bcGgA5zr1uuoi2IUyV+1IBb3K+teG
RtUyrw1Z+4aVhBlsi1kSoVoLdKiUTAr5IwtEEO6pVq2uxxYvia7D1g24R5m8JN1h
HgafrfnnAvtP8EFH//0XLrdWVAUL25KtMpqjhJricWsc62CnbCiGPb/AsmaIJcBe
hxQ=
---End - This line not part of the certificate request---
Redisplay enrollment request? [yes/no]: no
Router(config)# end
Router# wr mem
```

Now cut out the CSR the router has generated and send it to Thawte.

# Import Certificate
Once you have received your certificate back from Thawte you need to import it into the router.


```text
Router# conf t
Router(config)# crypto pki import thawte.com certificate

Enter the base 64 encoded certificate.
End with a blank line or the word "quit" on a line by itself
-----BEGIN CERTIFICATE-----
MIIDJzCCApCgAwIBAgIBATANBgkqhkiG9w0BAQQFADCBzjELMAkGA1UEBhMCWkEx
FTATBgNVBAgTDFdlc3Rlcm4gQ2FwZTESMBAGA1UEBxMJQ2FwZSBUb3duMR0wGwYD
VQQKExRUaGF3dGUgQ29uc3VsdGluZyBjYzEoMCYGA1UECxMfQ2VydGlmaWNhdGlv
biBTZXJ2aWNlcyBEaXZpc2lvbjEhMB8GA1UEAxMYVGhhd3RlIFByZW1pdW0gU2Vy
dmVyIENBMSgwJgYJKoZIhvcNAQkBFhlwcmVtaXVtLXNlcnZlckB0aGF3dGUuY29t
MB4XDTk2MDgwMTAwMDAwMFoXDTIwMTIzMTIzNTk1OVowgc4xCzAJBgNVBAYTAlpB
MRUwEwYDVQQIEwxXZXN0ZXJuIENhcGUxEjAQBgNVBAcTCUNhcGUgVG93bjEdMBsG
A1UEChMUVGhhd3RlIENvbnN1bHRpbmcgY2MxKDAmBgNVBAsTH0NlcnRpZmljYXRp
b24gU2VydmljZXMgRGl2aXNpb24xITAfBgNVBAMTGFRoYXd0ZSBQcmVtaXVtIFNl
cnZlciBDQTEoMCYGCSqGSIb3DQEJARYZcHJlbWl1bS1zZXJ2ZXJAdGhhd3RlLmNv
bTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEA0jY2aovXwlue2oFBYo847kkE
VdbQ7xwblRZH7xhINTpS9CtqBo87L+pW46+GjZ4X9560ZXUCTe/LCaIhUdib0GfQ
ug2SBhRz1JPLlyoAnFxODLz6FVL88kRu2hFKbgifLy3j+ao6hnO2RlNYyIkFvYMR
uHM/qgeN9EJN50CdHDcCAwEAAaMTMBEwDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG
9w0BAQQFAAOBgQAmSCwWwlj66BZ0DKqqX1Q/8tfJeGBeXm43YyJ3Nn6yF8Q0ufUI
hfzJATj/Tb7yFkJD57taRvvBxhEf8UqwKEbJw8RCfbz6q1lu1bdRiBHjpIUZa4JM
pAwSremkrj/xw0llmozFyD4lt5SZu5IycQfwhl7tUCemDaYj+bvLpgcUQg==
-----END CERTIFICATE-----
quit
Certificate has the following attributes:
 Fingerprint MD5: 069F6979 16669002 1B8C8CA2 C3076F3A
 Fingerprint SHA1: 627F8D78 27656399 D27D7F90 44C9FEB3 F33EFA9A 

% Do you accept this certificate? [yes/no]: yes
Trustpoint CA certificate accepted.
% Certificate successfully imported
Router(config)# end
Router# wr mem 
```

An you should find you have your certificate registered on your router for use as required secure website or ssl vpn.

# RENEWALS
Unless Thawte’s CA Certificate has expired or changed – it presently expires in 2020 – you only need to go through enrolment. Also your certificate will only be effected when you import the replacement.
 
So to renew a certificate go back to step 3 and run enrolment.
 
Update: Please note that in IOS Cisco are in the process of changing the command ‘crypto ca’ to ‘crypto pki’ these are presently interchangable. The commands in this note are in the new style but you could just as easily have typed ‘crypto ca trustpoint thawte.com’ for example. The config however seems to show the new format.



