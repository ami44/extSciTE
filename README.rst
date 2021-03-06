.. -*- coding: utf-8 -

Introduction
=============================

`SciTE <http://www.scintilla.org/SciTE.html>`_ est un éditeur multiplatforme léger et hautement configurable qui fonctionne sous Linux/Win/Mac (contrairement à notepad++).
Ma productivité acquise sous Linux est conservée quand je passe sur Windows.

J'utilise ``extSciTE`` comme mémo pour réinstaller cet éditeur sur une nouvelle machine
et rapidement ajouter quelques scripts très pratiques pour un usage quotidien.

``extSciTE`` a pour vocation de fonctionner avec le programme ``SciTE`` standard.

extSciTE
=============================


Installation sous Linux
--------------------------------------------

- ``sudo apt-get install git``
- ``sudo apt-get install lua5.1``
- ``sudo apt-get install liblua5.1-socket2`` (ou ``sudo apt-get install lua-socket``)
- ``sudo apt-get install liblua5.1-socket-dev``
- ``sudo apt-get install lua5.1-filesystem``
- ``sudo apt-get install lua5.1-sql-sqlite3`` ( ou ``sudo apt-get install lua5.1-sql-sqlite3-2`` selon version)


- Lancer la console et exécuter la commande ``lua`` : un prompt s'affiche invitant à saisir du code lua. Tester le code ``socket = require "socket";print(socket._VERSION);`` sans génèrer une erreur dans la console.
- ``sudo apt-get install scite``
- Télécharger https://github.com/ami44/extSciTE.git dans ``/home/myloginname/extSciTE`` : ``cd /home/myloginname && git clone https://github.com/ami44/extSciTE.git``
- Exécuter SciTE :

    - Editer ``.SciTEUser.properties`` (menu --> Options --> Open User Options File) pour ajouter des options spécifiques à ``extman.lua`` : ::

            -- -*- coding: utf-8 -
            # ------------------------------------------------------------------------
            # extman.lua
            # ------------------------------------------------------------------------
            # Indiquer à ``extman.lua`` où se trouve les scripts lua à charger
            ext.lua.directory=/home/myloginname/extSciTE/extman/scite_lua
            # si on modifie le fichier ``SciTEStartup.lua`` alors SciTE le relancera automatique ( cf aussi SHIFT+CTRL+R)
            ext.lua.auto.reload=1
            # option utile pour le développement
            ext.lua.debug.traceback=1
            # (facultatif) Emplacement spécifique du script de chargement si on n'utilise pas le fichier ``SciTEStartup.lua`` par défaut
            #~ ext.lua.startup.script=$(SciteUserHome)/SciTEStartup.lua

    - Editer le script de chargement principal ``SciTEStartup.lua`` (menu --> Options --> Open Lua Startup Scripts) et ajouter ::

        -- -*- coding: utf-8 -
        -- -------------------------------------------------------------------------------------------------------
        -- Extension
        -- charger les extensions *avant* d'exécuter extman.lua
        -- -------------------------------------------------------------------------------------------------------
        -- corriger le chemin des extensions (à revoir)
        -- OLD UBUNTU : package.path = package.path..';/usr/share/lua/5.1/?.lua'
        -- OLD UBUNTU : package.cpath = package.path..';/usr/lib/lua/5.1/?.so'
        package.path = package.path..';/usr/lib/x86_64-linux-gnu/lua/5.1/?.lua;/usr/share/lua/5.1/?.lua'
        package.cpath = package.path..';/usr/lib/x86_64-linux-gnu/lua/5.1/?.so;/usr/share/lua/5.1/?.so'
        -- charger les extensions
        io = require "io";
        socket = require "socket";
        mime = require "mime";
        ltn12 = require "ltn12";
        http = require "socket.http";
        url = require "socket.url";
        ftp = require "socket.ftp";
        tp = require "socket.tp";
        lfs = require "lfs";
        require "luasql.sqlite3";


        -- -------------------------------------------------------------------------------------------------------
        -- Si SciTE installé avec apt-get alors ``ext.lua.directory`` retourne ``$HOME/scite_lua``
        -- DONC CORRIGER EMPLACEMENT POUR ExtSciTE
        -- -------------------------------------------------------------------------------------------------------
        path = '/home/myloginname/extSciTE/extman/scite_lua'
        props['ext.lua.directory'] = path

        -- -------------------------------------------------------------------------------------------------------
        -- extman.lua
        -- ce script exécutera ensuite les scripts présents dans le répertoire extman/scite_lua
        -- -------------------------------------------------------------------------------------------------------
        dofile "/home/myloginname/extSciTE/extman/extman.lua"

