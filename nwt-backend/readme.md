## Installation

1. Install [Python 3.9](https://www.python.org/downloads/release/python-390/).
1. In terminal change current working directory to the base directory of this file.
1. (Optional) Initialize virtual environment and activate it according to the
   [tutorial](https://docs.python.org/3/library/venv.html).
1. [Update pip](https://pip.pypa.io/en/stable/installing/#upgrading-pip).
    - On Linux and macOS run `pip3.9 install -U pip`.
    - On Windows run `python -m pip install -U pip`.
1. Run `pip3.9 install -U setuptools wheel`. This will update setuptools and wheel packages.
1. Run `pip3.9 install -r requirements.txt`. This will install all necessary packages for the project.
1. Create `backend/secret_key.py` file and put secret key value inside:

    ```python
    KEY = 'your_secret_key'
    ```

1. Run `python manage.py makemigrations`.
1. Run `python manage.py migrate`.
1. Allow incoming TCP requests for port 8000
    - On Ubuntu run `ufw allow 8000/tcp`.

## Usage

### Starting server

```
python manage.py runserver 0:8000
```

### Creating superuser

```
python manage.py createsuperuser
```

## API

### login/

#### POST

Request with POST fields `username` and `password`.

- No such user: 400 Bad Request.
- Otherwise: 302 Redirect to **home/**.

### logout/

#### GET

- If authenticated: 302 Redirect to **home/**.
- Otherwise: 401 Unauthorized.

### api-total/

#### GET

- If authenticated: 200 OK with JSON.

    ```json
    {
      "asset": 100,
      "liability": 24
    }
    ```

- Otherwise: 401 Unauthorized.

### api-records/

#### GET

- If authenticated: 200 OK with JSON.

    ```json
    [
      {
        "date_time": "2020-12-05T16:09:52Z",
        "name": "Milk",
        "type": "liability",
        "amount": 10
      },
      {
        "date_time": "2020-12-05T16:10:15Z",
        "name": "Bread",
        "type": "liability",
        "amount": 14
      },
      {
        "date_time": "2020-12-05T16:10:35Z",
        "name": "Stipend",
        "type": "asset",
        "amount": 100
      }
    ]
    ```

- Otherwise: 401 Unauthorized.

#### POST

Request with key-value POST data as shown below:

```
{
  "date_time": "2020-12-05T16:10:35Z",
  "name"     : "Stipend",  // any string with 200 max chars
  "type"     : "asset",    // "asset", "liability", 1, 2
  "amount"   : 100         // any non-negative integer
}
```

- If authenticated and data is valid: 200 OK.
- If authenticated and data is not valid: 400 Bad Request.
- Otherwise: 401 Unauthorized.

## Database Structure

![UML](UML-of-DB.png)
