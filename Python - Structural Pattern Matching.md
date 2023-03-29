#python #advancedconcepts

The equivalent to [[JavaScript - Switch statements]], added in Python 3.10.
For the full docs on this relatively new feature, see [PEP 622](https://peps.python.org/pep-0622/).

Basic syntax is:
```
def http_error(status):
	match status:
		case 400:
			return "Bad request"
		case 404:
			return "Not found"
		case 418:
			return "I am a teapot"
		case _:
			return "Somethings wrong with the internet"
```
The underscore case is always the default case, and will only execute in the instance that no exact match is found between the test statement and any of the other cases.