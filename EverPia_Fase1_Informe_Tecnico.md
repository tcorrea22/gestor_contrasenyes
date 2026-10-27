# EverPia — Informe d'anàlisi i justificació tècnica: gestors de contrasenyes

| | |
|---|---|
| **Empresa (fictícia)** | EverPia, consultora tecnològica |
| **Tema** | Anàlisi i selecció d'un gestor de contrasenyes |
| **Eina escollida** | **Proton Pass** |
| **Alumne/a** | Toni Correa |
| **Mòdul** | Seguretat Informatica |

---

## 1. Introducció i context de l'incident

EverPia és una consultora tecnològica que ha patit un incident de seguretat. Un atacant ha aconseguit entrar al compte d'un tècnic, probablement perquè tenia una contrasenya **dèbil o reutilitzada**. Un cop dins, l'atacant amenaça de publicar informació confidencial d'un projecte que encara està en desenvolupament si l'empresa no paga un rescat.

Davant d'això, la Direcció Tècnica demana que el personal tècnic faci servir un **gestor de contrasenyes validat**. Aquest informe explica per què, compara tres eines (Proton Pass, Bitwarden i KeePassXC) i justifica quina es fa servir a la pràctica.

### 1.1. Per què una contrasenya dèbil o reutilitzada és un risc crític

Una contrasenya és, molt sovint, l'única barrera entre un atacant i les dades de l'empresa. Si és fàcil d'endevinar o si ja s'ha filtrat en un altre lloc, aquesta barrera no serveix de gaire. Els atacs més habituals són:

| Atac | Com funciona | Per què afecta contrasenyes dèbils o repetides |
|---|---|---|
| **Atac de diccionari** | Es proven contrasenyes d'una llista de paraules i combinacions habituals (`Password123`, noms, anys...). | Les contrasenyes previsibles hi surten de les primeres. |
| **Força bruta** | Es proven totes les combinacions possibles de caràcters fins a encertar. | Com més curta és la contrasenya, menys combinacions hi ha i abans es troba. Una de llarga fa que el temps necessari creixi moltíssim. |
| **Credential stuffing** | L'atacant agafa parelles usuari/contrasenya filtrades d'un servei i les prova en molts altres. | Només funciona si la persona **ha reutilitzat** la contrasenya. Una filtració en una web sense importància pot obrir el compte de la feina. |
| **Phishing i robatori de credencials** | Un correu o una web falsa enganya la víctima perquè escrigui ella mateixa les seves dades. | Una contrasenya forta no ajuda si es regala. Per això calen MFA i formació, a més del gestor. |

### 1.2. El problema de reutilitzar contrasenyes

Quan una persona fa servir la mateixa contrasenya (o una de molt semblant) a diversos serveis, tots els comptes depenen del servei **menys protegit**. Si aquest es filtra, l'atacant pot provar la mateixa contrasenya al correu, al repositori de codi, a la VPN o als gestors de projectes de l'empresa. Això és justament el que va passar a EverPia: **un sol compte feble va ser suficient**.

### 1.3. Conseqüències possibles per a una consultora tecnològica

- **Exposició de projectes**: el codi, els documents o les dades d'un client poden acabar en mans d'un tercer.
- **Accés no autoritzat**: amb un compte tècnic es pot arribar a sistemes interns, servidors o eines de clients.
- **Robatori d'informació**: es pot copiar informació confidencial abans que ningú se n'adoni.
- **Dany reputacional**: un client que veu que la consultora no protegeix les seves dades pot deixar de confiar-hi.
- **Costos de recuperació**: investigar l'incident, canviar credencials, avisar clients i, si cal, gestionar aspectes legals.

Tot això està relacionat amb les credencials. Per tant, millorar-ne la gestió és una de les mesures més urgents i d'impacte més gran.

---

## 2. Com ajuda un gestor de contrasenyes

Un gestor de contrasenyes és un programa que guarda les credencials en un **magatzem xifrat**, protegit per l'accés principal de l'usuari. Això és el que aporta:

- **Generació de contrasenyes llargues i úniques.** El gestor les crea a l'atzar, de manera que no depenen de la imaginació de la persona.
- **Emmagatzematge.** Ningú no ha de recordar desenes de contrasenyes. Només cal protegir bé l'accés al gestor.
- **Sincronització** (en els gestors que la tenen). Les mateixes dades estan disponibles a l'ordinador, al mòbil i al navegador.
- **Autocompletat i comoditat.** Omple l'usuari i la contrasenya per nosaltres. A més, normalment només ho fa al domini que correspon, i això ajuda a detectar webs falses.
- **Menys reutilització.** Si crear una contrasenya nova no costa res, deixa de fer-se servir la mateixa a tot arreu.
- **Protecció de l'accés principal.** Tot el sistema depèn de com es protegeix l'entrada al gestor. Per això convé una contrasenya llarga i única, i activar la **MFA**.

