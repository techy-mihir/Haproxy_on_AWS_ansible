# Haproxy_on_AWS_ansible
![image](https://user-images.githubusercontent.com/70094412/111814812-eb8fb080-8900-11eb-9344-d214d3b5b838.png)

👉 **HAProxy**, which stands for High Availability Proxy, is a popular open-source software TCP/HTTP Load Balancer and proxying solution which can be run on Linux, Solaris, and FreeBSD.

**In this blog ..** You will see LoadBalancer on top of AWS cloud and Automate the ** LoadBalancer** With the help of **Ansible Playbook** so We need to Only **one click** and Entire Setup will Automatic Generated ….

Lets, make two instances on AWS Cloud One Instance for Load balancer and another for Backend webserver .

![image](https://user-images.githubusercontent.com/70094412/111815125-4e814780-8901-11eb-9653-68b48e22a54a.png)

We can make this setup by the manually , but it can take More time so In this situation we use Ansible-Playbook Which make Entire setup in one-click .

**First we need to install ansible in AWs Cloud .**

**Step 1:** Ansible work on Python So first install Python .

**step 2:** the pip install ansible .

```
pip install ansible
```

**step 3:** before install ansible some dependencies require in AWS Instances ,
For this install below rpm file .
```
# yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm 
```
If you have redhat verion ≥8 then install below rpm file


```
# yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```

**step 4:** Now see ansible version

![image](https://user-images.githubusercontent.com/70094412/111815800-175f6600-8902-11eb-8715-66f53a703fb5.png)

**Note :** If you have ansible **config file=None** then ,

create the **directory /etc/ansible/ansible.cfg**  , so Ansible automatic fetch this path for config. path .

**Step 5:** Now Make ansible-playbook …

haprooxy.yml
```
- hosts: webserver
  become: yes
  tasks:
  - name: "installing webserver..."
    package:
      name: "httpd"
      state: present
  - name: "maing website ..."
    copy:
      dest: "/var/www/html"
      src: "/root/index.html"
  - name: "start server"
    service:
      name: "httpd"
      state: started

- hosts: loadbalancer
  become: yes
  tasks:
  - name: "installing haproxy servr ...."
    package:
      name: "haproxy"
      state: present
  - name: "making dynamic haproxy consguration file"
    template:
      dest: "/etc/haproxy/haproxy.cfg"
      src: "/root/haproxy.cfg"
  - name: "restarting haproxy server"
    service:
      name: haproxy
      state: restarted
```

index.html
```
<html>

<head>
    <style>
        img {
            display: block;
            margin-left: auto;
            margin-right: auto;
        }
    </style>
</head>

<body style="background-color: pink;">
    <h1>This is demonstartion of AWS ec2+ebs+cloud front +s3 bucket </h1>
    <pre>
        Webserver configured on EC2 Instance
        Document Root(/var/www/html) made persistent by mounting on EBS Block Device.
        Static objects used in code such as pictures stored in S3
        Setting up Content Delivery Network using CloudFront and using the origin domain as S3 bucket.
        Finally place the Cloud Front URL on the webapp code for security and low latency.
     </pre>
    <img src="C:\Users\PARTH\Desktop\s3image.jpg" hight="800px" width="800px">
</body>

</html>
```


hear one hosts — webserver will configure the webserver with respected ip’s and —loadbalancer for configure the loadbalancer with respected ip’s . ip’s are writen in **Inventory file .**

![image](https://user-images.githubusercontent.com/70094412/111816229-9a80bc00-8902-11eb-9e3a-d58645354b1f.png)


In this Playbook Index.html Which is Our website And this Run in web server and top of webserver we have Load Balancer So , If we want access the website We need to use Only LoadBalancer IP:Port , SO we can make Our Website Secure.

Furthermore , If Many Request come on LoadBalancer we can make Many webserver behind LoadBalancer So we can Balnce Load .

For making dynamic Haproxy.cfg File We need to use **Jinja code in haproxy.cfg** , bcz if We add one ip in Inventory then automaticlly attach with LoadBalancer 

![image](https://user-images.githubusercontent.com/70094412/111816289-aa000500-8902-11eb-8b6a-8acab1c0dee0.png)


**Now Run the Playbook**

![image](https://user-images.githubusercontent.com/70094412/111816308-b1271300-8902-11eb-9cdd-eace64695976.png)


![image](https://user-images.githubusercontent.com/70094412/111816318-b4220380-8902-11eb-81d7-5f9ef10bb666.png)


Here Load Balancer IP is : 65.0.18.90

and webserver ip is : 13.233.208.243

SO access the Website use Loadbalancer IP:Port

![image](https://user-images.githubusercontent.com/70094412/111816386-cc921e00-8902-11eb-85bc-ad8ea8e65bc8.png)


# ✨Now successfully run Website with LoadBalancer….✨
