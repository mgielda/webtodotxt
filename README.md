# Web Todo.txt

A web-based GUI to manage a [Todo.txt](http://todotxt.com/) file.

<p align="center">
  <img src="https://github.com/EpocDotFr/webtodotxt/raw/master/screenshot.png">
</p>

## Features

  - Add / edit / remove tasks
  - Tasks due date (WIP)
  - Filters on all possible task data
  - Projects and contexts are cached locally for future use (using [localStorage](https://en.wikipedia.org/wiki/Web_storage#localStorage)) (WIP)
  - Automatic sorting
  - Save and reload the task list
  - Clear displaying of the task priority and completion
  - Automatic task creation date and completion date setting
  - Links are automatically created for URLs and email addresses
  - Possible to integrate with other system using a very basic "API" (I put API in quotes because it isn't really an API). See below for more information
  - Warns before quitting if your task list wasn't saved to prevent data loss
  - Support 2 storage backends (see below in the **Supported storage backends** section for the list)
  - Internationalized & localized in 3 languages:
    - English (`en`)
    - French (`fr`)
    - Portuguese (WIP) (`pt`)

## Prerequisites

  - Should work on any Python 3.x version. Feel free to test with another Python version and give me feedback
  - A [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/)-capable web server (optional, but recommended)
  - A modern web browser (which optionally support localStorage)

## Installation

  1. Clone this repo somewhere
  2. `pip install -r requirements.txt`
  3. `pybabel compile -d translations`
  4. **IMPORTANT:** Other dependencies are needed regarding the storage backend you'll use. Please refer to the table in the **Supported storage backends** section below and install them accordingly using `pip install <package>` before continuing

## Configuration

Copy the `config.example.py` file to `config.py` and fill in the configuration parameters.

Available configuration parameters are:

  - `SECRET_KEY` Set this to a complex random value
  - `DEBUG` Enable/disable debug mode

More informations on the three above can be found [here](http://flask.pocoo.org/docs/0.12/config/#builtin-configuration-values).

  - `TITLE` If set to a string, will be used to replace the default app title (which is "Web Todo.txt")
  - `USERS` The credentials required to access the app. You can specify multiple ones. **It is highly recommended to serve Web Todo.txt through HTTPS** because it uses [HTTP basic auth](https://en.wikipedia.org/wiki/Basic_access_authentication)
  - `FORCE_LANGUAGE` Force the lang to be one of the supported ones (defaults to `None`: auto-detection from the `Accept-Language` HTTP header). See in the features section above for a list of available lang keys
  - `DEFAULT_LANGUAGE` Default language if it cannot be determined automatically. Not taken into account if `FORCE_LANGUAGE` is defined. See in the features section above for a list of available lang keys
  - `DISPLAY_CREATION_DATE` Whether the creation date of the tasks must be displayed or not
  - `STORAGE_BACKEND_TO_USE` The storage backend to use. Can be one of the ones in the table below, in the **Supported storage backends** section
  - `STORAGE_BACKENDS` Self-explanatory storage backends-specific configuration values. Don't forget to change them before using your desired storage backend

I'll let you search yourself about how to configure a web server along uWSGI.

## Usage

  - Standalone

Run the internal web server, which will be accessible at `http://localhost:8080`:

```
python local.py
```

Edit this file and change the interface/port as needed.

  - uWSGI

The uWSGI file you'll have to set in your uWSGI configuration is `uwsgi.py`. The callable is `app`.

  - Others

You'll probably have to hack with this application to make it work with one of the solutions described [here](http://flask.pocoo.org/docs/0.12/deploying/). Send me a pull request if you make it work.

## How it works

This project is built on [Vue.js](http://vuejs.org/) 2 for the frontend and [Flask](http://flask.pocoo.org/) (Python) for
the backend. The [todotxtio](https://github.com/EpocDotFr/todotxtio) PyPI package is used to parse/write the Todo.txt file,
giving/receiving data through [Ajax](https://en.wikipedia.org/wiki/Ajax_(programming)). Several storage backends
are available so one can choose to save its Todo.txt file locally on the filesystem, on its Dropbox or in a WebDav instance
like nextcloud..

## "API"

Please navigate [here](https://github.com/EpocDotFr/webtodotxt/blob/master/api.md) for the full docs.

## Gotchas

  - Be aware that this web application isn't intended to be used on mobile devices

Instead, use native mobile apps to edit and sync the Todo.txt file:

**On Android**, you can use [Simpletask](https://play.google.com/store/apps/details?id=nl.mpcjanssen.todotxtholo&hl=en)
(free) which can natively sync your tasks with your Dropbox and Nextcloud. If you're using another storage provider (third-party or
self-hosted), you can use a modified version of this app called [Simpletask Cloudless](https://play.google.com/store/apps/details?id=nl.mpcjanssen.simpletask&hl=en)
(also free, from the same author) which comes with no sync at all, but instead saves all your tasks in a file on your
device. You can then do whatever you want with this file like syncing it via SFTP or many other providers / protocols
with [FolderSync](https://play.google.com/store/apps/details?id=dk.tacit.android.foldersync.lite&hl=en) (free, but a
[pro version](https://play.google.com/store/apps/details?id=dk.tacit.android.foldersync.full&hl=en) is available).

**On iOS**, I don't know. Feel free to share your finds.

  - This web application wasn't designed to be multi-process compliant

If you sync your Todo.txt file via Dropbox or something from the mobile apps and at the same time you're modifiying it via
this web app, you'll probably end with a loss of data because both sides can't be aware of the latest version of the file
in realtime: they both erase the file with their data.

So make sure you're modifying it from one location at a time with the latest up-to-date Todo.txt file.

## Supported storage backends

| Name | Configuration value | Additional PyPI dependencies |
|------|---------------------|------------------------------|
| Local file system | `FileSystem` |  |
| [Dropbox](https://www.dropbox.com/) | `Dropbox` | `dropbox` |
| WebDAV | `WebDav` | `webdavclient3` |

## Contributors

Thanks to:

  - [@Pepsit36](https://github.com/Pepsit36) (Portuguese translations)

## End words

If you have questions or problems, you can [submit an issue](https://github.com/EpocDotFr/webtodotxt/issues).

You can also submit pull requests. It's open-source man!
