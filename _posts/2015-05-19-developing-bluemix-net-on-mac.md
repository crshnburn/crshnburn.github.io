---
layout: post
title: Developing Bluemix .NET applications on OSX
---

The new .NET buildpack for Bluemix allows for ASP.NET applications to be run in the cloud environment. Traditionally developing in C#, or any of the .NET framework languages required a Windows machine and an copy of Visual Studio. Having moved to OSX as a development environment, and given the support the recently open sourced .NET framework has for other operating systems I set about trying to getting a local development environment up and running.

####Creating the application

1. Install the .NET execution environment following the instructions
2. Get a copy of Visual Studio Code
3. Create a new .NET application in Bluemix and add the code to an IBM DevOps project as described here
4. Clone the git repository

####Running locally

Normally at this stage you would perform a `dnu restore` and `dnx . web-kestrel` in the directory containing the `project.json` and the application would be running locally. However in this case it produces an error because the buildpack is based on Beta3 and by default on OSX you get Beta4. Fortunately help is at hand thanks to Microsoft's work with Docker and making images available there. The following Dockerfile will produce an image that is equivalent of what it running in Bluemix.

```
FROM microsoft/aspnet:1.0.0-beta3

COPY ./src/samplemvc /app
WORKDIR /app
RUN ["kpm", "restore"]

EXPOSE 5000
ENTRYPOINT ["k", "web-kestrel"]
```

Build the image

```
docker build -t aspnetapp .
```

and run

```
docker run -t -d -p 5000:5000 aspnetapp
```

and the application is running locally. This allows for testing before pushing the changes up to the cloud using the DevOps pipeline.
