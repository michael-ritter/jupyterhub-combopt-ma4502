# Imprint and Privacy Statement

Next, we'll add an imprint and a privacy statement to the jupyterhub configuration.

[TOC]

## Copy templates

We start by copying the login page template where we will add two links to imprint and privacy statement pages.

```bash
$ cd /srv/jupyterhub
$ source /srv/jupyterhub/venv/bin/activate
(venv)$ mkdir custom-templates
(venv)$ cp ./venv/share/jupyterhub/templates/login.html custom-templates/
```

## Modify templates

Modify the template to your liking. As an example, we could use the following ```login.html```:

```html
{% extends "templates/login.html" %}

{% block login %}
{{ super() }}

<div class="container"><div class="row"><a class="pull-left" href="/hub/static/about.html">About / Impressum</a><a class="pull-right" href="/hub/static/privacy.html">Privacy Statement / Datenschutzerklärung</a></div></div>
{% endblock %}
```

## Modify jupyterhub_config.py

To actually use the custom template, we add the following lines to jupyter_config.py (after the other edits). Remember, the file is located at ```/etc/jupyterhub/```.

```python
# sets a custom html template at the login screen.
c.JupyterHub.template_paths = ['/srv/jupyterhub/custom-templates/']
```

## Add imprint and privacy statement static pages

JupyterHub automatically serves pages located in ```/srv/jupyterhub/venv/share/jupyterhub/static```. We therefore simply create the files ```about.html``` and ```privacy.html``` that we need in that directory. Copy the following contents into the respective files:

### /srv/jupyterhub/venv/share/jupyterhub/static/about.html