### 2.1. Què NO soluciona un gestor

Un gestor de contrasenyes **no elimina tots els riscos**. No substitueix:

- la **autenticació multifactor (MFA)**;
- les **actualitzacions** de sistemes i aplicacions;
- la **formació contra el phishing**;
- els **procediments de resposta a incidents**.

Si l'ordinador de la persona té programari maliciós, o si algú entra al compte principal del gestor, el gestor tampoc no serveix per si sol. És una peça important, però no l'única.

---

## 3. Comparativa tècnica: Proton Pass, Bitwarden i KeePassXC

Proton Pass i Bitwarden són gestors **amb sincronització al núvol**. KeePassXC és un gestor **local**: guarda tot en un fitxer xifrat (`.kdbx`) que gestiona l'usuari.

> **Com llegir la taula.** Les dades marcades amb ✔ surten de la documentació oficial consultada per fer aquest informe (enllaços a la bibliografia). Les marcades amb (*) depenen de la versió, de l'aplicació o del pla contractat, o no s'han pogut comprovar en detall: cal mirar la web oficial abans de decidir. No s'indiquen preus perquè canvien sovint.

| Criteri | Proton Pass | Bitwarden | KeePassXC |
|---|---|---|---|
| **Model d'emmagatzematge** | Al núvol de Proton, amb dades xifrades ✔ | Als servidors de Bitwarden, només hi ha dades xifrades ✔. (*) També es pot allotjar en servidor propi segons el pla | Local: un fitxer `.kdbx` al dispositiu de l'usuari ✔ |
| **Xifrat i arquitectura** | Xifrat d'extrem a extrem. Cada caixa forta (*vault*) té una clau pròpia i les dades es xifren amb AES-256-GCM. Les operacions criptogràfiques es fan al dispositiu ✔ | Xifrat AES-256 en mode CBC amb HMAC-SHA256. Model de «coneixement zero»: les dades es xifren al dispositiu abans de pujar-les ✔ | La base de dades sempre va xifrada. Amb KDBX 4 es recomana la derivació de clau **Argon2** ✔. Es pot combinar la contrasenya amb un fitxer clau o una clau de maquinari (p. ex. YubiKey) ✔ |
| **Codi obert i auditories** | Totes les aplicacions són de codi obert. Hi ha auditories independents amb informes públics i un programa de recompenses per errors ✔ (segons Proton) | (*) No s'ha comprovat en aquest informe: cal consultar el web oficial | Llicència GPL v3 ✔. (*) Cal consultar el web oficial per saber si hi ha auditories recents |
| **Sincronització** | Sí, entre dispositius amb el mateix compte ✔ | Sí, entre dispositius amb el mateix compte ✔ | **No en té integrada** ✔. Es pot guardar el fitxer en un núvol de tercers, o fer servir KeeShare o ordres de pujada/baixada pròpies |
| **Accés sense connexió** | L'aplicació de Windows indica que té accés sense connexió ✔ (*) detalls segons la versió | (*) Segons l'aplicació; cal consultar la documentació | Sí, per naturalesa: el fitxer és local ✔ |
| **Aplicacions i extensions** | Aplicació de Windows i extensions per Chrome, Firefox, Edge, Safari i Brave ✔ (*) També hi ha aplicacions mòbils | (*) Aplicacions i extensions per a diverses plataformes; cal consultar la web | Windows, macOS i Linux ✔. Extensió KeePassXC-Browser per a navegadors basats en Chromium, Firefox i Edge ✔ |
| **Generador de contrasenyes** | Sí: mode «aleatori» i mode «memorable». Longitud (16 per defecte) i lletres majúscules, números i símbols ✔ | Sí, inclòs a tots els comptes, també el gratuït ✔ | Sí: contrasenyes aleatòries amb jocs de caràcters triats i frases de pas amb llistes de paraules ✔ |
| **Autocompletat** | Sí, amb l'extensió i les aplicacions ✔ | (*) Sí, amb extensió i apps; cal consultar els detalls | Sí: Auto-Type i extensió de navegador, amb suport de passkeys ✔ |
| **Còpia de seguretat i exportació** | Exportació a ZIP xifrat amb PGP, ZIP sense xifrar o CSV. No es pot exportar des de les apps mòbils ✔ | Exportació a `.csv`, `.json` i `.json` xifrat (restringit al compte o protegit amb contrasenya) ✔ | Còpia automàtica abans de cada desat i còpies a demanda ✔. El fitxer `.kdbx` ja és la còpia, però cal guardar-ne còpies |
| **Cost i pla gratuït** | Pla gratuït amb entrades i dispositius il·limitats ✔. (*) Plans de pagament i de negoci: cal veure la web oficial | Pla gratuït amb dispositius i contrasenyes il·limitats ✔. (*) Hi ha plans Premium, Families, Teams i Enterprise | Gratuït (programari lliure) ✔ |
| **Ús personal** | Molt adequat: fàcil i amb pla gratuït | Molt adequat | Adequat per a qui vol control total i no li fa res gestionar el fitxer |
| **Ús empresarial** | Hi ha Proton Pass for Business: consola d'administració, polítiques, compartició i registres. Funcions com SSO/SCIM depenen del pla ✔ (*) | Els plans Teams i Enterprise inclouen administració central, registres, polítiques i SSO segons el pla ✔ (*) | No té administració central. Cada persona gestiona el seu fitxer, i això complica el control a una empresa |
| **Avantatges principals** | Fàcil d'usar, xifrat d'extrem a extrem, codi obert, alies de correu i autenticador 2FA integrats | Molt complet, pla gratuït generós, plans d'empresa clars | Control total de les dades, sense dependre de cap servei, molt configurable |
| **Limitacions principals** | Depèn d'un servei extern i del compte Proton. Les funcions d'empresa depenen del pla | Depèn d'un servei extern. Les funcions d'empresa depenen del pla | Cal gestionar sincronització i còpies. Si es perd el fitxer o la contrasenya, no hi ha recuperació |

