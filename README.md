# Pisocasas 

## Description
Scraping property details from https://www.pisocasas.com/ and store it in Postgresql database.


## Setup Environment Variables
In `settings.py` add the following configuration:
1. Database connection
    ```
    DATABASE = {
        'drivername': 'postgres',
        'host': os.environ.get('DB_HOST', 'localhost'),
        'port': os.environ.get('DB_PORT', '5432'),
        'username': os.environ.get('DB_USERNAME', 'user'),
        'password': os.environ.get('DB_PASSWORD', 'pwd'),
        'database': os.environ.get('DB_NAME', 'mydb')
    }
    ```
2. Database pipeline
    ```
    ITEM_PIPELINES = {
        'pisocasas.pipelines.PostgresDBPipeline': 330
    }
    ```

3. Scrapy Splash settings
    ```buildoutcfg
    SPLASH_URL = 'http://localhost:8050'
 
    DOWNLOADER_MIDDLEWARES = {
        'scrapy_splash.SplashCookiesMiddleware': 723,
        'scrapy_splash.SplashMiddleware': 725,
        'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware': 810,
    }
 
    SPIDER_MIDDLEWARES = {
        'scrapy_splash.SplashDeduplicateArgsMiddleware': 100,
    }
 
    DUPEFILTER_CLASS = 'scrapy_splash.SplashAwareDupeFilter'
    
    HTTPCACHE_STORAGE = 'scrapy_splash.SplashAwareFSCacheStorage'
    ```

## Dependencies
1. Install the following dependencies from requirements.txt
    `pip install -r requirements.txt`
    
    ```buildoutcfg
    sqlalchemy
    psycopg2
    scrapy_splash
    ```

## Create eggfile
1. Create `setup.py` file at the same level as `scrapy.cfg` file with content as:
    ```
    from setuptools import setup, find_packages
    setup(
        name='pisocasas',
        version='1.0',
        packages=find_packages(),
        install_requires=[
            'psycopg2',
            'sqlalchemy'
            'scrapy_splash'
        ],
        entry_points={'scrapy': ['settings = pisocasas.settings']}
    )
    ```
    
2. Execute `python setup.py bdist_egg` in folder at the same level as `scrapy.cfg` file
3. Upload the eggfile into the scrapyd server using: `curl http://localhost:6800/addversion.json -F project=pisocasas -F version=1.0 -F egg=@dist/pisocasas-1.0-py3.7.egg`
