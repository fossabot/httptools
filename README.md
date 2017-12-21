Package httptools tries to augment the basic net/http package with
functionality found in webframeworks without breaking the original API.

For details and examples, please see the [documentation](http://godoc.org/github.com/surma/httptools).

[![Build Status](https://drone.io/github.com/surma/httptools/status.png)](https://drone.io/github.com/surma/httptools/latest)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Faboutsource-test%2Fhttptools.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Faboutsource-test%2Fhttptools?ref=badge_shield)

## Contrived example

```Go
r := httptools.NewRegexpSwitch(map[string]http.Handler{
	"/people/(.+)": httptools.L{
		httptools.SilentHandler(AuthenticationHandler),
		httptools.MethodSwitch{
			"GET": ListPeople,
			"PUT": http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
				vars := w.(httptools.VarsResponseWriter).Vars()
				AddNewPerson(vars["1"])
			})
		},
		SaveSessionHandler,
	},
	"/.+": http.FileServer(http.Dir("./static")),
})
http.ListenAndServe("localhost:8080", r)
```

## Tools
httptools provides the following tools:
### Handler list
Define a sequence of `http.Handler`. One will be executed after another. A
customized `http.ResponseWriter` allows the passing of data in between handlers.
### Silent handler
If a silent handler produces output, it is assumed to be an error. If the
silent handler is in a handler list, the execution of the list will be aborted.
### Switches
#### Method switch
Dispatch requests to different handlers according the the HTTP verb used
in the request.
#### RegexpSwitch
Dispatch requests to different handlers according to regexps being matched
agains the request path.
#### HostnameSwitch
Dispatch requests to different handlers according to the hostname used
in the request.
### Mounts
Dispatch requests to different handlers according to path prefixes. The
path prefix will be stripped from the request before being passed to the
handler.

---
Version 1.2.1


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Faboutsource-test%2Fhttptools.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Faboutsource-test%2Fhttptools?ref=badge_large)