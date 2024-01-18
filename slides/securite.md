# Taxonomie des failles les plus classiques dans une application WEB

----

## Taxonomie

- **Injections** : correspond au risque d’injection de commande (Système, SQL, Shellcode, ...)
- **Broken Authentification and Session Management** : correspond au risque de casser ou de contourner la gestion de l’authentification et de la session. Comprend notamment le vol de session ou la récupération de mots de passe
- **Cross-Site Scripting** : correspond au XSS soit l’injection de code HTML dans une page, ce qui provoquent des actions non désirées sur une page Web. Les failles XSS sont particulièrement répandues parmi les failles de sécurités Web

----

### Taxonomie

- **Broken Access Control** : correspond aux failles de sécurité sur les droits des utilisateurs authentifiés. Les attaquants peuvent exploiter ces défauts pour accéder à d'autres utilisateurs
- **Security Misconfiguration** : correspond aux failles liées à une mauvaise configuration des serveurs Web, applications, base de données ou framework
- **Sensitive Data Exposure** : correspond aux failles de sécurité exposant des données sensibles comme les mots de passe, les numéros de carte de paiement ou encore les données personnelles et la nécessité de chiffrer ces données

----

### Taxonomie

- **Insufficient Attack Protection** : correspond à un manque de respect des bonnes pratiques de sécurité
- **Cross-Site Request Forgery (CSRF)** : correspond aux failles liées à l’exécution de requêtes à l’insu de l’utilisateur
- **Using Components with Known Vulnerabilities** : correspond aux failles liées à l’utilisation de composants tiers vulnérables
- **Underprotected APIs** : correspond au manque de sécurité d'applications utilisant des API (Applications Programming Interface)

----

## Aujourd'hui

- Injections (*e.g.* SQL)
- XSS: Cross-Site Scripting
- CSRF: Cross-Site Request Forgery
- SSRF: Server-Side Request Forgery
- Using Components with Known Vulnerabilities, Understand how to secure your supply chain

----

## Injection

- SQL
- Système
- Shellcode

----

## SQL Injection

> groupe de méthodes d'exploitation de faille de sécurité d'une application interagissant avec une base de données. Elle permet d'injecter dans la requête SQL en cours un morceau de requête non prévu par le système et pouvant en compromettre la sécurité.

----

<!-- .slide: data-background="./resources/SQLInjection.png" -->
<!-- .slide:data-background-size="40%" -->


----

## SQL Injection


```sql
SELECT uid FROM Users WHERE name =
   '(nom)' AND password = '(mot de passe hashé)';

```

```sql
-- nom = Dupont';--
SELECT uid FROM Users WHERE name =
     'Dupont';--' AND password = 'foo';
```

```sql
-- password = ' or 1 ;--
SELECT uid FROM Users WHERE name = 'Dupont' AND password = '' or 1 ; --';
```

----

## SQLi Protection

- Échappement des caractères spéciaux
- Utilisation d'une requête préparée
  - De nombreux frameworks sont équipés d'un ORM qui se charge entre autres de préparer les requêtes

----

## XSS Cross-site scripting

> type de faille de sécurité des sites web permettant d'injecter du contenu dans une page, provoquant ainsi des actions sur les navigateurs web visitant la page.

<p style="font-size:16pt; text-align: justify">
Les possibilités des XSS sont très larges puisque l'attaquant peut utiliser toutes les possibilités offertes par le navigrateur. Il est par exemple possible de rediriger vers un autre site pour de l'hameçonnage ou encore de voler la session en récupérant les cookies.
</p>

----

## XSS Cross-site scripting

![](resources/how-xss-works-910x404.png)

----

## XSS Protection

- String validation
- String escape
- String sanitization (default behavior in React, Angular, Vue)


----

## CSRF: cross-site request forgery

> CSRF ou XSRF, est un type de vulnérabilité des services d'authentification web.

<p style="font-size:16pt; text-align: justify">
L’objet de cette attaque est de transmettre à un utilisateur authentifié une requête HTTP falsifiée qui pointe sur une action interne au site, afin qu'il l'exécute sans en avoir conscience et en utilisant ses propres droits. L’utilisateur devient donc complice d’une attaque sans même s'en rendre compte. L'attaque étant actionnée par l'utilisateur, un grand nombre de systèmes d'authentification sont contournés.
</p>

----

<!-- .slide: data-background="./resources/CSRF.png" -->
<!-- .slide:data-background-size="80%" -->


----


## CSRF Protection

<ul style="font-size:17pt; text-align: justify">
<li> Demander des confirmations à l'utilisateur pour les actions critiques, au risque d'alourdir l'enchaînement des formulaires.</li>
<ul>  <li>e.g. Demander une confirmation de l'ancien mot de passe à l'utilisateur pour changer celui-ci ou changer l'adresse mail du compte. </li></ul>
<li> Effectuer une vérification du référent dans les pages sensibles : connaître la provenance du client permet de sécuriser ce genre d'attaques. Ceci consiste à bloquer la requête du client si la valeur de son référent est différente de la page d'où il doit théoriquement provenir</li>
<li>Utiliser des jetons de validité (ou Token) dans les formulaires est basé sur la création du token via le chiffrement d'un identifiant utilisateur, un nonce et un horodatage. Le serveur doit vérifier la correspondance du jeton envoyé en recalculant cette valeur et en la comparant avec celle reçue</li>
</ul>



----

## SSRF: Server-side request forgery


