# Übung 7 - Serverless Computing

![serverless.png](serverless.png)

**Aufgabe 1 - Eine Google Cloud Run Function schreiben und testen**

Hinweis: Diese Übung ist freiwillig! Sie erfordert zumindest das Anlegen eines kostenlosen Testaccounts unter [https://cloud.google.com/](https://cloud.google.com/). Selbst der kostenlose Test-Account erfordert jedoch die Angabe von z. B. Kreditkarteninformationen. Es liegt in ihrem eigenen Ermessen, ob Sie entsprechende Angaben machen möchten. Wichtig ist in jedem Fall, die angelegten Ressourcen wieder zu löschen (wie, das wird in dieser Anleitung erklärt). Die Verwendung der entsprechenden Dienste erfolgt jedoch unter eigener Verantwortung!  

Anmerkung: Wir nutzen hier ein IaaS-Angebot der Google Cloud exemplarisch. Dies lässt sich bei anderen Cloud-Providern wie AWS, Azure, IBM etc. in ähnlicher Weise nutzen. 

Für diese Aufgabe müssen Sie zunächst ein Projekt in der Google Cloud Console anlegen (siehe hierzu Anleitung zu Übung 3, Aufgabe 1).  

In den Einstellungen müssen Sie folgende Berechtigungen aktivieren: Cloud Functions, Cloud Build, Artifact Registry, Cloud Run, Logging und Pub/Sub APIs.  

Wählen Sie in der Cloud Console die Übersichtsseite zu Google Cloud Functions aus.  

Klicken Sie auf "Funktion mit einem Inline-Editor" erstellen. Vergeben Sie einen Namen für die Funktion, wählen Sie eine Region aus und wählen Sie unter Laufzeit "Node.js 20".

![function_1.png](function_1.png)

![auth.png](auth.png)

Sobald Sie alles eingerichtet haben, können Sie in den Quellcode-Editor wechseln. Fügen Sie dort den Code aus der Datei `function.js` ein (falls hier noch nichts hinterlegt ist).

![quellcode.png](quellcode.png)

Öffnen Sie anschließend den Tab "Test". 

![test.png](test.png)

Klicken Sie auf "In Cloud Shell ausführen". Dann sollte sich unterhalb ein Terminal-Fenster öffnen, in dem der Code zur Testausführung hinterlegt ist und den Sie per Eingabetaste ausführen können (ggf. müssen Sie die Cloud Shell noch autorisieren).  

Anschließend können Sie unter "Messwerte" einsehen, welche Ressourcen für die Funktionsausführung benutzt wurden.

![messwerte.png](messwerte.png)

**Wichtig:**

Löschen Sie die Ressourcen anschließend wieder (siehe Anleitung zu Übung 3, Aufgabe 1)!



**Hinweise:**

Zunächst einmal können Sie dieses Repository wieder über git klonen.  

Legen Sie für jede der Aufgaben einen Ordner an.   

In dieser Übung werden wir die Programmiersprache Go verwenden. Diese ist im cloud-nativen Umfeld sehr beliebt. Kernkomponenten von Docker und Kubernetes sind bspw. in Go geschrieben.  

Installieren Sie zunächst [Go](https://go.dev/)  

Ob die Installation erfolgreich war, können Sie testen durch: 

   ```bash
go version
   ```
Wichtige Befehle können Sie sich anzeigen lassen mit:

   ```bash
go help
   ```

Hier können Sie sich zunächst einen Überblick über die wichtigsten Sprachkonstrukte verschaffen: [https://www.golang-book.com/books/intro](https://www.golang-book.com/books/intro)  

Hier finden Sie die Standard-Library: [https://pkg.go.dev/std](https://pkg.go.dev/std)  

Außerdem werden wir in dieser Übung Postman verwenden, womit man u.a. APIs testen kann. Sie können das Tool hier kostenlos herunterladen bzw. sich kostenlos registrieren: [https://www.postman.com/](https://www.postman.com/)

**Aufgabe 1 - Eine vereinfachte Rest API mit Go entwickeln und mit Postman testen**

Erstellen Sie ein Verzeichnis für diese Aufgabe und legen Sie die Datei main.go darin ab.  

Erklären Sie, was das Programm tut.  

Öffnen Sie nun ein Terminal und navigieren Sie zum Verzeichnis für diese Aufgabe.  

Starten Sie das Programm mit:

   ```bash
go run main.go
   ```

Rufen Sie in ihrem Browser nun

   ```bash
http://localhost:8080
   ```
auf, und vergewissern sie sich, dass der Server Requests korrekt beantwortet.  

Testen Sie nun mit Postman, wie der Server auf GET, PUT, POST und DELETE Requests reagiert.

**Aufgabe 2 - Anpassung der API zum Handling unterschiedlicher http-Requests**

Passen Sie das Programm aus Aufgabe 1 so an, dass auf die unterschiedlichen http-Requests (GET, PUT, POST und DELETE) auch unterschiedliche Responses erfolgen.  

Hinweis: In der Funktion helloHandler kann durch die Abfrage ob `r.Method` einer `http.MethodGet` entspricht bspw. überprüft werden, ob es sich um einen GET-Request handelt.  

Prüfen Sie nun mit Postman, ob ihr Programm auf unterschiedliche Requests auch entsprechend reagiert.  

**Aufgabe 3 - HTML-Responses**

Passen Sie das Programm aus Aufgabe 2 so an, dass statt reinem Text, html-Code zurückgegeben wird.  

Hinweis: In der Funktion `Fprintf` kann statt Text auch in backticks gewrappter html-Code eingefügt werden.    

Prüfen Sie mit ihrem Browser bzw. mit Postman, ob ihr Programm auf unterschiedliche Requests auch entsprechenden html-Code zurück gibt. 

**Aufgabe 4 - Zwei einfache kommunizierende Microservices**

Im Verzeichnis 2-simple-microservices finden Sie in den Unterverzeichnissen zwei einfache in Go implementierte Microservices. Starten Sie sowohl im Verzeichnis service-a als auch im Verzeichnis service-b die beiden Dienste mit

   ```bash
go run main.go
   ```

Öffnen Sie ihren Browser mit der Adresse

   ```bash
http://localhost:8080/service-a
   ```
und prüfen Sie, ob die Kommunikation der beiden Dienste funktioniert.  

Wie sind die beiden Dienste gekoppelt? Ist das gut? Diskutieren Sie!

**Aufgabe 5 - Microservices und Event Broker am Beispiel von RabbitMQ**

In dieser Aufgabe werden wir [RabbitMQ](https://www.rabbitmq.com/) als Event Broker für die Kommunikation von Microservices im Pub-Sub Modus verwenden. Über folgenden Befehl können Sie RabbitMQ in einem Docker Container starten:

   ```bash
docker run -d --hostname rabbitmq --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
   ```
Öffnen Sie ihren Browser mit der Adresse 

   ```bash
http://localhost:15672
   ```
Mit `login: guest`, `password: guest` können Sie sich in das Management Interface einloggen.  

Im Verzeichnis microservices-rabbitmq finden Sie in den Unterverzeichnissen jeweils zwei einfache Microservices, die über den Event Broker miteinander kommunizieren können.  

Welcher ist der Publisher und welcher der Subscriber?  

Öffnen Sie ein Terminal und navigieren Sie in das Verzeichnis microserviceA und führen Sie dort folgendene Befehle aus:

   ```bash
go mod init microserviceA
   ```
   ```bash
go get github.com/streadway/amqp
   ```
   ```bash
go run main.go
   ```
Der Dienst sollte nun laufen und Nachrichten senden. Überprüfen Sie das im RabbitMQ Management Interface.  

Starten Sie microserviceB auf dieselbe Weise.  

Um weitere Aktivitäten erkennen zu können, müssen Sie anschließend microserviceA erneut starten. Überprüfen Sie das im RabbitMQ Management Interface.  

Diskutieren Sie auch hier die Kopplung der Dienste.  

Fügen Sie dem System einen weiteren Microservice als Subscriber hinzu.  

![rabbitmq.png](rabbitmq.png)


