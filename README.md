# phynxAchievements

Dieses Repository verwaltet die Benutzer-Erfolge für das phynx Framework.

Wenn Sie einen eigenen Erfolg beitragen möchten, so erstellen Sie bitte einen pull request.
Die Diskussion zu den Erfolgen findet im Forum statt: [Forum](https://forum.furtmeier.it/viewtopic.php?f=2&t=1723)

## Einen eigenen Erfolg erstellen

Installieren Sie zunächst das Erfolge-Plugin aus dem [Shop](https://www.open3a.de/page-Plugins)

Anschließend klicken Sie im Erfolge-Reiter auf "Erfolge aktualisieren" und das System lädt die Erfolge aus diesem Repository herunter.

Auf der rechten Seite des Browser-Fensters finden Sie einen Knopf "Erfolge bearbeiten". Wenn Sie diesen anklicken, so erhalten Sie die Möglichkeit, die Erfolge mit dem Stift-Symbol zu bearbeiten oder einen neuen Erfolg anzulegen.

Die einzelnen Felder haben folgende Bedeutung:

* Name: Der Name des Erfolges
* Beschreibung: Eine kurze Beschreibung
* PointCut: An welche Methode im System der Erfolg angehängt wird
* Icon: Das Symbol, das in der Übersicht angezeigt wird
* Points: Die Punkte, die dieser Erfolg gibt
* Requirements: Derzeit nicht benutzt
* Class: Eine Klasse, die ausgeführt wird, um kompliziertere Erfolge abzubilden. Siehe weiter unten.
* Plugin: Das Plugin, das von dem Erfolg betroffen ist
* UID: Die Unique ID des Erfolgs. Wird für die Synchronisation benötigt
* LastChange: Die letzte Änderung des Erfolgs. Wird für die Synchronisation benötigt. Wird automatisch gesetzt.

Für die UID berücksichtigen Sie bitte die Liste der Dateinamen aus diesem Repository.

Um den Erfolg zu veröffentlichen klonen Sie dieses Repository und fügen Sie die Datei hinzu, die Sie mit dem Knopf "XML erzeugen" herunterladen können. Anschließend erstellen Sie einen pull request. Nach einer Prüfung des Codes werde ich den Erfolg dann übernehmen.

## Die point cuts

Derzeit stehen folgende point cuts zur Verfügung:

* ::saveMe - Wird beim Speichern eines Datensatzes ausgeführt
* ::newMe - Wird beim Erstellen eines neuen Datensatzes ausgeführt
* ::deleteMe - Wird beim Löschen eines Datensatzes ausgeführt

Adresse::deleteMe wird entsprechend aufgerufen, wenn eine Adresse aus dem System gelöscht wird.


## Die Erfolge-Klasse

Als Beispiel dient folgende Klasse aus dem Achievement "Erstellen Sie eine Rechnung über mehr als 30.000€".
Beim Speichern des Belegs wird die Methode GRLBM::saveMe aufgerufen und damit eine Instanz der folgenden Klasse erstellt:
```
class CLASSNAME implements iAchievement {
	public function run(Achievement $A, AchievementStatus $S, array $args) {
		if($args[0]->isR == "1" AND $args[0]->bruttobetrag > 30000)
			return true;
		
		return false;
	}
}
```

Der "CLASSNAME" ist in diesem Fall ein Platzhalter, der bei der Instanzierung automatisch ersetzt wird.
Die Methode run() wird dann ausgeführt und muss "true" zurückgeben, wenn der Erfolg erfüllt ist, oder andernfalls "false".

In diesem Beispiel wird nach dem Speichern des Belegs geprüft, ob es sich um eine Rechnung handelt und ob der Bruttobetrag größer als 30 000 ist. Falls ja, wird "true" zurück gegeben.


Die im Array $args enthaltenen Werte sind wie folgt definiert:

* ::saveMe: in [0] befinden sich die Attribute des gespeicherten Datensatzes
* ::deleteMe: in [0] befinden sich die Attribute des gelöschten Datensatzes
* ::newMe: in [0] befinden sich die Attribute des gerade angelegten Datensatzes
