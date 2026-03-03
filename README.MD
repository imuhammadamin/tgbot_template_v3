This template is used for developing Telegram bots using the [`aiogram v3.0+`](https://github.com/aiogram/aiogram/tree/dev-3.x) library.

## SQLAlchemy + Alembic
The code contains examples of a User table using SQLAlchemy 2.0, and scripts for Alembic (alembic initialization, creating and applying migrations).

If you have never worked with these tools before, please refer to the documentation to learn about them. 
Also, I have an English [course on these tools on Udemy](https://www.udemy.com/course/sqlalchemy-alembic-bootcamp/?referralCode=E9099C5B5109EB747126).

![img.png](https://img-c.udemycdn.com/course/240x135/5320614_a8af_2.jpg)

### To start using it:
1. Copy `.env.dist` to `.env` and fill in the necessary data.
2. Create new handlers.
3. **Docker:**
   1. You can immediately run the project with Docker. If you don't have it, [download and install it](https://docs.docker.com/get-docker/).
   2. Run the project with the command `docker-compose up`
4. **Without Docker:**
   1. Create a [venv](https://docs.python.org/3/library/venv.html)
   2. Install dependencies from requirements.txt: `pip install -r requirements.txt --pre`
   3. Run the project with the command `python3 bot.py`


### How to create and register handlers:
Create a module `your_name.py` in the `handlers` folder.

Create a router in `your_name.py`.
```python
from aiogram import Router
user_router = Router()
```
You can create multiple routers in one module and attach handlers to each of them.
You can register handlers using decorators:
```python
@user_router.message(commands=["start"])
async def user_start(message):
    await message.reply("Hello, regular user!")
```

Go to the `handlers/__init__.py` file and add all routers to it:
```python
from .admin import admin_router
from .echo import echo_router
from .user import user_router

...


routers_list = [
    admin_router,
    user_router,
    echo_router,  # echo_router must be last
]

```
### How to add handlers to our bot:
Go to the `bot.py` file and unpack our handlers:
```python
from tgbot.handlers import routers_list

...

async def main():
   
    ...

   dp.include_routers(*routers_list)

    ...


```

### How to start the Database and run your first migration:
1. Go to the `.env` file and fill in the data for the database, if you haven't done so already.

2. Go to the `docker-compose.yml` file and uncomment the sections: `api`, `pg_database`, and `volumes` to get started.

3. Go to `config.py` and implement the `TODO` in the `construct_sqlalchemy_url` function. Also, find the section of the `load_config` function and uncomment the line that corresponds to the database configuration initialization `db=DbConfig.from_env(env)` to activate the database connection.

4. Now we can restart Docker with the new containers using the command:

    `docker-compose up --build`

5. All set! Now we can run the migration!
Open the terminal and enter the following command:

    `docker-compose exec api alembic upgrade head`

### Tutorials on aiogram v3
There are no videos yet, but @Groosha has already started creating [his own guide](https://mastergroosha.github.io/aiogram-3-guide).
