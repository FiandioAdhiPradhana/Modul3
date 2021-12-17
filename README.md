# **Module 3 Lab Report**

## **Reksa Aldian R (1202192048) & Fiandio Adhi P (1202192034)**

### **Study Case**

Create SubDomain dev.vm.local with rules :

* Ansible
* Same lxc like vm.local
* The folder must be in var/www/html/dev{name_app}



### **Problem Solving**

* The first step is to go to the directory using ansible

![1 (1)](https://user-images.githubusercontent.com/93086665/146562978-887ec503-27e9-45b6-b787-38341f6145e7.JPG)


  ```
  ---
  - hosts: all
    become : yes
    tasks:
      - name: install bind9 dan dnsutils
        apt:
         pkg:
           - bind9
           - dnsutils
  ```
 ![2 (1)](https://user-images.githubusercontent.com/93086665/146563320-7db72df7-97b4-42ed-ba5b-d3e22d94716d.JPG)



* The next step is install packages with ansible


![3 (1)](https://user-images.githubusercontent.com/93086665/146563468-b3634a83-160c-4562-a505-051d53fa9d87.JPG)


* Create e config.yml file

![4 (1)](https://user-images.githubusercontent.com/93086665/146563634-c89f2c18-7ef2-407e-a67b-90ae8030a506.JPG)

  ```
  ---
  - hosts: all
    become : yes
    tasks:
     - name: membuat direktori
       file:
        path: /var/www/html/dev/landing
        state: directory
     - name: copy file vm.local
       copy:
        src: /etc/bind/vm/vm.local
        dest: /var/www/html/dev/landing
     - name: mengganti konfigurasi
       replace:
        path: /var/www/html/dev/landing/vm.local
        regexp: 'www'
        replace: 'dev'
     - name: copy file named.conf.local
       copy:
        src: /etc/bind/named.conf.local
        dest: /etc/bind/named.conf.local
     - name: mengganti konfigurasi conf local
       replace:
        path: /etc/bind/named.conf.local
        regexp: '/etc/bind/vm/vm.local'
        replace: '/var/www/html/dev/landing/vm.local'
     - name: mengganti konfigurasi conf local part2
       replace:
        path: /etc/bind/named.conf.local
        regexp: '/etc/bind/vm/1.168.192.in-addr.arpa'
        replace: '/var/www/html/dev/landing/1.168.192.in-addr.arpa'
     - name: copy file 1.168.192.in-addr.arpa
       copy:
        src: /etc/bind/vm/1.168.192.in-addr.arpa
        dest: /var/www/html/dev/landing
     - name: copy file resolv.conf
       copy:
        src: /etc/resolv.conf
        dest: /etc/resolv.conf
     - name: copy file named.conf.options
       copy:
        src: /etc/bind/named.conf.options
        dest: /etc/bind/named.conf.options
     - name: restart nginx
       service:
        name: nginx
        state: restarted
     - name: restart bind9
       action: service name=bind9 state=restarted
  ```

* Do the installation using the script below

![5 (1)](https://user-images.githubusercontent.com/93086665/146563842-1a3adb3f-0eeb-4e22-8a34-0b6ba4d08909.JPG)

  
* Add subdomain to /etc/hosts

![6 (1)](https://user-images.githubusercontent.com/93086665/146564065-53483b8a-f3aa-4199-b2de-994f72e7d860.JPG)

![7](https://user-images.githubusercontent.com/93086665/146564173-60a29fd6-c591-439b-9acd-ede1979cbaa1.JPG)

* Open vm.local file

 ![8](https://user-images.githubusercontent.com/93086665/146564298-c82f06d9-ce4d-4c8d-bda6-d9e5adb8041d.JPG)


* Add line www. After that exit lxc

 ![9](https://user-images.githubusercontent.com/93086665/146564392-e40c903a-9a63-4d16-83fa-61bd5298c4fa.JPG)
 

* Open and edit vm.local in directory /etc/nginx/sites-enabled/

 ![10](https://user-images.githubusercontent.com/93086665/146564504-da9f4e95-fefc-46fe-b8d1-1c2aec246e4e.JPG)
 ![11](https://user-images.githubusercontent.com/93086665/146564568-9e6be606-8ce4-4d34-b949-0a1be8d1145f.JPG)
 
* Open and edit vm.local in directory /etc/bind/vm/

 ![12](https://user-images.githubusercontent.com/93086665/146564690-1abcc549-b227-4e3b-a2fa-ec9cac21e640.JPG)

* Restart all packages

  ![13](https://user-images.githubusercontent.com/93086665/146564747-2196bb45-ae44-4d7d-9568-b5ed37e11bcc.JPG)

- Open Wifi Settings (in the case we use linux os), add dns server and uncheck automatic at menu Ipv4
 
 ![Screenshot_1](https://user-images.githubusercontent.com/93086665/146565222-f1f6650a-d373-4346-b9a4-00abb8035a43.png)

- At menu IPv6, unchecklist

- Just reconnecting the wifi, and check dev.vm.local on the browser :)

 ![14](https://user-images.githubusercontent.com/93086665/146565310-a28460f0-7c1b-476b-b3ad-d3968c38a6ee.JPG)
