---

title: Using Figwheel with Docker

layout: post

---

One of the things I've been using a lot lately is Docker. I don't pretend to be an expert, or even be very enthusiastic about it, but there's a lot of interesting tooling being built up around it. docker-compose lets you put together large-ish systems using a configuration file, and deploy those configuration files to remote clusters. Working within Docker also means that your development environment closely resembles production, which is a boon.

The other thing I've been playing with in my spare time is Figwheel. The ability to live-code user interfaces without losing application state is incredible, and being able to force the application into particular states makes it really easy to test unusual edge cases.

Using Docker and Figwheel together is a bit fiddly, as I have discovered, so I've written down the magic incantations to save others (and my future self) the time it takes to figure it out.

It should be easy to retrofit these changes onto an existing project, but I'm going to start with a fresh project here, becasue I think it will illustrate the changes that need to happen more clearly: 

```sh
lein new figwheel dockerwheel
```

Now that we have a new project called `dockerwheel`, we need a Dockerfile. This will be our recipe for plugging into the Docker infrastructure:

```
FROM clojure

WORKDIR /clj
ADD . /clj
VOLUME /clj

# 3449 is default http and websocket port that figwheel uses to communicate
EXPOSE 3449
# 7888 is the default nrepl port
EXPOSE 7888

RUN lein deps
RUN lein cljsbuild once
CMD lein figwheel
# This command will start figwheel and open a repl socket
# This would need to change for a live environment -- you wouldn't want
# the figwheel socket open in production, but I'll leave that bit as an
# exercise for the reader.
```

There are also some changes that need to be made to the `project.clj` configs to get the hostnames/IPs to line up for the Dokcer environment:

```clj
;; project.clj
:cljsbuild {
  :builds [{:id "dev"
            :source-paths ["src"]
            :figwheel { :on-jsload "dockerwheel.core/on-js-reload"
                        :websocket-host "192.168.99.100"} ;; <--
                        ;; This IP address needs to be the IP to your docker
                        ;; container, otherwise the websocket will attempt to
                        ;; connect to localhost:3449

;; AND:                        
:figwheel {:css-dirs ["resources/public/css"] ;; leave this bit alone
           :nrepl-host  "0.0.0.0"
           ;; nrepl only accepts connections from certain IP addresses
           ;; as a secuity measure. Setting this to "0.0.0.0" lets incoming
           ;; REPL requests access it from any IP address.
           :nrepl-port  7888
           ;; Nrepl doesn't start if you don't give it a port in the configs
           :repl        false }
           ;; Finally, don't start a REPL when the container is started,
           ;; instead, we want to allow remote connections
```

Now you can build and start the container:

```bash
docker build -f Dockerfile -t dockerwheel .
# this builds the container, it can take a while to pull all the dependencies
docker run -p 3449:3449 -p 7888:7888 -v `pwd`:/clj -it --rm \
  --name dockerwheel dockerwheel
# start the container and wire up the relevant ports
```

Now you can go to your container's IP (in this example `192.168.99.100`) at port 3449 and see the starter figwheel app. If you want to connect a REPL, hit that same IP at port 7888.
