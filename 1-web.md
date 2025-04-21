# 🌐 RoboShop Web/Frontend Setup 🛒
* Welcome to the RoboShop Web/Frontend setup guide! This service uses Nginx to serve web content for the RoboShop application, creating your online store’s webpage. Follow these simple steps to get it running! 🚀

## 📋 Prerequisites

- A Linux server (e.g., Rocky Linux, Fedora) with internet access.  
- Root or sudo access to run commands.
- Backend services (catalogue, user, cart, shipping, payment) running with known IP addresses.
- Port 80 open for web traffic.


## 🛠️ Setup Instructions
#### 1️⃣ Install Nginx
- Install the Nginx web server to host the RoboShop website.
```powershell
yum install nginx -y
```
##### 🎉 What’s happening? Nginx is installed and ready to serve web content.

#### 2️⃣ Start & Enable Nginx
- Start the Nginx service and ensure it runs automatically on boot.
```powershell
systemctl enable nginx
```
```powershell
systemctl start nginx
```
##### ✅ What’s happening? Nginx is active and will start with your server.

#### 3️⃣ Test Nginx in Browser
##### Open a browser and visit your server’s public IP to see the default Nginx page.
```powershell
http://<your-server-public-IP>:80
``` 
##### 🔍 What’s happening? You should see Nginx’s welcome page. If not, check Nginx status with systemctl status nginx.

#### 4️⃣ Remove Default Content
* Clear out the default Nginx files to make room for RoboShop’s content.
```powershell
rm -rf /usr/share/nginx/html/*
```
##### 🧹 What’s happening? The default web files are removed, ready for RoboShop files.

#### 5️⃣ Download RoboShop Frontend Files
##### Download the RoboShop web content as a zip file.
```powershell
curl -o /tmp/web.zip https://roboshop-builds.s3.amazonaws.com/web.zip
```

##### ⬇️ What’s happening? The RoboShop website files are downloaded to /tmp/web.zip.

#### 6️⃣ Extract Frontend Files
##### Unzip the downloaded files to Nginx’s web directory.
```powershell
cd /usr/share/nginx/html
```
```powershell-interactive
unzip /tmp/web.zip
```
##### 📦 What’s happening? The RoboShop web files are now in the correct folder for Nginx to serve.

#### 7️⃣ Verify RoboShop Content
##### Revisit the same URL in your browser to see the RoboShop website.
```powershell
http://<your-server-public-IP>:80
```
##### 🎨 What’s happening? You should now see the RoboShop storefront instead of the default Nginx page.

#### 8️⃣ Configure Nginx Reverse Proxy
- Set up Nginx to route requests to the appropriate backend services (e.g., catalogue, user). Create a configuration file.
```powershell
vim /etc/nginx/default.d/roboshop.conf
```
##### Add the following content, replacing <catalogue-server-IP>, <user-server-IP>, etc., with the actual IP addresses of your backend services:
```powershell
proxy_http_version 1.1;

location /images/ {
  expires 5s;
  root   /usr/share/nginx/html;
  try_files $uri /images/placeholder.jpg;
}
location /api/catalogue/ { proxy_pass http://<catalogue-server-IP>:8080/; }
location /api/user/ { proxy_pass http://<user-server-IP>:8080/; }
location /api/cart/ { proxy_pass http://<cart-server-IP>:8080/; }
location /api/shipping/ { proxy_pass http://<shipping-server-IP>:8080/; }
location /api/payment/ { proxy_pass http://<payment-server-IP>:8080/; }

location /health {
  stub_status on;
  access_log off;
}
```
##### ⚠️ Important: Replace localhost with the actual IP addresses of your backend servers. localhost is a placeholder to avoid errors.
##### 🔧 What’s happening? This configuration tells Nginx how to forward API requests to backend services and serve images.

#### 9️⃣ Restart Nginx
* Apply the new configuration by restarting Nginx.
```powershell
systemctl restart nginx
```
##### 🔄 What’s happening? Nginx reloads with the updated settings, making your RoboShop site fully functional.

##### 🎉 Success!
* Your RoboShop web frontend is now live! 🌟 Visit 
```powershell
http://<your-server-public-IP>:80
```
to see your online store in action.

### 🐛 Troubleshooting

* Nginx not running? Check status:
```powershell
systemctl status nginx
```

* Page not loading? Ensure port 80 is open and the correct IP is used.

* Backend errors? Verify backend IPs in roboshop.conf and ensure services are running.

### Check logs for clues:
```powershell
cat /var/log/nginx/error.log
```







