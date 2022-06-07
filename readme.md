<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html>
<head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8"/>
    <title>Projekt IIS</title>
    <style type="text/css">
    	table { border-collapse: collapse; }
    	td, th { border: 1px solid black; padding: 0.3em 0.5em; text-align: left; }
    	dt { font-weight: bold; margin-top: 0.5em; }
    </style>
</head>
<body>

<h1>Zadání č. 20 – Zkoušky (IUS)</h1> 

<dl>
	<dt>Autoři</dt>
	<dd>Petr Buchal
	    <a href="mailto:xautor01@stud.fit.vutbr.cz">xbucha02@stud.fit.vutbr.cz</a> -
		správa uživatelů, databázový systém, uživatelské rozhraní
	</dd>
	<dd>Tomáš Holík
	    <a href="mailto:xautor02@stud.fit.vutbr.cz">xholik13@stud.fit.vutbr.cz</a> - 
		správa uživatelů, databázový systém, uživatelské rozhraní
	</dd>
	<dt>URL aplikace</dt>
	<dd><a href="http://ec2-35-177-81-71.eu-west-2.compute.amazonaws.com/">http://ec2-35-177-81-71.eu-west-2.compute.amazonaws.com/</a></dd>
	<dd><a href="http://35.177.81.71/">http://35.177.81.71/</a></dd>
</dl>

<h2>Uživatelé systému pro testování</h2>
<table>
<tr><th>Login</th><th>Heslo</th><th>Role</th></tr>
<tr><td>xdvora00</td><td>xdvora00</td><td>Vyučující</td></tr>
<tr><td>xnovak10</td><td>xnovak10</td><td>Student</td></tr>
</table>
<p>Každý předmět má svého garanta, který má větší pravomoce než vyučující. Roli garanta při autentizaci však náš program nenabízí, protože jeden vyučující může učit více předmětů a pokud je v jednom předmětu garant a ve druhém není, nejdou jeho pravomoce obecně určit pomocí role. Pravomoce jsou tak stanoveny až při autorizaci na jednotlivých stránkách předmětů podle příznaku vyučujícího, zda-li je nebo není garant daného předmětu.</p>
<p>Roli administrátora nenalezenete v informačním systému jako takovém. Administrátor pracuje s celou databází pomocí webové stránky  <a href="http://35.177.81.71/phpmyadmin">http://35.177.81.71/phpmyadmin/</a>. Tato stránka má dvojí autentizaci: </p>
<table>
<tr><th></th><th>Login</th><th>Heslo</th></tr>
<tr><td>pop up</td><td>tomas</td><td>heslo</td></tr>
<tr><td>phpmyadmin</td><td>root</td><td>heslo</td></tr>
</table>
<p>Pro připojení k databázi z externího zdroje (IDE) je nutná jiná kombinace.</p>
<table>
    <tr><th></th><th>Login</th><th>Heslo</th><th>Host</th></tr>
    <tr><td>iisprojekt oprávnění</td><td>tomas</td><td>heslo</td><td>http://ec2-35-177-81-71.eu-west-2.compute.amazonaws.com/</td></tr>
</table>
<h2>Implementace</h2>
<p>K implementaci projektu jsme využili PHP framework Laravel verzi 5.5.22. Námi vytvořený informační systém je postaven na architektuře MVC (model-view-contoler). Tato architektura rozděluje aplikaci do tří nezávislých částí tak, že modifikace kterékoli z nich má jen minimální vliv na ostatní.</p>
<h3>Modely</h3>
<p>Modely se nachází ve složce '/app'. Pro každou entitní množinu se zde nachází jeden model, který je pojmenován shodně s názvem entitní množiny. Vyjímkou je implementace modelu person, ten je řešen v modelu '/app/User.php', kvůli jeho využití jako modelu pro správu uživatelů informačního systému.</p>
<h3>Kontrolery</h3>
<p>Kontrolery se nachází ve složce '/app/Http/Controllers'. Pro každý model se zde nachází jeden kontroler, který obsahuje funkce, kterými zprostředkovává data pro jednotlivé pohledy. Kontroler '/app/Http/Controllers/SessionsController.php' se stará o autorizaci a autentifikaci uživatelů.</p>
<h3>Pohledy</h3>
<p>Pohledy se nachází ve složce '/resources/views'. Počet pohledů neodpovídá ani počtu modelů, ani počtu kontrolerů, jejich počet odpovídá počtu stránek, které využívá náš informační systém. K implementaci jednotlivých stránek využíváme prostředku blade, který nám usnadňuje hromadné úpravy uživatelského rozhraní bez nutnosti průchodu všech upravovaných souborů. Vazby mezi jednotlivými stránkami jsou zprostředkovány přes cesty (Routes) v souboru '/routes/web.php'.</p>
<h3>Middleware</h3>
<p>Což je lepidlo mezi jednotlivými částmi MVC. Zjednodušuje komunikaci a input-output. Nachází se ve složce 'app/Http/Middleware'. V middlewaru kontrolujeme role uživatelů - Student, Garant a Učitel. Pomocí něho pak určité stránky nemohou zpřístupnit nebo je nevidí. Kromě rolí se v middlewaru implementuje samotná autentizace a automatické odhlašování.</p>
<h2>Instalace</h2>
<p>Softwarové požadavky naleznete v souboru 'composer.json'.</p>
<h3>Postup instalace</h3>
<ol>
<li>Nainstalujte program Composer.</li>
<li>V příkazové řádce vytvořte příkazem 'composer create-project laravel/laravel app' nový projekt frameworku laravel.</li>
<li>Zkopírujte a přepište soubory vytvořeného projektu soubory z archivu obsahujícího náš informační systém. V archivu jsou všechny soubory až na složku vendor, který obsahuje veškeré frameworky a addony a složky node_modules.</li>
</ol>
<p>Připojte se k MYSQL databázi se jménem, heslem a hostem v IDE. Skript SQL iisprojekt.sql vložte do Vámi preferovaného IDE pro databáze a spusťte.</p>
</body>
</html>