- Redémarrer SciTE et constater dans la console que les modules de extSciTE se chargent :

    .. image:: https://github.com/ami44/extSciTE/raw/master/assets/startup.png
        :alt: chargement des modules extSciTE
        :align: center

- installer/démarrer nodeSciTE (cf ci-dessous)

- suite : extScite est la base pour ensuite utiliser scipm (``cd /mount/usb/myScipm && scipm build``)

Installation sous Windows
--------------------------------------------

- Installer luaforwindows : http://code.google.com/p/luaforwindows/ (inclus luarock) et s'assurer que lua51.dll est visible dans %PATH%
- Installer tree (dans le %PATH%) : http://gnuwin32.sourceforge.net/packages/tree.htm
- Lancer la console et exécuter la commande ``lua`` : un prompt s'affiche invitant à saisir du code lua. Tester le code ``socket = require "socket";print(socket._VERSION);`` sans génèrer une erreur dans la console. Si erreur alors corriger les variables d'environnement :

    - Windows XP : Démarrer --> Panneau de configuration --> Système --> Avancé --> Variables d'environnement --> Variables système
    - LUA_CPATH = ``C:\Program Files\Lua\5.1\clibs\?.dll``
    - LUA_PATH = ``;C:\Program Files\Lua\5.1\lua\?.luac;C:\Program Files\Lua\5.1\lua\?.lua``

- (info) installer les modules complémentaires via luarocks
    - si proxy : ``set http_proxy=http://login:mdp@server:1234``
    - ``luarocks install luaxxxxxx``