```html
<!DOCTYPE HTML>
<html>

<head>
    <meta charset="utf-8">

    <title>JupyterHub</title>
    <meta http-equiv="X-UA-Compatible" content="chrome=1">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">


    <link rel="stylesheet" href="/hub/static/css/style.min.css?v=883a1cf654bd9f5cdc5ca7c91efe7d71" type="text/css"/>

    <script src="/hub/static/components/requirejs/require.js?v=f0cc8bbb2fcef87fc194fecbb632fcfa" type="text/javascript" charset="utf-8"></script>
    <script src="/hub/static/components/jquery/dist/jquery.min.js?v=a09e13ee94d51c524b7e2a728c7d4039" type="text/javascript" charset="utf-8"></script>
    <script src="/hub/static/components/bootstrap/dist/js/bootstrap.min.js?v=2f34b630ffe30ba2ff2b91e3f3c322a1" type="text/javascript" charset="utf-8"></script>
    <script>
      require.config({

          urlArgs: "v=20190429131906",

          baseUrl: '/hub/static/js',
          paths: {
            components: '../components',
            jquery: '../components/jquery/dist/jquery.min',
            bootstrap: '../components/bootstrap/dist/js/bootstrap.min',
            moment: "../components/moment/moment",
          },
          shim: {
            bootstrap: {
              deps: ["jquery"],
              exports: "bootstrap"
            },
          }
      });
    </script>

    <script type="text/javascript">
      window.jhdata = {
        base_url: "/hub/",
        prefix: "/",


        admin_access: false,


        options_form: false,

      }
    </script>




</head>

<body>

<noscript>
  <div id='noscript'>
    JupyterHub requires JavaScript.<br>
    Please enable it to proceed.
  </div>
</noscript>


  <nav class="navbar navbar-default">
    <div class="container-fluid">
      <div class="navbar-header">
        <span id="jupyterhub-logo" class="pull-left"><a href="/hub/"><img src='/hub/logo' alt='JupyterHub' class='jpy-logo' title='Home'/></a></span>
        <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#thenavbar" aria-expanded="false">
          <span class="sr-only">Toggle navigation</span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </button>
      </div>

      <div class="collapse navbar-collapse" id="thenavbar">

        <ul class="nav navbar-nav navbar-right">

            <li>


            </li>

        </ul>
      </div>



    </div>
  </nav>

<div id="about-main" class="container">
<h1>Impressum</h1>
<p>Die nachstehenden Informationen enthalten die gesetzlich vorgesehenen Pflichtangaben zur Anbieterkennzeichnung, zu den datenschutzrechtlichen Informationspflichten
sowie wichtige rechtliche Hinweise zur Internet-Präsenz <a href="https://m09vm14.ma.tum.de">https://m09v14.ma.tum.de</a>.</p>

<h2>Vertretungsberechtigt</h2>
<p>Die Technische Universität München wird gesetzlich vertreten durch den Präsidenten Prof. Dr. Dr. h.c. mult. Wolfgang A. Herrmann.</p>
<p>Die Technische Universität München ist gemäß Art. 11 Abs. 1 S. 1 BayHSchG eine Körperschaft des öffentlichen Rechts mit dem Recht der Selbstverwaltung im Rahmen der Gesetze und zugleich gemäß Art. 1 Abs. 2 Satz 1 Nr. 1 BayHSchG staatliche Hochschule (staatliche Einrichtung). Die Technische Universität München nimmt eigene Angelegenheiten als Körperschaft (Körperschaftsangelegenheiten) unter der Rechtsaufsicht der Aufsichtsbehörde, staatliche Angelegenheiten als staatliche Einrichtung wahr (Art. 12 Abs. 1 BayHSchG).</p>

<h2>Zuständige Aufsichtsbehörde</h2>
<address><strong>Bayerisches Staatsministerium für Wissenschaft und Kunst</strong><br>
  Salvatorstraße 2<br>
  80333 München.</address>

<h2>Umsatzsteuer<U+00AD>identifikations<U+00AD>nummer</h2>
<p>DE811193231 (gemäß § 27a Umsatzsteuergesetz)</p>

<h2>Inhaltlich verantwortlich</h2>
<address><strong>Dr. Michael Ritter</strong><br>
Fakultät für Mathematik<br>
Boltzmannstr. 3<br>
85747 Garching <br>
michael.ritter (at) tum.de</address>

<h2>Bildnachweis</h2>
<p>Sofern keine weiteren Angaben zu den Bildern angegeben sind, gelten folgende Quellen: MA TUM, Andreas Heddergott / TUM, Astrid Eckert / TUM.</p>

<h2>Technische Umsetzung</h2>
<p>Diese Website ist mit Nginx / JupyterHub umgesetzt.</p>

<h2>Haftungshinweis</h2>
<p>Trotz sorgfältiger inhaltlicher Kontrolle übernehmen wir keine Haftung für die Inhalte externer Links. Für den Inhalt der verlinkten Seiten sind ausschließlich deren Betreiber verantwortlich.</p>
<p>Namentlich gekennzeichnete Beiträge geben die Meinung des Autors wieder. Für die Inhalte der Beiträge sind ausschließlich die Autoren verantwortlich.</p>

<h2>Anschrift</h2>
<address><strong>Fakultät für Mathematik</strong><br>
Boltzmannstraße 3<br>
85748 Garching</address>
</div>

<script>
if (window.location.protocol === "http:") {
  // unhide http warning
  var warning = document.getElementById('insecure-login-warning');
  warning.className = warning.className.replace(/\bhidden\b/, '');
}
</script>

</body>

</html>
```

### /srv/jupyterhub/venv/share/jupyterhub/static/privacy.html