<ul style="font-size:17pt; text-align: justify">
<li>SSRF est un type d'attaque où un hacker abuse de la fonctionnalité d'un serveur en lui faisant accéder ou manipuler des informations dans le domaine de ce serveur qui, autrement, ne lui seraient pas directement accessibles.</li>
</ul>

<p style="font-size:16pt; text-align: justify">
Similar to cross-site request forgery which utilises a web client, for example, a web browser, within the domain as a proxy for attacks; an SSRF attack utilizes an insecure server within the domain as a proxy.

If a parameter of a url is vulnerable to this attack, it is possible an attacker can devise ways to interact with the server directly (ie: via 127.0.0.1 or localhost) or with the backend servers that are not accessible by the external users. An attacker can practically scan the entire network and retrieve sensitive information.
</p>


----

<!-- .slide: data-background="./resources/SSRF_diagram.png" -->
<!-- .slide:data-background-size="70%" -->


----

## SSRF Protection

- String validation
- String escape
- Query sanitization


----
## Using Components with Known Vulnerabilities

- Understand the notion of Web Component

---

# Secure Supply Chain

----

## Qu'est ce que la supply chain ?

----

<!-- .slide: data-background="./resources/build-process-diagram.svg" -->
<!-- .slide:data-background-size="80%" -->


----

## Exemple d'architecture d'une application Web moderne ?


Le cas [JHipster](https://www.jhipster.tech/) (dépendances côté back et front)

> démo


----

## Des risques réeels

- e.g. **event-stream**, a widely used Node.js library available via npm.

<p style="font-size:15pt; text-align: justify">
In the fall of 2018, a new user volunteered to take over event-stream on GitHub.The author welcomed the extra help and even gave the newcomer publishing rights. A few weeks later, the new user added a new dependency to event-stream, flatmap-stream. Then the next week, that same user rewrote the code to no longer require the flatmap-stream dependency, and tried to remove it. That change was made in the codebase, but unfortunately wasn’t pushed through to where the library is hosted on npm. In October, another user added malware to flatmap-stream. Anyone who then used event-stream and pulled in the latest version of flatmap-stream would receive this malware. It’s important to emphasize that the library author is not to blame here—who doesn’t have an open source project that would welcome extra help?
</p>

https://github.blog/2020-09-02-secure-your-software-supply-chain-and-protect-against-supply-chain-threats-github-blog/

----

## Des risques réels

- Automatic pwnage (https://blog.sonatype.com/sonatype-spots-150-malicious-npm-packages-copying-recent-software-supply-chain-attacks)
https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610

- Compromission de dépendances (https://www.bleepingcomputer.com/news/security/npm-nukes-nodejs-malware-opening-windows-linux-reverse-shells/)

----

## Unpatched software is still the main culprit

<p style="font-size:16pt; text-align: justify">
Fortunately, it’s estimated that 85 percent of vulnerabilities in open source are disclosed with a patch already available, so all you have to do is apply the patch to stay secure. But being proactive and successful at addressing software supply chain threats goes beyond patching. Following DevSecOps means you need to approach security as an ongoing part of software development, which also means you need to regularly know what dependencies you use, know about vulnerabilities in those dependencies, patch them—then get back to work! As a developer, that means having a few capabilities:

<BR><BR>

Know what’s in your environment. This requires discovering your dependencies, including transitive dependencies; and understanding the risks of those dependencies, such as vulnerabilities and licensing restrictions.
Manage your dependencies. When a new security vulnerability is discovered, first, determine if you’re impacted, and if so, update to obtain the latest functionality and security patches. And keep a hold on your dependencies by reviewing changes that introduce new dependencies and regularly pruning to remove unnecessary dependencies.
Monitor your supply chain. Audit the controls you have in place to manage your dependencies, and eventually, move from audit to enforcement to prevent drift in your supply chain.</p>


----

## Counter measure example

- GitHub provides native tools for software supply chain security:
   -  **Dependency graph** identifies all upstream dependencies and public downstream dependents of a repository or package. You can see their project’s dependencies, as well as any detected vulnerabilities, in the dependency graph.
   - **Dependabot alerts** alerts you of repositories affected by a newly discovered vulnerability based on the dependency graph and GitHub Advisory Database.

----

## Counter measure example

- GitHub provides native tools for software supply chain security:
   - **Dependabot security** updates are pull requests sent to you from Dependabot to update a dependency to the minimum version that resolves a known vulnerability.
   - **Dependabot version** updates also check for new versions of your dependencies on a schedule you set and suggest updates.


----
<!-- .slide: data-background="./resources/surfaceAttaque.jpeg" -->
<!-- .slide:data-background-size="100%" -->

<h2 style="color:#FFFFFF;"> Mais la surface d'attaque est importante </h2>


----

## Comprendre l'arbre d'attaque


----

<!-- .slide: data-background="./resources/Attack-Tree.svg" -->
<!-- .slide:data-background-size="100%" -->

----

<!-- .slide: data-background="./resources/Attack-Tree1.svg" -->
<!-- .slide:data-background-size="70%" -->

----

<!-- .slide: data-background="./resources/Attack-Tree2.svg" -->
<!-- .slide:data-background-size="80%" -->

----

<!-- .slide: data-background="./resources/Attack-Tree3.svg" -->
<!-- .slide:data-background-size="70%" -->

----

## Autre Graal: Reproducible build

<div align="center"><img src="resources/beliefs-attack.png" width="50%"></div>


----

## Autre Graal: Reproducible build

<div align="center"><img src="resources/reprobuilds.png" width="100%"></div>



