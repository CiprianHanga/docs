## Homestead

Homestead runs within a Virtual Machine. It comes with the following software installed:
- Ubuntu 16.01
- Git
- PHP 7.0
- HHVM
- Nginx
- MySQL
- Sqlite3
- Postgres
- Composer
- Node (with PM2, Bower, Grunt, and Gulp)
- Redis
- Memcached
- Beanstalkd

> TIP: 
> [install phpMyAdmin](http://stackoverflow.com/questions/23788096/how-to-setup-phpmyadmin-on-a-laravel-homestead-box)
> user:password for phpmyadmin: homestead:secret
> [Youtube](https://www.youtube.com/watch?v=q5ESL5MxmzA)
> [Spanish, Youtube](https://www.youtube.com/watch?v=f0sEkkEfhJI)


### Using the Laravel/Homestead box from Sitepoint

- first you have to download it:

```bash
$ git clone https://github.com/swader/homestead_improved my_project
```

- *Note:* change my_project to what you want
- then run:

```bash
$ cd my_project
$ mkdir -p Project/public
$ bin/folderfix.sh # optional, but advised
```

- the last command makes the Project/public the shared folder between the host and the VM

#### Set Up the Host File

- you need to add a few settings to your ```/etc/hosts``` file
- make homestead.app to be seen as ```192.168.10.10``` address by adding this line in the ```/etc/hosts``` file:

```
129.168.10.10	homestead.app
```

- *Note:* for every project/app that you want to use with this VM and want to have distinctive addresses, add more aliases like this:

```
192.168.10.10	test1.app
192.168.10.10	test2.app
192.168.10.10	test3.app
```

- it just makes it easier to call those apps

