# Architecture

Reliance on Wikidata rather than inside the SCTA system itself?

Half the content of a prosopography can be built by the computer (pulling from
other sources).

SCTA Person file with basic person information and external id pointers. A seed
file. Processed for populating a more rich file.

Is it possible within our interface to add or edit change that is fed back to
Wikidata?

One might worry about a discouraging user experience where the user needs to
jump to an external source to change the data that eventually will make it back
into the SCTA datastore.

Creating new entries, procedure:
- Create person
- Check for Wikidata or SCTA info:
  - If exists: populate the data
  - If not, populate manually
- Save in SCTA
- Send update request to Wikidata

Backend details:
- Enable multiple input possibilities (SCTA UI, Oxygen, other web app)
- Changes pushed in pull requests
- Travis CI: Github checks that against the Schema
- Github fires webhooks starting rebuild of the relevant part of the SCTA graph
- Github could fire webhook with submission to the Wikidata.


IMAGE OF THE ARCHITECTURE DRAWING


# Person Description File Fields

Filename needs to be the short id (e.g. Aquinas, JohnDinsdale, AdamWodeham).

The source of truth in pulling data from the world outside is the local file.

```json
{
    "@context": "https://scta.info/api/core/1.0/people/context.json",
    "@id": "https://scta.info/resource/Aquinas",
    "@type": "https://scta.info/resource/person",
    "sctap:personType": "https://scta.info/resource/Scholastic",
    "owl:sameAs": "https://www.wikidata.org/wiki/Q9438",
    "dc:title": [
        {
            "@value": "Thomas Aquinas",
            "@language": "en"
        },
        {
            "@value": "Tomaso di Aquino",
            "@language": "it"
        }
    ],
    "alternativeNames": [
        {
            "@value": "Doctor Angelicus",
            "@language": "la"
        },
        {
            "@value": "Angelic Doctor",
            "@language": "en"
        },
        {
            "@value": "Dottore Angelico",
            "@language": "it"
        }
    ],
    "hasAffiliation": [
        "https://scta.info/resource/Dominican"
    ],
    "hasEvent": [
        {
            "@id": "https://scta.info/resource/event-id",
            "@type": "https://scta.info/resource/eventype-id",
            "dc:title": "Birth",
            "dc:description": "Description",
            "location": "wikidata-id",
            "start": "124u",
            "end": "1251",
        }
    ],
    "description": "Short description"
}
```

Field types and source:
- `owl:sameAs`: Wikidata if it is available. If not,


## Anonymous names

Handling of anonymous authors:
- In the case that we don't know anything about the author, it would just be
  attributed to "Anonymous" as a common grab-bag for all those cases.
- If something is known about the person, just not the name (period, text,
  etc.), it would be a new, unique Anonymous, e.g. "Anonymous1", "Anonymous2"
  etc.



# Contributor roles

`sctap:Contributor` is the top-level role of which every other role is an
instance.

Expression contributor roles:
```
sctap:Contributor

  sctap:Author

  sctap:Abbreviator

  sctap:Redactor
```


Manifestation Text contributor roles:
```
sctap:Contributor

  sctap:Copyist (Creator of the manuscript copy, scribe)

  sctap:Annotator (Author of marginal annotation)

  sctap:Editor (Corrector of errors in a manuscript or typeset print)

  sctap:Translator
```


Manifestation Codex contributor class:
```
sctap:Contributor

  sctap:Printer

  sctap:Owner

  sctap:Sponsor
```

Transcription contributor class:
```
sctap:Contributor

  sctap:Editor (Main editor, responsible for the general constitution of the text, implies responsibility for the other categories)

  sctap:Transcriber (Contributor to the transcription)

  sctap:Encoder (Contributor to the encoding of the file)

  sctap:Reviser (Contributor of textual review or revision of the material)
```
