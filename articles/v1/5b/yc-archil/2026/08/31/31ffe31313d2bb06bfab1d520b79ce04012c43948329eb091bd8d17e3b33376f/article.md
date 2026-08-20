---
schema_version: "1.0.0"
document_id: "31ffe31313d2bb06bfab1d520b79ce04012c43948329eb091bd8d17e3b33376f"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-6ed493c8d31a"
canonical_url: "https://archil.com/article/wordpress-on-archil"
published_at: null
first_seen_at: "2026-08-09T19:14:46.011593+00:00"
fetched_at: "2026-08-09T19:14:47.557372+00:00"
content_hash: "sha256:a6733f50af321aeafed56804d46c356e729d8b368a67fb59aa57b7041938c032"
---

# Tutorial: Archil with Wordpress

## What is Wordpress?


[WordPress](https://wordpress.com/) is an open-source content management system (CMS) that powers approximately 40% of all websites on the internet. Its popularity stems from an intuitive user interface, thousands of customizable themes, and an extensive plugin ecosystem that enables users to add sophisticated functionality without requiring coding expertise. This flexibility makes WordPress suitable for diverse projects, from personal blogs and portfolio sites to complex e-commerce platforms and enterprise-level applications.


## Hosting Considerations: Balancing Cost, Control, and Complexity


When it comes to hosting a WordPress site, users face several trade-offs between cost, control, and technical complexity:


**Managed Hosting Solutions** offer the easiest path forward, handling server management, security updates, and infrastructure maintenance. While convenient, these services can become expensive as your site scales and traffic increases.


**Self-Hosting** presents an attractive alternative for cost-conscious users, allowing complete control over the server environment and potentially significant savings. However, this approach comes with notable challenges:


- **Reliability concerns** : Personal hosting setups typically lack the redundancy and uptime guarantees of professional data centers
- **Technical expertise required** : Managing servers, security patches, backups, and troubleshooting requires substantial technical knowledge
- **Time investment** : Server maintenance can become a significant ongoing commitment


**Cloud Hosting Solutions** offer a middle ground, providing scalable infrastructure at competitive prices. However, they introduce their own learning curve, as cloud platforms often use specialized tools, interfaces, and deployment methods that differ significantly from traditional server management.


It can be challenging to choose a hosting approach that aligns with your technical expertise, budget constraints, and reliability requirements. Fortunately, there are tools available that help you gain the benefits of all these approaches.


## **Why Archil Makes the Difference**


[Archil](https://archil.com/) bridges the gap between cost-effective cloud storage and user-friendly hosting by eliminating the traditional complexity of cloud platforms. Instead of wrestling with unfamiliar cloud interfaces and deployment processes, Archil allows you to[leverage S3's](https://docs.archil.com/data-sources/s3-object-storage) scalable storage while interacting with it through a POSIX compliant file system interface.


Key advantages include:


- **Mounted file system access** : Copy, move, and manage files using familiar commands
- **Zero code changes** : WordPress plugins and themes work without modification
- **Reduced latency** : File system operations outperform API calls once cached
- **Seamless workflow** : No need to learn new interfaces or modify existing development processes


This solution is ideal for WordPress sites seeking S3's cost benefits without operational complexity. To mount an Archil disk to your EC2 instance, follow the[Archil Getting Started Guide](https://docs.archil.com/getting-started/quickstart) to create an Archil disk that will serve as the storage for your WordPress site.


## Getting Started with Archil


To create a WordPress site using Archil, you'll need a few essential components:


1. An AWS Account


1. To create, go to the[AWS Homepage](https://aws.amazon.com/) and select “Create account”
2. Follow the prompts until the account is set up


2. A Linux AWS EC2 instance (this is just a virtual machine that you can interact with like a normal terminal)


1. Access the **AWS Management Console**
2. Search for **EC2** in the top search bar and select it
3. Select **Launch Instance**
4. Select **Amazon Linux** as the AMI
5. The rest is configurable with more information in the[AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) . However, as long as the instance is **Amazon Linux** it should work.
6. *Note: Since Archil handles storage management, you can minimize EBS storage allocation to reduce costs*


3. An S3 or S3 Compatible Storage Bucket (this is where we will store the WordPress site and any other files needed for it)


1. Go to the **AWS Management Console**
2. Go to **S3** and select it
3. The rest is configurable to however you would like, with more information in the[AWS Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)


4. A WordPress Site


1. Create a new site and develop your WordPress content locally
2. Once ready, you'll deploy this site to your **EC2 Instance** with Archil


## Migrate to EC2


1. Find the root folder of your **WordPress** site
2. Copy the site over to your **EC2 Instance**
3. ` scp -i /path/to/your-key.pem -r ./yourWordPressSite \[ec2-user@\](<mailto:ec2-user@3.84.112.253>)yourEc2PublicIp:~/wordPressSite`
4. ` ssh` into your **EC2 Instance**
5. ` ssh -i /path/to/your-key.pem \[ec2-user@\](<mailto:ec2-user@3.84.112.253>)yourEc2PublicIp`
6. Install Apache or Nginx + PHP + MySQL (e.g., for Apache + PHP)
7. ` sudo yum update -y sudo yum install -y httpd php php-mysqlnd mariadb-server unzip`
8. Start the service
9. ` sudo systemctl start httpd sudo systemctl enable httpd`
10. Check the status of the service
11. ` sudo systemctl status httpd`
12. If the connection is **healthy** , it should output something like this
13. ` ● httpd.service - The Apache HTTP Server Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: di> Active: active (running) since Wed 2025-08-06 01:31:49 UTC; 18s ago Docs: man:httpd.service(8) Main PID: 127349 (httpd) Status: "Total requests: 0; Idle/Busy workers 100/0;Requests/sec: 0; Bytes> Tasks: 177 (limit: 1111) Memory: 13.0M CPU: 77ms CGroup: /system.slice/httpd.service ├─127349 /usr/sbin/httpd -DFOREGROUND ├─127350 /usr/sbin/httpd -DFOREGROUND ├─127351 /usr/sbin/httpd -DFOREGROUND ├─127352 /usr/sbin/httpd -DFOREGROUND └─127353 /usr/sbin/httpd -DFOREGROUND`
14. **Set up MySQL/MariaDB Database (skip if database is already configured)**


1. Start MariaDB service:
2. ` sudo systemctl start mariadb sudo systemctl enable mariadb`
3. Secure the MariaDB installation:` sudo mysql_secure_installation`


- Press Enter for no root password initially
- Set a strong root password when prompted
- Answer security questions


4. Create a WordPress database and user, then run these SQL commands:
5. ` mysql -u root -p CREATE DATABASE wordpress; CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'YourStrongPassword123!'; GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost'; FLUSH PRIVILEGES; EXIT;`


15. **Move WordPress Files to Web Directory**


1. Copy your WordPress files to the web root:
2. ` sudo cp -r ~/wordPressSite/* /var/www/html/ sudo chown -R apache:apache /var/www/html/ sudo chmod -R 755 /var/www/html/`


16. **Configure WordPress Database Connection**


1. Edit the WordPress configuration file:
2. ` sudo nano /var/www/html/wp-config.php`
3. Update the database settings with your actual values:
4. ` define( 'DB_NAME', 'wordpress' ); define( 'DB_USER', 'wpuser' ); define( 'DB_PASSWORD', 'YourStrongPassword123!' ); define( 'DB_HOST', 'localhost' );`


17. **Install Required PHP Extensions**
18. ` sudo yum install -y php-gd php-xml php-mbstring php-json php-curl`
19. **Restart Services**
20. ` sudo systemctl restart httpd sudo systemctl restart mariadb`
21. **Test Database Connection**
22. ` mysql -u wpuser -p'YourStrongPassword123!' -e "USE wordpress; SELECT 'Connection successful!' as status;"`
23. **Configure EC2 Security Group**


1. Go to AWS EC2 Console
2. Select your instance
3. Click on the Security tab
4. Edit inbound rules to allow HTTP (port 80) and HTTPS (port 443) from anywhere (0.0.0.0/0)


### Troubleshooting Common Issues


- **“Error establishing a database connection”:** Check database credentials in wp-config.php and verify MySQL is running
- **Permission denied errors:** Ensure proper ownership with` sudo chown -R apache:apache /var/www/html/`
- **404 errors:** Check Apache configuration and ensure mod_rewrite is enabled
- **PHP errors:** Install missing PHP extensions and restart Apache


Congratulations! Your WordPress site should now be up and running. To view it, simply find the **Public IPv4 address** of your EC2 instance in the EC2 Console, paste it into your browser, and your **WordPress site** should appear.


## Using Archil on Your WordPress Site to Host Static Images


At this point, you should have your Archil disk mounted to the EC2 instance. If you don’t, no worries. Just follow this quick[tutorial](https://docs.archil.com/getting-started/quickstart) . For the remainder of this tutorial, we will use` /mnt/data` as the root of the Archil disk.


Now, all you need to do is create a symbolic link between your old WordPress content store and your mounted bucket. This will seamlessly migrate your data from your` wp-content` folder to S3.


*Note: Make sure to update*` /mnt/data` *file paths with whats on your EC2 instance.*


` # Stop Apache to avoid file conflicts during copy sudo systemctl stop httpd # Create the target directory in your Archil mount mkdir -p /home/ec2-user/mnt/data/wp-content/uploads # Copy existing uploads to Archil (preserving permissions and structure) if \[ -d "/var/www/html/wp-content/uploads" \]; then echo "Copying existing uploads to Archil directory..." sudo cp -r /var/www/html/wp-content/uploads/* /home/ec2-user/mnt/data/wp-content/uploads/ 2>/dev/null || echo "No files to copy or directory doesn't exist" echo "Copy completed." else echo "No existing uploads directory found - creating fresh setup." fi # Backup the original uploads directory (just in case) if \[ -d "/var/www/html/wp-content/uploads" \]; then sudo mv /var/www/html/wp-content/uploads /var/www/html/wp-content/uploads.backup.$(date +%Y%m%d_%H%M%S) echo "Original uploads directory backed up." fi # Create symbolic link sudo ln -sf /home/ec2-user/mnt/data/wp-content/uploads /var/www/html/wp-content/uploads # Set proper permissions sudo chown -R apache:apache /home/ec2-user/mnt/data/wp-content/ sudo chown -h apache:apache /var/www/html/wp-content/uploads # Ensure Apache can access the parent directories sudo usermod -a -G ec2-user apache sudo chmod 755 /home/ec2-user sudo chmod 755 /home/ec2-user/mnt/data sudo chmod -R 755 /home/ec2-user/mnt/data/wp-content/ # Start Apache sudo systemctl start httpd echo "Migration completed! Your uploads are now stored in Archil."`


As quickly as that, your finite local media store just became an infinitely scalable, highly reliable, S3-backed storage. If you go to S3 you should see all of your files uploaded there. As you add more items to your S3 bucket from S3 or locally, it will sync, letting you work on your WordPress site while Archil manages the rest.


With the symbolic link, you won’t even have to update your file paths within your WordPress site!


## **The Archil Advantage**


Traditional cloud migration requires mastering multiple services, APIs, and unfamiliar infrastructure tools. Archil eliminates this barrier by presenting S3's enterprise-grade storage through a familiar file system interface.


The result? You get cloud-scale performance and cost savings while maintaining the straightforward development experience that makes WordPress so accessible. Simply mount your storage, deploy your site, and benefit from enterprise infrastructure without the enterprise complexity.
