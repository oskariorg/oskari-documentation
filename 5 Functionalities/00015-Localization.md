### Multilingual support

Oskari supports localisation for the UI and currently includes the translations in English, Finnish and Swedish. Additional translations can be added quite easily if required.

In addition to the languages mentioned above, Oskari also includes partial translations for the following languages:

- Dutch
- Estonian
- French
- German
- Icelandic
- Italian
- Norsk bokmål
- Nynorsk
- Slovakian
- Slovenian
- Spanish

#### Localization on server

Most of the localizations are in the frontend bundles, but there are some localizations on the server side as well. The server localization can be found under [oskari-server messages.properties](https://github.com/oskariorg/oskari-server/tree/master/servlet-map/src/main/resources/locale) and the can be overridden with `messages-ext.properties` in applications. These localizations can be used in JSP files with the JSTL tags:

```xml
<spring:message code="logout" text="Logout" />
```