---

## 4. Models en línia i fora de línia

### 4.1. Gestors en línia (Proton Pass i Bitwarden)

**Avantatges**
- **Comoditat i sincronització**: les mateixes contrasenyes són a tots els dispositius sense fer res.
- **Ús en diversos dispositius**: ordinador, mòbil i navegador.
- **Funcions d'administració per a empreses**, en alguns plans: gestió d'usuaris, polítiques i registres.

**Inconvenients**
- **Depenen d'un servei** per sincronitzar. Si el servei falla, no es sincronitza fins que torna.
- **El compte és la clau de tot**. Si algú el compromet, pot accedir a la caixa forta. Per això és imprescindible protegir-lo amb una contrasenya llarga i única i activar la **MFA**.
- Les funcions d'empresa **depenen del pla** que es contracti.

Que les dades estiguin en servidors remots **no vol dir que siguin insegures**: en aquests gestors el que hi ha al servidor són dades xifrades al dispositiu de l'usuari. El que importa és com està dissenyat el xifrat i com es protegeix el compte.

### 4.2. Gestors fora de línia / locals (KeePassXC)

**Avantatges**
- **Control del fitxer**: les dades són a on l'usuari decideix.
- **No es necessita un servei de sincronització al núvol**.
- Es pot reforçar l'accés amb **fitxer clau** o **clau de maquinari**.

**Inconvenients**
- Cal gestionar **còpies de seguretat, versions i sincronització** a mà.
- Si es **perd el fitxer** o la **contrasenya mestra**, les dades es perden.
- Si **el dispositiu està compromès** (per exemple, amb programari maliciós), el fitxer també pot quedar exposat.
- Per a una empresa, controlar què fa cada empleat amb el seu fitxer és difícil.

Un gestor local **no és invulnerable**. Només canvia on són les dades i qui s'encarrega de protegir-les.

---

## 5. Recomanació per a EverPia

### 5.1. Decisió

Per a aquesta pràctica, l'eina escollida és **Proton Pass**.

### 5.2. Motius

Cal separar els fets de l'opinió personal. L'autor d'aquesta pràctica **ja fa servir Proton Pass habitualment i n'està content**. Això és una raó pràctica per triar-lo, però **no és una prova que sigui tècnicament millor** que les altres. Els motius que es poden defensar amb la documentació són:

- **Familiaritat i facilitat d'adopció.** Una eina que ja es coneix s'aprèn i s'adopta més ràpid, i això és clau si es vol que el personal la faci servir de veritat.
- **Disponibilitat.** Té aplicació de Windows i extensions per als navegadors principals (Chrome, Firefox, Edge, Safari i Brave).
- **Generació de contrasenyes i autocompletat**, integrats a l'aplicació i a l'extensió.
- **Sincronització còmoda** entre dispositius amb el mateix compte.
- **Exportació i còpia de seguretat**: permet exportar a un ZIP xifrat amb PGP, que és adequat per fer còpies.
- **Seguretat documentada**: xifrat d'extrem a extrem, codi obert i auditories públiques, segons Proton.
- **Pla gratuït** que permet fer la pràctica sense cost.

### 5.3. Comparació breu amb les altres opcions

