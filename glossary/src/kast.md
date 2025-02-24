# Kast

## What is Kast.

Lets imagine Kast as a Librarian that checks for all the *Books* and its contents, Kast is a tool to manage Argocd Apps, its values, and that *Renders* new apps using common resourses that are imported from an speciall kind of Helm Chart called "Kaster".

It allows you to standarize the whole deployment of apps by build  metacharts that are automaticly managed by Kast as Argo CD apps. with both default and user defined Values. This allows the User to define a whole platform with its standard resources in a simple short yaml file that follows a directory standard to setting the whole infrastructure as tidy as posible. 

all this works by the kast main chart, which has all the Helm code that renders the desired infrastructure using the charts, repos and values given by the user.

> WIP more kast definitions