- Installer SciTE ( http://www.scintilla.org/SciTEDownload.html ) dans le répertoire ``C:\Documents and Settings\myloginname\wscite`` et ajouter le raccourci de SciTE sur le bureau + barre de lancement rapide (explorer --> SciTE.exe --> clic droit --> Envoyer vers --> Bureau (créer un raccourci))
- Télécharger https://github.com/ami44/extSciTE.git dans ``C:\Documents and Settings\myloginname\extSciTE`` (télécharger le zip)
- Exécuter SciTE :

    - Editer ``SciTEUser.properties`` (menu --> Options --> Open User Options File) pour ajouter des options spécifiques à ``extman.lua`` : ::

            -- -*- coding: utf-8 -
            # ------------------------------------------------------------------------
            # extman.lua
            # ------------------------------------------------------------------------
            # Indiquer à ``extman.lua`` où se trouve les scripts lua à charger
            ext.lua.directory=C:\Documents and Settings\myloginname\extSciTE\extman\scite_lua
            # si on modifie le fichier ``SciTEStartup.lua`` alors SciTE le relancera automatique ( cf aussi SHIFT+CTRL+R)
            ext.lua.auto.reload=1
            # option utile pour le développement
            ext.lua.debug.traceback=1
            # (facultatif) Emplacement spécifique du script de chargement si on n'utilise pas le fichier ``SciTEStartup.lua`` par défaut
            #~ ext.lua.startup.script=$(SciteUserHome)/SciTEStartup.lua

    ..
        - ? ::

            #ext.lua.reset=1

    - Editer le script de chargement principal ``SciTEStartup.lua`` (menu --> Options --> Open Lua Startup Scripts) et ajouter ::

        -- -*- coding: utf-8 -
        -- -------------------------------------------------------------------------------------------------------
        -- Extension
        -- charger les extensions *avant* d'exécuter extman.lua
        -- -------------------------------------------------------------------------------------------------------
        io = require "io";
        socket = require "socket";
        mime = require "mime";
        ltn12 = require "ltn12";
        http = require "socket.http";
        url = require "socket.url";
        ftp = require "socket.ftp";
        tp = require "socket.tp";
        lfs = require "lfs";
        require "luasql.sqlite3";

        -- -------------------------------------------------------------------------------------------------------
        -- extman.lua
        -- ce script exécutera ensuite les scripts présents dans le répertoire extman/scite_lua
        -- -------------------------------------------------------------------------------------------------------
        dofile "C:\\Documents and Settings\\myloginname\\extSciTE\\extman\\extman.lua"


- Redémarrer SciTE et constater dans la console que les modules de extSciTE se chargent :

    .. image:: https://github.com/ami44/extSciTE/raw/master/assets/startup.png
        :alt: chargement des modules extSciTE
        :align: center

Si la console SciTE indique des problèmes avec lua, la solution radicale est la suivante :

        - Copier tous les fichiers du répertoire ``C:\PathToLua\clibs\*`` dans ``wscite``.
        - Copier le répertoire de ``C:\PathToLua\lua5.1.dll`` et ``C:\PathToLua\lua51.dll`` dans ``wscite``.
        - Copier le répertoire de ``C:\PathToLua\lua`` dans ``wscite``.
        - Redémarrer SciTE


- installer/démarrer nodeSciTE (cf ci-dessous)

- suite : extScite est la base pour ensuite utiliser scipm (``cd F:\myScipm && scipm build``)

Lua Startup Scripts
--------------------------------------------

Emplacement du script ``SciTEStartup.lua`` : menu --> Options --> Open Lua Startup Scripts

Le script ``SciTEStartup.lua`` est exécuté au démarrage de SciTE.
On exécute tout de suite le script ``extman.lua`` (http://lua-users.org/wiki/SciteExtMan) qui étend les
fonctionnalités lua de SciTE. J'ai amélioré ``extman.lua`` en ajoutant la méthode ``scite_OnKey()``.

Le script ``extman.lua`` se charge ensuite d'exécuter les scripts présents dans
le répertoire extSciTE/extman/scite_lua (cf option ``ext.lua.directory``). Il ajoute aussi un raccourci clavier
SHIFT+CTRL+R qui permet de recharger le script lua en cours d'édition (Cf menu --> Tools --> Reload Script ).
Si on édite le fichier ``SciTEStartup.lua`` alors on relancera ``extman.lua`` et les autres scripts en cascade.


nodeSciTE
------------------------------------------------------


.. note:: nodeSciTE n'analyse que les scripts ``*.js`` pour le moment

Compagnon de SciTE en charge d'analyser le code en cours d'édition (jslint...)


Installation de nodeSciTE
.............................................................

- installer ``extSciTE`` au préalable
- installer nodejs & npm : http://nodejs.org/download :

    - Linux :

        - sudo ``apt-get install nodejs``

    - Windows :

        - si administrateur : télécharger node-vX.Y-x86.msi (installe node et npm en même temps)
        - si non-administrateur (si échec avec msi) , il faut installer node puis npm séparément :

            - installer node :

                - télécharger node.exe depuis http://nodejs.org/download dans ``C:\Documents and Settings\myloginname\node``
                - mettre à jour la variable d'environnement PATH vers ``C:\Documents and Settings\myloginname\node``
                - dans la console tester ``node -v``

            - installer npm ( https://npmjs.org/doc/README.html) :

                - télécharger fichier npm-X.Y.Z.zip à cette adresse http://npmjs.org/dist/
                - extraire le contenu dans ``C:\Documents and Settings\myloginname\node``
                - dans la console tester ``npm -v``


- ouvrir la console
- linux : ``cd "/home/myloginname/extSciTE/nodeSciTE"``
- windows : ``cd "C:\Documents and Settings\myloginname\extSciTE\nodeSciTE"``
- ``npm install``
- @revoir : ne fonctionne pas !!! corriger ``extSciTE\nodeSciTE\node_modules\jslint\lib\jslint.js`` et corriger ``maxerr    : 1000`` en ``maxerr    : 10000``
- exécuter nodeSciTE (lire ci-après)

Exécution de nodeSciTE (manuel ou automatique)
.....................................................................

Manuel :

    - linux :

        - ouvrir la console bash
        - ``cd "/home/myloginname/extSciTE/nodeSciTE"``
        - ``node nodeSciTE.js``

    - windows :

        - ouvrir la console ``cmd``
        - ``cd "C:\Documents and Settings\myloginname\extSciTE\nodeSciTE"``
        - ``nodeSciTE.bat`` ou ``node nodeSciTE.js``


Automatique, Lancer le serveur nodeSciTE au démarrage de votre session :

    - windows : @todo
    - linux : @todo

    ..
        windows ? ajouter un fichier dans ``C:\Documents and Settings\myloginname\Menu Démarrer\Programmes\Démarrage\``

Corriger le port de nodeSciTE
.............................................................

Le serveur nodeSciTE écoute par défaut le port 3891 en local.

Si on corrige en dur le port dans le fichier ``extSciTE/nodeSciTE/nodeSciTE.js`` ou que ce service est sur un autre serveur, alors éditer le fichier ``SciTEUser.properties`` (menu --> Options --> Open User Options File) et ajouter ces options : ::

    # ------------------------------------------------------------------------
    # nodeSciTE
    # ------------------------------------------------------------------------
    extscite.nodeSciTE.host=http://127.0.0.1
    extscite.nodeSciTE.port=9999


SciTE
=============================


liens utiles :

    - http://ensiwiki.ensimag.fr/index.php/SciTE
    - http://ensiwiki.ensimag.fr/index.php/Configuration_De_SciTE
    - http://www.cloudconnected.fr/2005/11/11/scite-l-editeur-indispensable/
    - http://www.distasis.com/cpp/scitetip.htm
    - http://www.scintilla.org/SciTEDoc.html
    - http://www.scintilla.org/SciTELua.html
    - https://code.google.com/p/scite-files/w/list
    - http://pgl.yoyo.org/scite/bits/SciTEGlobal.properties
    - http://lua-users.org/wiki/FindPage (chercher ``scite``)
    - http://www.scintilla.org/ScintillaDoc.html
    - http://scite-files.googlecode.com/svn-history/trunk/extras/SciTELua.api
    - http://www.scintilla.org/PaneAPI.html (api scintilla)
    - http://scite-ru.googlecode.com/svn/trunk/pack/tools/ (plein d'idées lua)
    - https://groups.google.com/forum/?fromgroups#!forum/scite-interest



Editer ``SciTEUser.properties`` (menu --> Options --> Open User Options File) : ::


    buffers=30
    save.session=1
    check.if.already.open=1
    open.dialog.in.file.directory=1
    find.replace.advanced=1
    code.page=65001
    output.code.page=65001
    properties.directory.enable=1
    title.full.path=1
    title.show.buffers=1
    pathbar.visible=1
    save.position=1
    line.margin.visible=1
    highlight.current.word=1
    find.files=*
    tabsize=4
    indent.size=4
    use.tabs=0
    strip.trailing.spaces=1

    # The load.on.activate property causes SciTE to check whether the current file has been updated by another process whenever it is activated. This is useful when another editor such as a WYSIWYG HTML editor, is being used in conjunction with SciTE.
    # When both this and load.on.activate are set to 1, SciTE will ask if you really want to reload the modified file, giving you the chance to keep the file as it is. By default this property is disabled, causing SciTE to reload the file without bothering you.
    load.on.activate=1
    are.you.sure.on.reload=1

    # http://www.cloudconnected.fr/2005/11/11/scite-l-editeur-indispensable/
    # Par défaut, les touches Home et End déplacent le curseur au début et à la fin de la ligne logique. Pour changer se comportement afin qu’elles déplacent le curseur sur la ligne visuelle, c’est la propriété :
    wrap.aware.home.end.keys=1

    if PLAT_GTK
        all.files=All Files (*)|*|Hidden Files (.*)|.*|
    open.filter=\
    $(all.files)

    # charger le fichier markdown.properties
    # https://groups.google.com/forum/?fromgroups=#!topic/scite-interest/MZFRd161I6Y
    # https://github.com/vadmium/etc/blob/master/scite/markdown.properties
    import markdown

    # Status Bar
    # $(StatusMsg) est utilisé par les modules pour afficher des alertes diverses ( props["StatusMsg"]='Mon message'; )
    # impérativement maintenir $(LineNumber) et $(ColumnNumber) dans statusbar.text.1 pour aider à la mise à jour du texte dans $(StatusMsg)
    # rappel : seul window autorise statusbar.number > 1 (cliquer sur le status pour passer d'un texte à l'autre) mais pas très utile
    statusbar.visible=1
    statusbar.number=1
    statusbar.text.1=li=$(LineNumber) co=$(ColumnNumber) $(StatusMsg)

Facultatif : installer la police consolas pour la console :

    - prélalble : vérifier que la poilice n'est pas déjà présente dans C:\Windows\Fonts
    - télécharger la font consolas (`ici <http://www.fontpalace.com/font-download/Consolas/>`_ ou `là <https://code.google.com/p/scite-ru/downloads/detail?name=Consolas.rar&can=2&q=>`_ ) . Si win et problème de droit alors copier directement dans ``C:\Windows\Fonts``.
    - Editer le fichier ``SciTEGlobal.properties`` et corriger ``font.small=font:Verdana,size:8`` par ``font.small=font:Consolas,size:8``.
    - Le ``$(font.small)`` sera ensuite interprété par le fichier ``others.properties``

Todo :

    - tester scintillua ( http://foicica.com/scintillua/download, http://foicica.com/scintillua/api/lexer.html#Styling.Tokens )
    - implanter http://scite-ru.googlecode.com/svn/trunk/pack/tools/svn_menu.lua (et voir les autres scripts lua)
    - revoir 016outputcolor.lua ( http://lua-users.org/wiki/SciteColouriseDemo)
    - notify-send ( win : http://rodnic.net/notify-send/, linux : sudo apt-get install libnotify-bin )


Modules extSciTE
=============================

.. note:: pour désactiver un module : simplement renommer le fichier sans l'extension ``.lua`` pour ne plus être pris en compte. Pour le réactiver : remettre l'extension ``.lua``.

extSciTE/extman/scite_lua/001first.lua
--------------------------------------------

Indique que extSciTE est bien chargé

extSciTE/extman/scite_lua/015utils.lua
--------------------------------------------

- ``function luasqlrows (connection, sql_statement)`` utilisé par ``030bookmark.lua``.
- ``function vardump(value, depth, key)``


extSciTE/extman/scite_lua/020execlua.lua
--------------------------------------------

Permet d'éxécuter code lua présent dans la console.
Utilisé par 030bookmark.lua et 040dir.lua.

extSciTE/extman/scite_lua/030bookmark.lua
--------------------------------------------

.. image:: https://github.com/ami44/extSciTE/raw/master/assets/bookmark.png
    :alt: exemple bookmarks
    :align: center

CTRL+b : affiche les bookmarks dans la console SciTE :

    - fichiers ou répertoires préférés ( on peut même définir la ligne à afficher : utile pour descendre à la dernière ligne du fichier apache2/access.log par exemple : initialiser alors à 10000000000 )
    - code lua à exécuter ( afficher un message, fonction à lancer ... )

Pour aérer les bookmark, il y a aussi possibilité d'affichers des séparateurs.

Rappel : CTRL+B replace le comportement CTRL+B (Expand abbreviation) par défaut de Scite.


Bookmark & sqlite3
....................................................................

Pour créer/éditer la base  sqlite3 : télécharger l'outil sqliteStudio à cette adresse http://sqlitestudio.one.pl

Avec sqliteStudio, créer une base de données dans ``C:\Documents and Settings\myloginname\bookmark.sqlite3.db`` puis créer la table ``bookmark`` avec cette commande sql ::

    CREATE TABLE bookmark (
            id           INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
            label        TEXT    NOT NULL,
            FilePath     TEXT    NULL,
            FilePathLine INTEGER NOT NULL DEFAULT '0',
            doStringCode TEXT    NULL,
            isSep        INTEGER NOT NULL DEFAULT '0',
            ordre        INTEGER NOT NULL
    );

Ajouter des bookmarks dans la table bookmark

Pour définir la base de données sqlite3, éditer ``SciTEUser.properties`` (menu --> Options --> Open User Options File) : ::

    # ------------------------------------------------------------------------
    # bookmark
    # ------------------------------------------------------------------------
    extscite.bookmark.sqlite3=C:\Documents and Settings\myloginname\bookmark.sqlite3.db


Pour éditer la base de données sqlite3 via une interface web comme adminer (http://www.adminer.org/) préalablement installé sur votre poste de travail ``SciTEUser.properties`` (menu --> Options --> Open User Options File) : ::

    # ------------------------------------------------------------------------
    # bookmark
    # ------------------------------------------------------------------------
    extscite.bookmark.sqlite3=C:\Documents and Settings\myloginname\bookmark.sqlite3.db
    extscite.bookmark.navigateur="C:\\Documents and Settings\\myloginname\\Mes Programmes\\ChromiumPortable\\ChromiumPortable.exe"
    extscite.bookmark.http=http://127.0.0.1/adminer/?sqlite=&username=root&db=D%3A%5CUtilisateurs%5Cmyloginname%5Cbookmark.sqlite3.db&select=bookmark&modify=1&order%5B0%5D=ordre




extSciTE/extman/scite_lua/040dir.lua
--------------------------------------------

Affiche dans la console SciTE tous les fichiers depuis le répertoire du fichier courant.


.. image:: https://github.com/ami44/extSciTE/raw/master/assets/dir.png
    :alt: affichage dir dans la console
    :align: center

Usage depuis un fichier ouvert dans SciTE : CTRL+SHIFT+o

Usage depuis le module bookmark :

    Créer un bookmark (cf section 030bookmark.lua ci-dessus) et
    dans la colonne ``doStringCode`` appeller la fonction ``printListFileInDirCommun('C:\\BitNami\\wappstack-5.4.9-0\\apache2\\htdocs\\qcm')``

extSciTE/extman/scite_lua/042project.lua (tree)
-------------------------------------------------------------

Affiche les dossiers et fichiers présent depuis un répertoire sous forme d'arborescence. Nécessite l'utilitaire ``tree`` dans le path (installé par défaut sous linux, à installer sous windows : http://gnuwin32.sourceforge.net/packages/tree.htm).

.. image:: https://github.com/ami44/extSciTE/raw/master/assets/tree.png
    :alt: affichage tree dans la console
    :align: center

Usage depuis un fichier ouvert dans SciTE : Ctrl+Shift+T

Depuis le module bookmark :

    Créer un bookmark (cf section 030bookmark.lua ci-dessus) et
    dans la colonne ``doStringCode`` appeller la fonction ``printTree('C:\\BitNami\\wappstack-5.4.9-0\\apache2\\htdocs\\qcm')`` ou avec des options comme ``printTree('C:\\BitNami\\wappstack-5.4.9-0\\apache2\\htdocs\\qcm', '-a -L 2')`` (-a == fichier hidden, -L 2== afficher 2 niveaux uniquement, cf options de tree http://www.computerhope.com/unix/tree.htm ou http://linux.die.net/man/1/tree )

extSciTE/extman/scite_lua/043fileinfo.lua
--------------------------------------------

CTRL+SHIFT+i : affiche dans la console SciTE les infos du fichiers pour copier/coller

.. image:: https://github.com/ami44/extSciTE/raw/master/assets/fileinfo.png
    :alt: affichage info dans la console
    :align: center


extSciTE/extman/scite_lua/52outputToEditor.lua
------------------------------------------------------------------------------------------------

CTRL+7 : copier le contenu de la console dans un fichier ``console.txt`` et ouvrir ce fichier dans SciTE.

extSciTE/extman/scite_lua/53openexplorer.lua
--------------------------------------------

CTRL+6 : ouvrir l'explorateur de fichier

extSciTE/extman/scite_lua/100tictacto.lua
--------------------------------------------

CTRL+8 : A utiliser avec font monospace (CTRL+F11)

.. image:: https://github.com/ami44/extSciTE/raw/master/assets/tictactoe.png
    :alt: jouer avec tictactoe
    :align: center

source : http://lua-users.org/wiki/SciteTicTacToe

extSciTE/extman/scite_lua/101eliza.lua
--------------------------------------------

CTRL+9 : crazy elisa

source : http://lua-users.org/wiki/SciteElizaClassic

extSciTE/extman/scite_lua/103asciitable.lua
--------------------------------------------

Afficher tous les caractères spéciaux pour faciliter copier/coller

Usage : Menu > Tools > ASCII table


.. image:: https://github.com/ami44/extSciTE/raw/master/assets/asciitable.png
    :alt: afficher ascii table
    :align: center

source : http://lua-users.org/wiki/SciteAsciiTable



extSciTE/extman/scite_lua/800node.lua
--------------------------------------------

.. note:: version alpha, très instable ;-)

Scite envoie le contenu du code à analyser au serveur nodeSciTE ( jslint, etc ... ).
Afficher le résultat sous forme d'annotation dans SciTE :

    .. image:: https://github.com/ami44/extSciTE/raw/master/assets/nodescite.png
        :alt: chargement des modules extSciTE
        :align: center

Voir la section ci-dessus nodeSciTE pour installer et démarrer ce serveur.



Enjoy !
