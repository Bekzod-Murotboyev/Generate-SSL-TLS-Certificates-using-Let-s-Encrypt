
# ⚡**Let's Encrypte Definition**
Let’s Encrypt is a free, automated, and open certificate authority (CA). Yes, that’s right: SSL/TLS certificates for free. Certificates issued by Let’s Encrypt are trusted by most browsers today, including older browsers such as Internet Explorer on Windows XP SP3. In addition, Let’s Encrypt fully automates both of cost and the manual processes issuing and renewing of certificates.

# 🚀 **How Let’s Encrypt Works and how to configure it ?**
Before issuing a certificate, Let’s Encrypt validates ownership of your domain. The Let’s Encrypt client, running on your host, creates a temporary file (a token) with the required information in it. The Let’s Encrypt validation server then makes an HTTP request to retrieve the file and validates the token, which verifies that the DNS record for your domain resolves to the server running the Let’s Encrypt client.

# :grey_exclamation:**Prerequisites**
Before starting with Let’s Encrypt, you need to:
- [X] Have [NGINX](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/) or [NGINX](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-plus/) Plus installed.
- [X] Own or control the registered domain name for the certificate. If you don’t have a registered domain name, you can use a domain name registrar, such as [GoDaddy](https://www.godaddy.com/domains/domain-name-search).
- [X] Create a DNS record that associates your domain name and your server’s public IP address.


### **🏁 Step 1️⃣:** Download the Let’s Encrypt Client 👁️‍🗨️
First, download the Let’s Encrypt client, **certbot**.
```
sudo apt-get update
sudo apt-get install certbot
sudo apt-get install python3-certbot-nginx
```
:exclamation:**Note:**  In case, you are using Ubuntu 16.04 version, you should change last line above with command below:
```
sudo apt-get install python-certbot-nginx
```
---
---

### **Step 2️⃣:** Generate the SSL/TLS Certificate 👁️‍🗨️
The NGINX plug‑in for certbot takes care of reconfiguring NGINX and reloading its configuration whenever necessary.
- Run the following command to generate certificates with the NGINX plug‑in:
```
sudo certbot --nginx -d example.com -d www.example.com
```
**Note:** Here we used `example.com` and `www.example.com` to get certificate. You must enter your own domain instead!
- Respond to prompts from certbot to configure your HTTPS settings, which involves entering your email address and agreeing to the Let’s Encrypt terms of service.

When certificate generation completes, NGINX reloads with the new settings. certbot generates a message indicating that certificate generation was successful and specifying the location of the certificate on your server.

![image.png](/.attachments/image-beb1d80d-44b5-4495-9200-a18a04633d21.png)

**Note:** Let’s Encrypt certificates expire after **90 days** (on 2017-12-12 in the example). For information about automatically renenwing certificates, see Automatic Renewal of Let’s Encrypt Certificates below.

If you look at domain‑name.conf, you see that certbot has modified it:

![image.png](/.attachments/image-432bc817-b7bd-4ce8-bf76-60f2c492a7f6.png)

---
---

### **Step 3️⃣:** Verifying Certbot Auto-Renewal 👁️‍🗨️
Let’s Encrypt’s certificates are only valid for ninety days. This is to encourage users to automate their certificate renewal process. The certbot package we installed takes care of this for us by adding a systemd timer that will run twice a day and automatically renew any certificate that’s within thirty days of expiration.

You can query the status of the timer with `systemctl:`
```
sudo systemctl status certbot.timer
```

![image.png](/.attachments/image-7b9244e3-9ec6-41a0-a379-4195709d6b00.png)

To test the renewal process, you can do a dry run with `certbot:`
```
sudo certbot renew --dry-run
```
If you see no errors, you’re all set. When necessary, Certbot will renew your certificates and reload Nginx to pick up the changes. If the automated renewal process ever fails, Let’s Encrypt will send a message to the email you specified, warning you when your certificate is about to expire.

---
---

# **Conclusion** 🚩
In this wiki page, you installed the Let’s Encrypt client certbot, downloaded SSL certificates for your domain, configured Nginx to use these certificates, and set up automatic certificate renewal. If you have further questions about using Certbot, [the official documentation](https://certbot.eff.org/docs/) is a good place to start.
