# Currency Exchange application

This application serves as an example for the [DevOps Testing webminar](https://www.redhat.com/en/events/webinar/devops-testing-building-an-automated-test-driven-workflow-for-development-and-continuous-integration) demo.

- Frontend
- Exchange (Gateway)
- History
- Currency


## Jenkins agents

This demo uses Jenkins on OpenShift, which needs Jenkins Agents to run specific runtimes

To create Node.js agent:

```
oc process -f jenkins/jenkins-agent-template.yml  -p NAME=jenkins-agent-node-14 -p SOURCE_CONTEXT_DIR=jenkins/node14 SOURCE_REPOSITORY_REF=experiments | oc apply -f -
```

Python agent

```
oc process -f jenkins/jenkins-agent-template.yml  -p NAME=jenkins-agent-python-3 -p SOURCE_CONTEXT_DIR=jenkins/python3 SOURCE_REPOSITORY_REF=experiments | oc apply -f -
```

Cypress

```
oc process -f jenkins/jenkins-agent-template.yml  -p NAME=jenkins-agent-cypress -p SOURCE_CONTEXT_DIR=jenkins/cypress SOURCE_REPOSITORY_REF=experiments | oc apply -f -
```


## Deployments in test environment (branches)

Project:

```
oc new-project rht-jramirez-exchange-test

```

Allow Jenkins to deploy to this project:

```
oc policy add-role-to-user edit system:serviceaccount:rht-jramirez-jenkins:jenkins -n rht-jramirez-exchange-test
```




## Deployments in stage environment

Project:

```
oc new-project rht-jramirez-exchange-stage

```

Allow Jenkins to deploy to this project:

```
oc policy add-role-to-user edit system:serviceaccount:rht-jramirez-jenkins:jenkins
```

Currency

```
oc new-app --name currency \
https://github.com/jramcast/devops-testing#experiments \
--context-dir=currency \
--strategy=docker

oc expose svc/currency
```


History

```
oc new-app --name history \
https://github.com/jramcast/devops-testing#experiments \
--context-dir=history \
--strategy=docker

oc expose svc/history
```


Exchange

```
oc new-app --name exchange \
https://github.com/jramcast/devops-testing#experiments \
--context-dir=exchange

oc expose svc/exchange
```


News

```
oc new-app --name news \
https://github.com/jramcast/devops-testing#experiments \
--context-dir=news \
--strategy=docker

oc expose svc/news
```

Front

```
oc new-app --name frontend \
https://github.com/jramcast/devops-testing#experiments \
--context-dir=frontend \
--strategy=docker

oc expose svc/frontend
```