```html
<!DOCTYPE HTML>
<html>

<head>
    <meta charset="utf-8">

    <title>JupyterHub</title>
    <meta http-equiv="X-UA-Compatible" content="chrome=1">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">


    <link rel="stylesheet" href="/hub/static/css/style.min.css?v=883a1cf654bd9f5cdc5ca7c91efe7d71" type="text/css"/>

    <script src="/hub/static/components/requirejs/require.js?v=f0cc8bbb2fcef87fc194fecbb632fcfa" type="text/javascript" charset="utf-8"></script>
    <script src="/hub/static/components/jquery/dist/jquery.min.js?v=a09e13ee94d51c524b7e2a728c7d4039" type="text/javascript" charset="utf-8"></script>
    <script src="/hub/static/components/bootstrap/dist/js/bootstrap.min.js?v=2f34b630ffe30ba2ff2b91e3f3c322a1" type="text/javascript" charset="utf-8"></script>
    <script>
      require.config({

          urlArgs: "v=20190429131906",

          baseUrl: '/hub/static/js',
          paths: {
            components: '../components',
            jquery: '../components/jquery/dist/jquery.min',
            bootstrap: '../components/bootstrap/dist/js/bootstrap.min',
            moment: "../components/moment/moment",
          },
          shim: {
            bootstrap: {
              deps: ["jquery"],
              exports: "bootstrap"
            },
          }
      });
    </script>

    <script type="text/javascript">
      window.jhdata = {
        base_url: "/hub/",
        prefix: "/",


        admin_access: false,


        options_form: false,

      }
    </script>




</head>

<body>

<noscript>
  <div id='noscript'>
    JupyterHub requires JavaScript.<br>
    Please enable it to proceed.
  </div>
</noscript>


  <nav class="navbar navbar-default">
    <div class="container-fluid">
      <div class="navbar-header">
        <span id="jupyterhub-logo" class="pull-left"><a href="/hub/"><img src='/hub/logo' alt='JupyterHub' class='jpy-logo' title='Home'/></a></span>
        <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#thenavbar" aria-expanded="false">
          <span class="sr-only">Toggle navigation</span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </button>
      </div>

      <div class="collapse navbar-collapse" id="thenavbar">

        <ul class="nav navbar-nav navbar-right">

            <li>


            </li>

        </ul>
      </div>



    </div>
  </nav>

  <div id="about-main" class="container">
    <h1>Datenschutzerklärung</h1>
<p>Für die Technische Universität München ist Datenschutz ein wichtiges Anliegen. Wir möchten, dass Sie wissen, wann wir welche Daten speichern und wie wir sie verwenden. Personenbezogene Daten werden nur im technisch notwendigen Umfang erhoben. Die Datenverarbeitung unterliegt den geltenden datenschutzrechtlichen Bestimmungen, insbesondere der Datenschutz-Grundverordnung (DSGVO), dem Bayerischen Datenschutzgesetz (BayDSG) und dem Telemediengesetz (TMG).</p>
<p>Diese Datenschutzerklärung (Informationspflicht nach Art. 13, 14 der Datenschutz-Grundverordnung DSGVO) bezieht sich auf die Verarbeitung personenbezogener Daten im Rahmen dieses Internetauftritts. Nachfolgend informieren wir Sie über Art, Umfang und Zweck der Erhebung und Verwendung personenbezogener Daten. Diese Informationen können jederzeit von unserer Webseite abgerufen werden.</p>

<h2>Verantwortliche Stelle</h2>
<p>Verantwortliche Stelle im Sinne der Datenschutzgesetze ist:</p>
<address><strong>Technische Universität München</strong><br>
Arcisstraße 21<br>
80333 München<br>
Telefon +49 (0)89 289 01</address>

<h2>Datenschutzbeauftragter der Technischen Universität München:</h2>
<address><strong>Prof. Dr. Uwe Baumgarten</strong><br>
Arcisstraße 21<br>
80333 München<br>
E-Mail: beauftragter (at) datenschutz.tum.de<br>
Weitere Kontaktdaten finden Sie auf <a href="https://www.datenschutz.tum.de">www.datenschutz.tum.de.</a></address>

<h2>Technische Umsetzung</h2>
<p>Wenn Sie diese oder andere Internetseiten aufrufen, übermitteln Sie über Ihren Internetbrowser Daten an unseren Webserver. Dieser wird betrieben von:</p>
<address><strong>Leibniz-Rechenzentrum (LRZ)</strong><br>
  Boltzmannstr. 1<br>
  85748 Garching bei München</address>
<p>Bei jedem Zugriff auf Seiten unserer Webpräsenzen speichern die Webserver folgende Informationen temporär in Logdateien:</p>
<ul>
<li>IP-Adresse des anfragenden Rechners</li>
<li>Datum und Uhrzeit des Zugriffs</li>
<li>Name, URL und übertragene Datenmenge der abgerufenen Datei</li>
<li>Zugriffsstatus (angeforderte Datei übertragen, nicht gefunden etc.)</li>
<li>Erkennungsdaten des verwendeten Browser- und Betriebssystems (sofern vom anfragenden Webbrowser übermittelt)</li>
<li>Webseite, von der aus der Zugriff erfolgte (sofern vom anfragenden Webbrowser übermittelt)</li>
<li>Ihre TUM-Kennung oder Ihr Login-Name (sofern beim Login übermittelt)</li>
</ul>

<p>Die Verarbeitung der Daten in dieser Logdatei geschieht wie folgt:</p>
<ul>
<li>Die Logeinträge werden kontinuierlich automatisch ausgewertet, um Angriffe auf die Webserver erkennen und entsprechend reagieren zu können.</li>
<li>In Einzelfällen, d.h. bei gemeldeten Störungen, Fehlern und Sicherheitsvorfällen, erfolgt eine manuelle Analyse.</li>
<li>Logeinträge, die älter als sieben Tage sind, werden durch Kürzung der IP-Adresse anonymisiert.</li>
<li>Die anonymisierten Logs werden zur Erstellung von Zugriffsstatistiken verwendet.Die hierfür eingesetzte Software wird lokal vom LRZ betrieben.</li>
<li>Die in den Log-Einträgen enthaltenen IP-Adressen werden vom LRZ nicht mit anderen Daten<U+00AD>beständen zusammengeführt, sodass kein Rückschluss auf einzelne Personen möglich ist.</li>
</ul>

<h2>Zweck und Rechtsgrundlage der Datenverarbeitung</h2>
<p>Zweck der Verarbeitung personenbezogener Daten ist der Betrieb dieses Internetauftritts. Eine Verarbeitung personenbezogener Daten (IP-Adresse) erfolgt nur, soweit dies zum Zweck des Betriebs des Internetauftritts einschließlich dessen Sicherstellung erforderlich ist.</p>
<p>Rechtsgrundlage für die Verarbeitung personenbezogener Daten sind Art. 6 Abs. 1 Buchst. e, Abs. 2 und 3 DSGVO und Art. 4 Abs. 1 des Bayerischen Datenschutzgesetzes (BayDSG).</p>

<h2>Nutzung und Weitergabe personenbezogener Daten</h2>
<p>Die Nutzung unserer Webseite ist in der Regel ohne Angabe personen<U+00AD>bezogener Daten möglich. Personenbezogene Daten, die Sie gegebenenfalls über Kontaktformulare, Anmeldeformulare und ähnliche Webseiten auf unseren Webservern an uns leiten, werden nur mit Ihrem Einverständnis entgegengenommen und dienen ausschließlich dem in den Formularen genannten Zweck. Bitte beachten Sie die entsprechenden Hinweise auf den Formularen.</p>
<p>In allen Formularen werden nur die personenbezogenen Daten erhoben, die unbedingt erforderlich sind. Üblicherweise werden die personenbezogenen Daten in einer Datenbank des Leibnitz Rechenzentrums gespeichert und sind den jeweils zuständigen Mitarbeitern der TUM zugänglich. Ist der Zweck der Datenerhebung, die Daten auch anderen Mitgliedern der TUM zugänglich zu machen, so wird hierbei das Prinzip der Datensparsamkeit beachtet. Ihre Daten werden nicht an Dritte weiteregegeben, es sei denn, dies ist im Formular explizit angegeben.</p>
<p>Soweit Ihre personenbezogenen Daten verarbeitet werden, haben Sie diesbezüglich im Grundsatz nachfolgende Rechte:</p>
<ul>
<li>Auskunft über Ihre bei uns gespeicherten Daten und deren Verarbeitung (Art. 15 DSGVO),</li>
<li>Berichtigung/Vervollständigung unrichtiger personenbezogener Daten (Art. 16 DSGVO),</li>
<li>Löschung Ihrer bei uns gespeicherten Daten (Art. 17 DSGVO),</li>
<li>Einschränkung der Datenverarbeitung, sofern wir Ihre Daten aufgrund gesetzlicher Pflichten noch nicht löschen dürfen (Art. 18 DSGVO),</li>
<li>Ggf. ein Recht auf Datenübertragbarkeit (Art. 20 DSGVO),</li>
<li>Widerspruch gegen die Verarbeitung Ihrer Daten bei uns (Art. 21 DSGVO).</li>
</ul>

<p>Einschränkungen und Modifikationen der vorgenannten Rechte können sich aus Art. 9 und 10 BayDSG sowie aus Art. 20 BayDSG ergeben.</p>
<p>Sofern Sie uns eine Einwilligung erteilt haben, können Sie diese jederzeit mit Wirkung für die Zukunft widerrufen. Hierzu wenden Sie sich bitte an it-support (at) tum.de.</p>
<p>Sie können sich jederzeit mit einer Beschwerde an die für uns zuständige Aufsichtsbehörde wenden. Die zuständige Datenschutz Aufsichtsbehörde für die Technische Universität München ist:</p>
<address><strong>Der Bayerische Landesbeauftragte für den Datenschutz</strong><br>
Prof. Dr. Thomas Petri<br>
Wagmüllerstraße 18<br>
80538 München<br>
E-Mail: poststelle (at) datenschutz-bayern.de<br>
Telefon: +49 (0)89 212672-0<br>
<a href="https://www.datenschutz-bayern.de">https://www.datenschutz-bayern.de</a></address>

<p>Der Nutzung von im Rahmen der Impressumspflicht veröffentlichten Kontaktdaten durch Dritte zur Übersendung von nicht ausdrücklich angeforderter Werbung und Informationsmaterialien wird hiermit ausdrücklich widersprochen. Die Betreiber der Seiten behalten sich ausdrücklich rechtliche Schritte im Falle der unverlangten Zusendung von Werbeinformationen etwa durch Spam-Mails vor.</p>

<h2>E-Mail-Sicherheit</h2>
<p>Informationen, die Sie unverschlüsselt per elektronischer Post (E-Mail) an uns senden, könnten Dritte möglicherweise auf dem Übertragungsweg lesen. Wir können in der Regel Ihre Identität nicht überprüfen und wissen nicht, wer sich hinter einer E-Mail-Adresse verbirgt. Eine rechtssichere Kommunikation durch einfache E-Mail ist daher nicht gewährleistet. Wir setzen - wie viele E-Mail-Anbieter - Filter gegen unerwünschte Werbung ("SPAM-Filter") ein, die in seltenen Fällen auch normale E-Mails fälschlicherweise automatisch als unerwünschte Werbung einordnen und löschen. E-Mails, die schädigende Programme ("Viren") enthalten, löschen wir in jedem Fall automatisch.</p>

<h2>Cookies</h2>
<p>Um den Funktionsumfang unseres Internetangebotes zu erweitern und die Nutzung für Sie komfortabler zu gestalten, verwenden wir zum Teil so genannte "Cookies". Mit Hilfe dieser Cookies können bei dem Aufruf unserer Webseite Daten auf Ihrem Rechner gespeichert werden. Sie können das Speichern von Cookies jedoch deaktivieren oder Ihren Browser so einstellen, dass Cookies nur für die Dauer der jeweiligen Verbindung zum Internet gespeichert werden. Hierdurch könnte allerdings der Funktionsumfang unseres Angebotes eingeschränkt werden.</p>

<h2>Links</h2>
<p>Von unseren eigenen Inhalten sind Querverweise („Links“) auf die Webseiten anderer Anbieter zu unterscheiden.
Durch diese Links ermöglichen wir lediglich den Zugang zur Nutzung fremder Inhalte nach § 8 Telemediengesetz (TMG). Wir haben keinen Einfluss darauf, dass deren Betreiber die Datenschutzbestimmungen einhalten. Für illegale, fehlerhafte oder unvollständige Inhalte und insbesondere für Schäden, die aus der Nutzung oder Nichtnutzung von Informationen Dritter entstehen, haftet allein der jeweilige Anbieter der Seite.</p>

<h2>Auskunft und Berichtigung</h2>
<p>Sie haben das Recht, auf schriftlichen Antrag und unentgeltlich Auskunft über die personen<U+00AD>bezogenen Daten zu erhalten, die über Sie gespeichert sind. Zusätzlich haben Sie das Recht auf Berichtigung unrichtiger Daten, Sperrung und Löschung.</p>
<address><strong>Datenschutzbeauftragter der Fakultät für Mathematik</strong><br>
 <a href="https://campus.tum.de/tumonline/visitenkarte.show_vcard?pPersonenId=EC7C4F2B0019C019&pPersonenGruppe=3" Dr. Christian Ludwig<br>
Boltzmannstraße 3<br>
    85748 Garching bei München<br>
    E-Mail: bv.ma (at) datenschutz.tum.de</address>
</div>

<script>
if (window.location.protocol === "http:") {
  // unhide http warning
  var warning = document.getElementById('insecure-login-warning');
  warning.className = warning.className.replace(/\bhidden\b/, '');
}
</script>

</body>

</html>
```

## Summary

In this section we extended the jupyterhub login page by links to an imprint and a privacy statement and created the two static html files.

## Next Steps

The next step is to connect JupyterHub to the TUM Shibboleth system for authentication.