- **Bitwarden** és una alternativa molt vàlida i del mateix tipus (al núvol). Té plans d'empresa ben definits. No s'ha triat perquè la familiaritat amb Proton Pass pesa més en aquesta pràctica, no perquè sigui pitjor.
- **KeePassXC** dona més control, però exigeix gestionar a mà sincronització i còpies, i no té administració central. Per a una empresa amb molt de personal, això fa més difícil controlar-ho.

### 5.4. Advertència per a un desplegament empresarial real

Aquesta tria és per a **l'ús individual de la pràctica**. Abans d'implantar el gestor a tota EverPia caldria **avaluar**:

- les funcions d'**administració centralitzada** i de **gestió d'usuaris** del pla disponible;
- la **recuperació de comptes**;
- la **compartició segura** de credencials entre equips;
- el **compliment normatiu** (per exemple, protecció de dades);
- el **pla comercial** adequat i el seu cost;
- les polítiques de **MFA**, **permisos** i **còpies de seguretat**.

Proton Pass té una versió per a empreses (Proton Pass for Business) amb consola d'administració i polítiques, però quines funcions s'inclouen depèn del pla. Per això cal mirar la web oficial i, si convé, comparar-ho amb Bitwarden Teams/Enterprise abans de decidir.

---

## 6. Mesures de seguretat complementàries

Un gestor de contrasenyes és només una part de la solució. EverPia hauria de fer també:

1. **Activar la MFA** a tots els comptes que ho permetin, sobretot correu, VPN, repositoris i el mateix gestor.
2. **Fer servir contrasenyes úniques** a cada servei.
3. **Canviar les credencials compromeses** tan bon punt es detecta l'incident, començant pel compte afectat i pels que compartien la contrasenya.
4. **Revocar sessions i tokens** actius (tokens d'API, claus, sessions obertes) quan correspongui.
5. **Formar el personal** per reconèixer el phishing.
6. **Mantenir actualitzats** sistemes operatius, navegadors i aplicacions.
7. **Aplicar el principi de mínim privilegi**: cadascú només ha de tenir els accessos que necessita.
8. **Tenir un procediment de resposta a incidents** escrit: qui avisa a qui, què es fa primer i com es comunica als clients.
9. **Protegir i provar les còpies de seguretat**: que estiguin xifrades, ben guardades i que es comprovi que es poden restaurar.

---

## 7. Conclusió

L'incident d'EverPia mostra que **una sola credencial feble o reutilitzada pot posar en risc un projecte sencer**. Per això cal millorar la gestió de credencials.

Un gestor de contrasenyes ajuda perquè **genera contrasenyes llargues i úniques**, les guarda de manera xifrada, les omple automàticament i evita que es reutilitzin. Ara bé, no ho soluciona tot: cal MFA, actualitzacions, formació i un pla de resposta a incidents.

Hem comparat Proton Pass, Bitwarden i KeePassXC. Cap no és «la millor» per a tothom: depèn de si es prefereix comoditat i sincronització (Proton Pass, Bitwarden) o control total del fitxer (KeePassXC). **Proton Pass s'ha escollit per a la pràctica sobretot per familiaritat, facilitat d'ús i perquè compleix les funcions que es demanen**. Aquesta elecció és personal i no demostra que sigui tècnicament superior.

Per a un desplegament real a EverPia, la decisió definitiva ha de tenir en compte els requisits de l'organització: administració central, recuperació de comptes, compartició, normativa i pla comercial.

---

## 8. Bibliografia i fonts

Consultades per fer aquest informe (comproveu-les abans d'entregar, perquè les webs canvien).

**Proton Pass**
- Descàrrega per a Windows: <https://proton.me/pass/download/windows>
- Seguretat de Proton Pass: <https://proton.me/pass/security>
- Exportar dades de Proton Pass: <https://proton.me/support/pass-export>
- Contrasenya addicional de Proton Pass: <https://proton.me/support/pass-extra-password>
- Generador de contrasenyes: <https://proton.me/pass/password-generator>
- Proton Pass for Business: <https://proton.me/business/pass>

**Bitwarden**
- Xifrat utilitzat: <https://bitwarden.com/help/what-encryption-is-used/>
- Exportar dades: <https://bitwarden.com/help/export-your-data/>
- Plans: <https://bitwarden.com/pricing/>

**KeePassXC**
- Guia d'usuari: <https://keepassxc.org/docs/KeePassXC_UserGuide>
- Descàrrega i web oficial: <https://keepassxc.org/>

**Altres**
- CISA, *Use Strong Passwords* (Secure Our World): <https://www.cisa.gov/secure-our-world/use-strong-passwords> (aquesta pàgina no s'ha pogut obrir en el moment de fer l'informe; llegiu-la abans de citar-la)
- OWASP, *Authentication Cheat Sheet*: <https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html>
