# Übungsaufgabe zum Elastic Container Service (ECS)

In dieser Aufgabe erstellen Sie einen ECS-Cluster und installieren darauf einen Fargate-Service mit dem NGINX-Webserver.

## Cluster anlegen

Gehen Sie in die Webkonsole für ECS und erstellen Sie einen neuen Cluster.

![010-create-cluster.png](images/010-create-cluster.png)

---

Vergeben Sie einen beliebigen Namen und wählen den Betriebsmodus "Fargate".

![020-cluster-name.png](images/020-cluster-name.png)

Aktivieren Sie außerdem Container Insights für mehr Logs und Metriken.

![030-container-insights.png](images/030-container-insights.png)

## Aufgabendefinition erstellen

Gehen Sie nun zurück in die Hauptseite der ECS-Webkonsole. Gehen Sie zu den Aufgabendefinitionen und legen Sie eine neue an.

![040-taskdefinition-create.png](images/040-taskdefinition-create.png)

---

Vergeben Sie einen sinnvollen Namen und wählen Sie eine Kapazität von 0.25 vCPU und 0.5 GB.

![050-td-capacity.png](images/050-td-capacity.png)

Als Container wählen Sie das Image `nginx:latest`.

![060-container-details.png](images/060-container-details.png)

Schließen Sie nun die Erstellung der Aufgabendefinition ab.

## Service erstellen

Gehen Sie in die Detailansicht der neu angelegten Aufgabendefinition. Klicken Sie auf "Bereitstellen" und dann auf "Service erstellen".

![070-create-service.png](images/070-create-service.png)

---

Vergeben Sie einen sinnvollen Namen. 

![080-create-service-name.png](images/080-create-service-name.png)

---

Wählen Sie eine VPC mit einem öffentlichen Subnetz. Wählen Sie daraufhin nur öffentliche Subnetze aus. Aktivieren Sie die öffentliche IP.

![090-create-service-setting.png](images/090-create-service-setting.png)

Schließen Sie nun das Anlegen des Services ab.

## Service testen

Gehen Sie zurück in die Clusterübersicht und wechseln Sie in den Tab "Aufgaben". Klicken Sie auf die Aufgabe, die der Service erzeugt hat.

![100-tasks.png](images/100-tasks.png)

In der Detailansicht können Sie die öffentliche IP-Adresse verwenden, um den Service zu testen.

![110-IP.png](images/110-IP.png)