# AirBnB_clone_v2: MySQL, deploy web static, web framework

## Table of Contents

* [Description](#description)
* [Purpose](#purpose)
* [Requirements](#requirements)
* [File Descriptions](#file-descriptions)
* [Environmental Variables](#environmental-variables)
* [Usage](#usage)
* [Bugs](#bugs)
* [Authors](#authors)
* [License](#license)

## Description

### Part 1: Add a MySQL database storage system
![mysql_diagram](https://s3.amazonaws.com/intranet-projects-files/concepts/74/hbnb_step2.png)

### Part 2: Deploy web static with Fabric
![web_static_deployment](https://s3.amazonaws.com/intranet-projects-files/alx-higher-level_programming+/288/aribnb_diagram_0.jpg?cache=off)

### Part 3: Build a web framework with Flask
![webframwork](https://s3.amazonaws.com/intranet-projects-files/concepts/74/hbnb_step3.png)

**Links to other versions:**
* [AirBnB_clone_v1: Console and web static](https://github.com/johngit23/AirBnB_clone_v1)
* [AirBnB_clone_v3: RESTful API](https://github.com/johngit23/AirBnB_clone_v3)

## Purpose
The purpose of Phase 2 is to learn how to:
* create a MySQL database
* use ORM
* map a Python class to a MySQL table
* handle 2 different storage engines in same codebase
* use environmental variables
* deploy code on a server
* use Fabric for executing local or remote shell commands, uploading/downloading files, prompting the running user for input, or aborting execution. Fabric is taking care of all network connections (SSH, SCP etc.): it's an easy tool for transferring files and executing commands from local to a remote server.
* manage Nginx configurations
* build a web framework with Flask
* define routes in Flask
* create HTML response in Flask using a template
* create dynamic template with Jinja2
* display data from MySQL database in HTML

## Requirements
* All files compiled with Ubuntu 14.04 LTS
* Documentation
* Organized files in proper folders (i.e. CSS files should be in `styles` folder)
* Python unit tests for all files
* Use SQLAlchemy 1.2.x and MySQL 5.7
* All files must be pep8 and shellcheck compliant

## File Descriptions
  **Note:** Below highlights only new file additions for Phase 2. For Phase 1 file descriptions, click [here](https://github.com/johngit23/AirBnB_clone_v1).
  * [console.py](console.py) - command interpreter from Phase 1 that includes updated `do_create` function that allows object creation with given arameters with syntax `<key name>=<value>`
  * [setup_mysql_dev.sql](setup_mysql_dev.sql) - script that prepares a MySQL development server for the project
  * [setup_mysql_test.sql](setup_mysql_test.sql) - script that prepares a MySQL test server for the project
  * [0-setup_web_static.sh](0-setup_web_static.sh) - Bash script that sets up web servers for `web_static` deployment
  * [1-pack_web_static.py](1-pack_web_static.py) - a Fabric script that generates a .tgz archive from the contents of the `web_static` folder
    * `do_pack` - generates a .tgz archive from the contents of the `web_static` folder using Fabric
  * [2-do_deploy_web_static.py](2-do_deploy_web_static.py) - a Fabric script (based on 1-pack_web_static.py) that distributes an archive to web servers
    * `do_deploy` - distributes an archive to web servers
    * `do_pack` - generates a .tgz archive from the contents of the `web_static` folder using Fabric
  * [3-deploy_web_static.py](3-deploy_web_static.py) - a Fabric script (based on 2-do_deploy_web_static.py) that creates and distributes an archive to my web servers 
    * `deploy` - creates and distributes an archive to web servers
    * `do_deploy` - distributes an archive to web servers
    * `do_pack` - generates a .tgz archive from the contents of the `web_static` folder using Fabric
  * [models](models) - contains models for relevant AirBnB objects
    * [`__init__.py`](models/__init__.py) - switch to file storage or database storage modes
    * [base_model.py](models/base_model.py) - class BaseModel, parent class that will take care of initialization/serialization/deserialization of future instances
      * `__init__` - initialize instance attributes
      * `__str__` - returns formatted string representation of instance
      * `__repr__` - returns string representation of instance
      * save - updates `updated_at` attribute for new instance
      * to_dict - returns dictionary representation a BaseModel object
    * [user.py](models/user.py) - class User
    * [city.py](models/city.py) - class City
    * [state.py](models/state.py) - class State
    * [place.py](models/place.py) - class Place
      * `reviews` - get list of Review instances with place_id (equals current Place.id)
      * `amenities` getter - returns list of Amenity instances based on the attribute amenity_ids that contains all Amenity.id linked to the Place
      * `amenities` setter - adds an Amenity.id to attribute amenity_ids if obj is an instance of Amenity
    * [review.py](models/review.py) - class Review
    * [amenity.py](models/amenity.py) - class Amenity
  * [tests](/tests/) - unit test files
  * [engine](models/engine) - contains storage engines
    * [`__init__.py`](/models/engine/__init__.py) - empty `__init__.py` file for packages
    * [file_storage.py](/models/engine/file_storage.py) - class FileStorage; serializes instances to JSON file and deserializes from a JSON file
      * `all` - returns the dictionary `__objects`
      * `new` - sets in `__objects` the obj with key `<obj class name>.id`
      * `save` - serializes `__objects` to the JSON file (path: `__file_path`)
      * `reload` - deserializes the JSON file to `__objects`
      * `delete` - delete object from `__objects` if exists
      * `close` - call reload
    * [db_storage.py](/models/engine/db_storage.py) - class DBStorage; 
      * `__init__` - initalize instances
      * `all` - return dictionary of instance attributes
      * `new` - add new object to current database session
      * `save` - commit all changes of the current database session
      * `delete` - delete from the current database session obj if not None
      * `reload` - create all tables in database and current database session
      * `close` - close session
## Environmental Variables
```
HBNB_ENV: running environment. It can be “dev” or “test” for the moment (“production” soon!)
HBNB_MYSQL_USER: the username of your MySQL
HBNB_MYSQL_PWD: the password of your MySQL
HBNB_MYSQL_HOST: the hostname of your MySQL
HBNB_MYSQL_DB: the database name of your MySQL
HBNB_TYPE_STORAGE: the type of storage used. It can be “file” (using FileStorage) or db (using DBStorage)
```

## Usage
Run the following in your terminal:
```
user@ubuntu:~/AirBnB_v2$ curl -o 100-dump.sql "https://s3.amazonaws.com/intranet-projects-files/alx-higher-level_programming+/290/100-hbnb.sql"
user@ubuntu:~/AirBnB_v2$ cat 100-dump.sql | mysql -uroot -p
Enter password: 
user@ubuntu:~/AirBnB_v2$ HBNB_MYSQL_USER=hbnb_dev HBNB_MYSQL_PWD=hbnb_dev_pwd HBNB_MYSQL_HOST=localhost HBNB_MYSQL_DB=hbnb_dev_db HBNB_TYPE_STORAGE=db python3 -m web_flask.100-hbnb
* Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
....
```
Open up a web browser and type `0.0.0.0:5000/hbnb`.
  
## Bugs

At this time, there are no known bugs.

## License

**hbnb** is open source and free to download and use
