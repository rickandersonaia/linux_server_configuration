# linux_server_configuration
This is the final project in the Udacity Full Stack Web Developer Nanodegree program

## Connection Details
* Server IP address: 35.166.241.57
* SSH Login:  ssh -i ~/.ssh/udacity.rsa grader@35.166.241.57
* Site URL:  https://ourwinejournal.com

## Summary

This application is hosted on an Amazon AWS Lightsail instance

**Primary software used:**
* Ubuntu 16.04 LTS 
* Apache  
* Python 3.5 
* PostgreSQL
* mod_wsgi
* vituralenv.
* Flask

### Challenges
__Which version of Python? -__  The biggest challenge came from the fact that I wrote the application in Python 3.5 but the server installation makes Python 2.7 the default version of Python.  This lead me to install the wrong modules and overlook changes in configuration that are required for Python 3.5.

__The crux - Installing Apache & WSGI -__  In order for Apache, WSGI and Python 3.5 to work together there are a number of pieces of the puzzle that have to be right.  First I needed to install `apache2-dev` and `libapache2-mod-wsgi-py3`.  Next I needed to install `virtualenv` because the standard `venv` for Python 3 doesn’t create the needed `activate-this.py` file.  From here I had to force `virtualenv` to create a Python 3.5 instance using the `--python=python3.5` flag.  Finally, once the virtual environment was activated I had to `pip install mod_wsgi`.

__Environment Variables -__  In order protect secret data during development I used a combination of environment variables, instance settings and config settings.  For production I got rid of all of the `os.environ `calls.  I placed all secret variables in `instance/settings.py` and the rest in `config/settings.py`

__Removing development only files -__  I had created a CLI for this app that allowed me to rapidly replace my database and re-populate it with default data.  It also provided unit and coverage testing.  That stuff got in the way so I removed it.


__Adding SSL -__  I used Let’s Encrypt’s `certbot` to to create and install a certificate.  Unfortunately it couldn’t properly update the `winejournal.conf` file.  I ultimately resolved this by manually adding an additional virtual host entry in that file.


## 3rd Party Resources

* The most important resource was my starting point - Michael Mulligan’s https://github.com/mulligan121/Udacity-Linux-Configuration.  Many of his instructions weren’t appropriate for my situation, but he did provide an excellent point of reference.

* Central to solving my crux problem was Shawn Niederriter’s video How to Run A Flask App in A Virtual Environment with Python 3.6 & Ubuntu - https://www.youtube.com/watch?v=LREFHGXlXfE

* https://stackoverflow.com/questions/44914961/install-mod-wsgi-on-ubuntu-with-python-3-6-apache-2-4-and-django-1-11 - While the answer wasn’t entirely correct in my case, it was an important starting point for experimentation

* https://stackoverflow.com/questions/25020451/no-activate-this-py-file-in-venv-pyvenv helped me switch to virtualenv

* I had to keep coming back to http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/ and http://modwsgi.readthedocs.io/en/develop/user-guides/quick-configuration-guide.html

* Finally - to solve the ssl issue - https://stackoverflow.com/questions/32812570/configure-ssl-certificate-on-apache-for-django-application-mod-wsgi

